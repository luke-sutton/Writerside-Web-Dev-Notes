# Client and Server Side Validation

## Adding Client Side Validation

Client side validation is validation that happens in the browser.

If we go to the create diary entry page in our application and attempt to submit a form we will see several error
messages. This is because in our DiaryEntry model we have set the fields to be non-nullable by using the required
attribute

```C#
public class DiaryEntry
{
    public int Id { get; set; }
    [Required]
    public string Title { get; set; } = string.Empty;
    [Required]
    public string Content { get; set; } = string.Empty;
    [Required]
    public DateTime Created { get; set; } = DateTime.Now;
}
```

So we should implement validation to prevent this error screen.

We can do this very easily in by just adding the following to the bottom of our Create.cshtml view:

```Razor
@section Scripts{
    <partial name="_ValidationScriptsPartial" />
}
```

And that's it! Now if we go back and try to submit the form instead of seeing the ugly error page, the first empty field
will be highlighted as soon as we press create.

## Adding Client Side Validation Summary

We should now inform the user why there form is currently not valid. One way to do that is with a validation summary.

Again in .net this is very easy to implement by adding a new div above our form with a tag helper called
asp-validation-summary

```Razor
<div asp-validation-summary="All" class="text-danger"></div>
```

Now when we try to submit our form a list of the requirements not currently met will be displayed (e.g. the title
field is required)

## Adding Client Side Individual Validations

Rather than the summary that displays all the errors together we can use a different tag helper called
asp-validation-for to show just one.   
This can be used to display an error directly below the relevant input.

```Razor
<div class="form-group mb-3">
    <label asp-for="Title" class="form-label">Title</label>
    <input asp-for="Title" class="form-control" placeholder="Enter a title"/>
    <span asp-validation-for="Title" class="text-danger"></span>
</div>
```

In the example above adding the tag helper within a span beneath the input and stating that it is for the Title will
show the validation for that input directly below it.

## Amending the Validation Message

If you wish to change the validation message that is displayed this can be done within the model.   

Add parenthesis after the word required in the property attribute, pass in the property `ErrorMessage` and set it to a
string with your chosen message

```C#
[Required(ErrorMessage = "Please enter a title!")]
public string Title { get; set; } = string.Empty;
```

Other validations can be added here as well. For example, we can set a string length requirement using the 
`StringLength` attribute

```C#
[Required(ErrorMessage = "Please enter a title!")]
[StringLength(100, MinimumLength = 3, ErrorMessage = "Title must be between 3 and 100 characters.")]
public string Title { get; set; } = string.Empty;
```

## Adding Server Side Validation

The client side validation covers most of the validation needs we require. But, we also need to make sure that no
one can bypass our client and enter erroneous data and corrupt out database.

We can do this within our controller by using the `ModelState` property which is part of the controller base class

```C#
[HttpPost]
public IActionResult Create(DiaryEntry objDiaryEntry)
{
    if (objDiaryEntry.Title.Length < 3)
    {
        ModelState.AddModelError("Title", "Title is too short");
    }

    if (ModelState.IsValid)
    {
        _db.DiaryEntries.Add(objDiaryEntry);
        _db.SaveChanges();
        
        return RedirectToAction("Index");
    }
    
    return View(objDiaryEntry);
}
```

In the if statement here we are checking if the title property of the submitted object is too short. If it is, 
the `AddModelError` method is called on `ModelState` and we pass in the name of the field that we want to attach
the error too, and then the error wording.

The next if statement checks if the model is valid and our previous code to update the database is moved here.

Outside of this a new return statement is added to return the existing view along with the supplied object. This will
reload the form the user is currently on, with there entered data and display the error we have entered.

Note:
: To see this in action you would need to disable the client side validation.

Below is a refactored version with the validation moved to its own function:

```C#
[HttpPost]
public IActionResult Create(DiaryEntry diaryEntry)
{
    ValidateTitleLength(diaryEntry);

    if (ModelState.IsValid)
    {
        _db.DiaryEntries.Add(diaryEntry);
        _db.SaveChanges();
        return RedirectToAction("Index");
    }
    return View(diaryEntry);
}

private void ValidateTitleLength(DiaryEntry diaryEntry)
{
    if (diaryEntry.Title.Length < 3)
    {
        ModelState.AddModelError("Title", "Title is too short");
    }
}
```
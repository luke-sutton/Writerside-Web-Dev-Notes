# CRUD - Update and Delete

Now we will add the final CRUD operations, which are Update and Delete, to complete the basic functionality of 
our application.

## Add the Update and Delete Buttons

In the index page for the diary entries view, in the last table data row within out table body, remove the temporary
text we added of update and delete.

Replace this with 2 anchor tags with tag helpers of asp-controller and asp-action.   
add the value of `DiaryEntires` for the controller and leave the action blank for now.   
Enter the text Edit and Delete for the anchor tags and add some bootstrap icons as we have before.   
Finally add bootstrap classes to make them look more like buttons.

```Razor
<td>
    <a class="btn btn-primary btn-sm" asp-controller="DiaryEntries" asp-action="">
        <i class="bi bi-pencil"></i> Edit
    </a>
    <a class="btn btn-danger btn-sm" asp-controller="DiaryEntries" asp-action="">
        <i class="bi bi-trash3"></i> Delete
    </a>
</td>
```

## 'Update' View and Controller Action

The edit view will be almost identical to our create view, so create a new file called edit.cshtml and then copy
over the contents from the create view.   
The only details that need to be changed are the header should say Edit View Page instead of create, the form asp-action
should be Edit, and the button should say Edit instead of create.

```Razor
@model DiaryEntry

<div class="container mt-4">
    <div class="row">
        <div class="col-md-8 offset-md-2">
            <div class="card">
                <div class="card-header bg-primary text-white">
                    <h2>Edit Diary Entry</h2>
                </div>
                <div class="card-body">
                    <form asp-action="Edit" method="post">
                        <div asp-validation-summary="All" class="text-danger"></div>
                        <div class="form-group mb-3">
                            <label asp-for="Title" class="form-label">Title</label>
                            <input asp-for="Title" class="form-control" placeholder="Enter a title"/>
                            <span asp-validation-for="Title" class="text-danger"></span>
                        </div>
                        <div class="form-group mb-3">
                            <label asp-for="Content" class="form-label">Content</label>
                            <input asp-for="Content" aria-owns="5" class="form-control" placeholder="Enter what you did that day"/>
                            <span asp-validation-for="Content" class="text-danger"></span>
                        </div>
                        <div class="form-group mb-3">
                            <label asp-for="Created" class="form-label">Date</label>
                            <input asp-for="Created" class="form-control" type="date"/>
                            <span asp-validation-for="Created" class="text-danger"></span>
                        </div>
                        <div class="d-flex justify-content-between">
                            <button type="submit" class="btn btn-primary">Edit</button>
                            <a asp-controller="DiaryEntries" asp-action="Index" class="btn btn-secondary">Cancel</a>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>

@section Scripts{
    <partial name="_ValidationScriptsPartial" />
}
```

Next we need to update the button on our index page.

Open the diary entries index page and update the edit button asp-action value to Edit.   
We will also need to let the controller know what database entry we are editing. To do this, add another tag helper
`asp-route-id` and pass in `@entry.Id` as the value (entry references the current object in the foreach loop).

```C#
<a class="btn btn-primary btn-sm" asp-controller="DiaryEntries" asp-action="Edit" asp-route-id="@entry.Id">
    <i class="bi bi-pencil"></i> Edit
</a>
```

Now we need to add our controller methods.

In the diary entries controller, add a new action method to return the view, this should receive the id we are passing
in as a parameter.

Within the method we should create a new object of type `DiaryEntry`, and we should make it nullable. The object should
be set to our DiaryEntries table within our database, then the find method called via dot notation and the id passed in
as a parameter. Then we can return the View with the object as a parameter.

```C#
public IActionResult Edit(int? id)
{   
    DiaryEntry? objDiaryEntry = _db.DiaryEntries.Find(id);

    return View(objDiaryEntry);
}
```

We made the object nullable in case the id is null. We can improve upon this further by adding an if statment before
this that checks if the id is null or zero, and if it is return the 404 not found error page.   

We can add another if statement that does a similar function for the result of our diary entries object.

```C#
public IActionResult Edit(int? id)
{
    if (id == null || id == 0)
    {
        return NotFound();
    }
    
    DiaryEntry? objDiaryEntry = _db.DiaryEntries.Find(id);

    if (objDiaryEntry == null)
    {
        return NotFound();
    }
    
    return View(objDiaryEntry);
}
```

## 'Update' Post Request

Copy the 'create' post request we already have and rename it to Edit, then Ccange the command called on the database
object from Add to Update

```C#
[HttpPost]
public IActionResult Edit(DiaryEntry objDiaryEntry)
{
    if (objDiaryEntry.Title.Length < 3)
    {
        ModelState.AddModelError("Title", "Title is too short");
    }

    if (ModelState.IsValid)
    {
        _db.DiaryEntries.Update(objDiaryEntry);
        _db.SaveChanges();
        
        return RedirectToAction("Index");
    }
    
    return View(objDiaryEntry);
}
```

And that should be all that is required. You can now test this by selecting the edit button on onw of your diary
entries, making an alteration and pressing the edit button. You should now be returned to the main diary page,
with the updated information showing for that entry.

## 'Delete' View and Controller Action

First lets create the Delete route action in our controller. To do this we can just copy the action we made
for Edit, and rename it to Delete. No other changes should be required.

```C#
public IActionResult Delete(int? id)
{
    if (id == null || id == 0)
    {
        return NotFound();
    }
    
    DiaryEntry? objDiaryEntry = _db.DiaryEntries.Find(id);

    if (objDiaryEntry == null)
    {
        return NotFound();
    }
    
    return View(objDiaryEntry);
}
```

Next lets create the Delete view.   

Add a new file within DiaryEntries called Delete.cshtml. The view for this will be almost identical to our edit page,
so copy and paste the code from there into the new file.   
Then just change the title to see Edit instead of Delete, so the same for the asp-action tag helper, ane the button.
Also change the css style for the button from btn-primary to btn-danger to give it a red colour.

```Razor
@model DiaryEntry

<div class="container mt-4">
    <div class="row">
        <div class="col-md-8 offset-md-2">
            <div class="card">
                <div class="card-header bg-primary text-white">
                    <h2>Delete Diary Entry</h2>
                </div>
                <div class="card-body">
                    <form asp-action="Delete" method="post">
                        <div asp-validation-summary="All" class="text-danger"></div>
                        <div class="form-group mb-3">
                            <label asp-for="Title" class="form-label">Title</label>
                            <input asp-for="Title" class="form-control" placeholder="Enter a title"/>
                            <span asp-validation-for="Title" class="text-danger"></span>
                        </div>
                        <div class="form-group mb-3">
                            <label asp-for="Content" class="form-label">Content</label>
                            <input asp-for="Content" aria-owns="5" class="form-control" placeholder="Enter what you did that day"/>
                            <span asp-validation-for="Content" class="text-danger"></span>
                        </div>
                        <div class="form-group mb-3">
                            <label asp-for="Created" class="form-label">Date</label>
                            <input asp-for="Created" class="form-control" type="date"/>
                            <span asp-validation-for="Created" class="text-danger"></span>
                        </div>
                        <div class="d-flex justify-content-between">
                            <button type="submit" class="btn btn-danger">Delete</button>
                            <a asp-controller="DiaryEntries" asp-action="Index" class="btn btn-secondary">Cancel</a>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>

@section Scripts{
    <partial name="_ValidationScriptsPartial" />
}
```

Finally, we just need to update the navigation to the page by amending the button in the index page.

All we need to do here is add Delete as the value for the asp-action and to add an asp-route-id helper tag with a 
value of the entry id (the same as we have for the edit button)

```Razor
<a class="btn btn-danger btn-sm" asp-controller="DiaryEntries" asp-action="Delete" asp-route-id="@entry.Id">
    <i class="bi bi-trash3"></i> Delete
</a>
```

### 'Delete' Post Request

Now all that is left is to add the controller action to handle the post request for deleting an entry.

Again we can start by copying one of the post actions we already have for Edit or Create and naming the new method
Delete. Then a few changes are required, we can remove any checking of the data as all we are doing is deleting.
So all that is required is to call the database object with the Remove method, save the changes then redirect to the 
index page

```C#
[HttpPost]
public IActionResult Delete(DiaryEntry objDiaryEntry)
{
    _db.DiaryEntries.Remove(objDiaryEntry);
    _db.SaveChanges();
    return RedirectToAction("Index");
}
```

And that's it! Our MVC application with a SQL database and a full CRUD functionality is now complete.
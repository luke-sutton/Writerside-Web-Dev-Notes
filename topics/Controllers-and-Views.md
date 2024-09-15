# Controllers and Views

## Add a Controller

Add a new class file to the controllers folder called DiaryEntriesController.

Inherit from the Controller base class, then add an IActionResult called index that returns a view.

```C#
using Microsoft.AspNetCore.Mvc;

namespace lambdaluke_mvc_example.Controllers;

public class DiaryEntriesController : Controller
{
    public IActionResult Index()
    {
        return View();
    }
}
```

## Creating Views

Now we need to create the view to go with our controller.

Add a new folder called DiaryEntries (to match the name of the controller) inside the Views folder.

Inside the DiaryEntries folder, add a new razor file called Index.cshtml (to match the name of our action method).

You can now test that the controller and view are working. Enter some text such as hello world into the view.
Run the application and type /diaryentries/index our new page should now be visible.

We can now add a link to our new page in the header navigation. Open the layout view and look for the existing links
(Home and Privacy).   
Copy one of these links and paste iit below them. Change the value of the asp-controller attribute to our controller
name (DiaryEntries), and change the value of the asp-action attribute to our view name (Index).   
Give the `<a>` tag a name of My Diary.

```C#
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="DiaryEntries" asp-action="Index">My Diary</a>
</li>
```

## Create the View for the Table

Add the following to the DiaryEntries Index page:

```Razor
@model List<DiaryEntry>

<div class="table-responsive">
    <table class="table table-striped table-hover">
        <thead>
        <tr>
            <th>Title</th>
            <th>Content</th>
            <th>Date</th>
            <th>Actions</th>
        </tr>
        </thead>
        <tbody>
        @foreach (var entry in Model)
        {
            <tr>
                <td>@entry.Title</td>
                <td>@entry.Content</td>
                <td>@entry.Created</td>
                <td>Edit and Delete</td>
            </tr>
        }
        </tbody>
    </table>
</div>
```

The first line declares that the view expects a model of type `List<DiaryEntry>`. This list will be used to generate the
table rows dynamically. 

The classes used define a responsive table that will adapt to different screen sizes. - `table-responsive` is a
Bootstrap class that makes the table scrollable when needed. - `table`, `table-striped`, and `table-hover` are
Bootstrap classes used for styling the table. 

The `@foreach` loop iterates over each `DiaryEntry` object in the `Model` (which is a list of `DiaryEntry`). - For each
`entry` in the list, a new table row (`<tr>`) is created. 

Viewing this page will currently display an error as the controller does not know how to access the database

## Add a GET Request to the Controller

Add a private readonly field of type `ApplicationDbContext` with an appropriate name such as _db or _dbContext

```C#
using lambdaluke_mvc_example.Data;
using Microsoft.AspNetCore.Mvc;

namespace lambdaluke_mvc_example.Controllers;

public class DiaryEntriesController : Controller
{
    private readonly ApplicationDbContext _db;
    
    public IActionResult Index()
    {
        return View();
    }
}
```

This sets up a way for our controller to interact with the database.

Next, add a constructor and pass in a parameter of type `ApplicationDbContext`, then assign this parameter to the
field we just created

```C#
using lambdaluke_mvc_example.Data;
using Microsoft.AspNetCore.Mvc;

namespace lambdaluke_mvc_example.Controllers;

public class DiaryEntriesController : Controller
{
    private readonly ApplicationDbContext _db;

    public DiaryEntriesController(ApplicationDbContext db)
    {
        _db = db;
    }
    
    public IActionResult Index()
    {
        return View();
    }
}
```

Note:
: This is an example of dependency injection. We are injecting an instance of `ApplicationDbContext`, this is 
possible as we set it as a service in the program.cs file, which allows us to access it throughout our application.

Now, within the action method, add a variable of type `List<DiaryEntry>` and set it to equal the diary entries in 
the database. Then, pass this variable into the view

```C#
using lambdaluke_mvc_example.Data;
using lambdaluke_mvc_example.Models;
using Microsoft.AspNetCore.Mvc;

namespace lambdaluke_mvc_example.Controllers;

public class DiaryEntriesController : Controller
{
    private readonly ApplicationDbContext _db;

    public DiaryEntriesController(ApplicationDbContext db)
    {
        _db = db;
    }
    
    public IActionResult Index()
    {
        List<DiaryEntry> objDiaryEntryList = _db.DiaryEntries.ToList();
        
        return View(objDiaryEntryList);
    }
}
```

The results should now be visible in the view.

## Add some Styling

We will now make some amendments to the layout and our new page to improve the styling. We will use Bootstrap
classes to achieve this as bootstrap is automatically included in .NET projects.

Within the layout view change the class within the nav component from bg-white to bg-primary.   
Add the class bg-light to the first anchor tag, then change the class bg-dark to bg-light to the anchor tags within
the list items.

Next, we will add an icon to our header link using a bootstrap icon

[Bootstap Icons:](https://icons.getbootstrap.com) 

Go to the bootstrap icons website and search for an appropriate image such as notebook. Then add wither the icon font
code (this requires either installing bootstrap icons via npm, or adding the link text to the header in the layout)

```HTML
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/bootstrap-icons/1.10.0/font/bootstrap-icons.min.css">
```

Or you can use copy HTML on the website to pasete in the SVG code for the image.

Add this before the text `My Diary` in the anchor tag in the navbar.

Now lets add some styling to our diary page.  Add the following to the top of the index page after the model declaration

```Razor
<div class="container">
    <div class="row pt-4">
        <div class="col-6">
            <a class="btn btn-primary">
                <i class="bi bi-journal-plus"></i> Create new Diary Entry
            </a>
        </div>
        <div class="col-6 text-end">
            <h2>Diary Entries</h2>
        </div>
    </div>
</div>
```

## Add the Create Controller Method and View

Add a Create action method to the controller that returns a view

```C#
public IActionResult Create()
{
    return View();
}
```

Next, create the view that this method will return. Add a new file to the DiaryEntries view folder called
Create.cshtml. Add some dummy text to the page for testing purposes, we should now be able to view the page by going
to the URL /DiaryEntries/create.

Note:
: Remember that IActionResult methods listen out for incoming URL requests, so the method we added listens for 
requests from /DiaryEntries/create and then performs the code within the controller, which for now is return view.

Next, we will use tag helpers to call our new action.

```Razor
<a class="btn btn-primary" asp-controller="DiaryEntries" asp-action="Create">
    <i class="bi bi-journal-plus"></i> Create new Diary Entry
</a>
```

The asp-controller tag helper states which controller should be used, and the asp-action tag-helper states which
IActionResult method is called.
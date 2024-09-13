# Creating Controllers and Views

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
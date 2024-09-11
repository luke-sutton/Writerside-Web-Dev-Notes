# Controllers and Views

Here is a basic explanation of the controllers and views in the default project.

Controllers handle the logic and flow of the application, while views are responsible for displaying the data to the
user. In the default project, controllers can be found in the "Controllers" folder and views can be found in the "Views"
folder.

In the controllers folder, you will find a file called HomeController.cs. This home controller dictates what the home
view will do. The name 'Home' that appears before the word controller in the title needs to have a folder with the
same name within the views' folder. That folder should contain files that match the names of the actions listed in
the home controller (Index and Privacy for the default app).

IActionResult methods within the home controller:

```C#
    public IActionResult Index()
    {
        return View();
    }
```

When the above method Index() is called from the home controller, the view named index will be returned from within
the home folder in views. It knows where to look using the name of the controller and the method. It knows to look
in the home folder as that is the name of the controller, and it knows to return the index view as that is the name of
the IActionResult method.

## _Layout.cshtml View

_Layout.cshtml view is a shared layout file that contains the common elements of all views in the project. It
typically includes the HTML structure, navigation menu, and other elements that are common across multiple pages.

```HTML

<div class="container">
    <main role="main" class="pb-3">
        @RenderBody()
    </main>
</div>
```

Between the header and footer code within this view is the above div containing a section called main. This contains
a function called RenderBody. It is this line that loads the rest of the views within the application.

The default view loaded by RenderBody() is set within the main Program.cs file -

```C#
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
```

So we can see above that the index action within the home controller is called to load the default view.
When the user clicks a link etc. to request a new page, the contents of the RenderBody() function are replaced with the
requested view.

At the end of the body section in the layout file is this line:

```C#
@await RenderSectionAsync("Scripts", required: false)
```

This line is used to render any scripts that are specific to a particular view. It is optional and can be omitted
if there are no scripts to render.

So what this means is, that you can add the following to the bottom of a view:

```C#
@section Scripts {

}
```

And here you can add any JavaScript code required by that view.

How does the layout file get loaded? 

The _ViewStart.cshtml file is a special file that is automatically executed before any view is rendered. It is used to
set the layout file for all views in the project. By default, the _ViewStart.cshtml file in the Shared folder sets the
layout file to _Layout.cshtml. However, you can customize this file to set a different layout file for specific views or
areas of your project.
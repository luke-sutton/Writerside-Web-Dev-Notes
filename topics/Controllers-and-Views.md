# Understanding the Default Project

## Controllers and Views

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

How does the layout file get loaded?

The _ViewStart.cshtml file is a special file automatically executed before any view is rendered. It is used to
set the layout file for all views in the project. By default, the _ViewStart.cshtml file in the Shared folder sets the
layout file to _Layout.cshtml. However, you can customize this file to set a different layout file for specific views or
areas of your project.

The default view loaded by RenderBody() is set within the main Program.cs file:

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

## ViewImports and ValidationScriptsPartial

ViewImports.cshtml is located in the Views folder. This file contains using statements for the views, models and tag
helpers in the project. This file allows you to use any view and model within your project without having to declare it
with a using statement at the top of the view file. Tag helpers are the .net commands used inline such as asp-controller
and asp-action. This file is what gives you access to them in your views.

ValidationScriptsPartial.cshtml is a partial view that includes the necessary JavaScript files for client-side
validation. It is typically included in the layout file and is used to enable client-side validation for forms in the
project.

## appsettings.json

The appsettings.json file is a configuration file that stores various settings for the application. It is typically used
to store connection strings, API keys, and other sensitive information. Multiple appsettings files can be created so
that separate connection strings etc can be stored for use in different environments such as dev and live.

## program.cs

The program.cs file is the entry point of the application. It contains the Main method which is the starting point of
the application. This method sets up the web host and configures the application's services and middleware.

```C#
var builder = WebApplication.CreateBuilder(args);
```

This line initializes a new instance of WebApplicationBuilder using the provided args. The CreateBuilder method sets up
the application's configuration sources, logging, and other essential services needed to build the web application.

```C#
builder.Services.AddControllersWithViews();
```

Here, services are being added to the Dependency Injection (DI) container.
AddControllersWithViews is a method call that registers services required for controllers and views in an ASP.NET Core
MVC application. This enables the Model-View-Controller (MVC) pattern, with controllers that can render views.

```C#
var app = builder.Build();
```

After configuring services, the builder.Build() method is called, which compiles everything into a WebApplication
instance. This app instance will be used to configure and run the application.

```C#
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
    // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
    app.UseHsts();
}
```

This line checks if the current environment is not a development environment.
When in a non-development environment, this line adds a middleware to the application's request pipeline that will
handle exceptions. Instead of showing detailed error information (which you might want to avoid in production for
security reasons), this middleware will direct the user to a generic error page.
The "/Home/Error" argument specifies the path where the error-handling controller action is located.

HSTS stands for HTTP Strict Transport Security. This is a web security policy mechanism that helps to protect websites
against protocol downgrade attacks and cookie hijacking.
The app.UseHsts() method enforces this policy by adding a Strict-Transport-Security header to responses. The default
duration for this policy is 30 days.

```C#
app.UseHttpsRedirection();
```

This middleware redirects HTTP requests to HTTPS. This is an important security feature ensuring that communications
between the client and server are encrypted.

```C#
app.UseStaticFiles();
```

This middleware serves static files such as HTML, CSS, JavaScript, and images. It looks for these files in the default
wwwroot folder or any other specified directory.

```C#
app.UseRouting();
```

Adds routing capabilities to the middleware pipeline. This middleware looks at the incoming request and maps it to an
endpoint (such as a controller action) based on matching route patterns.

```C#
app.UseAuthorization();
```

This middleware ensures that the user is authorized to access secured resources based on the app's defined authorization
policies. It must be placed after app.UseRouting but before any endpoint mapping (app.UseEndpoints or similar).

```C#
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
```

This line defines the default route for the application. This means requests like /Home/Index or /Home/Index/1 will be
mapped to the Index action of the Home controller.

```C#
app.Run();
```

This line starts the application and begins listening for incoming HTTP requests.
# CORS - Allow Requests From Your Local Client App

Add the following code to your services -

```C#
builder.Services.AddCors(options =>
{
    options.AddPolicy(name: "AllowLocal", policy
        => policy.WithOrigins("http://localhost:5173")
            .AllowCredentials()
            .AllowAnyMethod());
});
```
Replace the policy name with a name of your choosing and add the appropriate URL that your client app is running on.

Then after initializing your app add the following code -

```C#
app.UseCors("AllowLocal");
```

Example
: Here is a completed minimal API with the code in place -

```C#
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddDbContext<TodoDb>(opt => opt.UseInMemoryDatabase("TodoList"));
builder.Services.AddDatabaseDeveloperPageExceptionFilter();
builder.Services.AddCors(options =>
{
    options.AddPolicy(name: "AllowLocal", policy
        => policy.WithOrigins("http://localhost:5173")
            .AllowCredentials()
            .AllowAnyMethod());
});
var app = builder.Build();

app.UseCors("AllowLocal");

var todoItems = app.MapGroup("/todoitems");

todoItems.MapGet("/", async (TodoDb db) =>
    await db.Todos.ToListAsync());

todoItems.MapGet("/complete", async (TodoDb db) =>
    await db.Todos.Where(t => t.IsComplete).ToListAsync());

todoItems.MapGet("/{id}", async (int id, TodoDb db) =>
    await db.Todos.FindAsync(id)
        is Todo todo
        ? Results.Ok(todo)
        : Results.NotFound());

todoItems.MapPost("/", async (Todo todo, TodoDb db) =>
{
    db.Todos.Add(todo);
    await db.SaveChangesAsync();

    return Results.Created($"/todoitems/{todo.Id}", todo);
});

todoItems.MapPut("/{id}", async (int id, Todo inputTodo, TodoDb db) =>
{
    var todo = await db.Todos.FindAsync(id);

    if (todo is null) return Results.NotFound();

    todo.Name = inputTodo.Name;
    todo.IsComplete = inputTodo.IsComplete;

    await db.SaveChangesAsync();

    return Results.NoContent();
});

todoItems.MapDelete("/{id}", async (int id, TodoDb db) =>
{
    if (await db.Todos.FindAsync(id) is Todo todo)
    {
        db.Todos.Remove(todo);
        await db.SaveChangesAsync();
        return Results.NoContent();
    }

    return Results.NotFound();
});

app.Run();
```
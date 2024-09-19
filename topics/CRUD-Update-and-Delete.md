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
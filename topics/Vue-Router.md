# Vue Router

### $route

You can use $route to access url parameters.

For example to access the id from this url - http://localhost:5173/postDetail/id1

```Javascript
<p>Display the content of post with ID of {{ $route.params.id }} here!</p>
```

Would display the text -   
Display the content of post with ID of id1 here!
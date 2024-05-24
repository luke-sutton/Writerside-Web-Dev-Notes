# Vue Router

### $route

You can use $route within a template to access the route.

For example to access the id from this url - http://localhost:5173/postDetail/id1

```html
<p>Display the content of post with ID of {{ $route.params.id }} here!</p>
```

Would display the text -   
Display the content of post with ID of id1 here!

### useRoute

In order to gain access to the route within setup you first need to import useRoute from vue-router -

```Javascript
import { useRoute} from "vue-router"
```

You then need to assign it to a variable, vue recommends you name this route -

```Javascript
const route = useRoute()
```

You are then able to use this to gain access to the route e.g. -

```Javascript
const showPostId = () => {
  alert(`The ID of this post is: ${route.params.id}`)
}
```

### useRouter

In order to gain access to the router within setup you need to import useRouter from vue-router -

```Javascript
import { useRouter} from "vue-router"
```

You then need to assign it to a variable, vue recommends you name this router -

```Javascript
const router = useRouter()
```

You are then able to use this to gain access to the router e.g. -

```Javascript
const goHomeIn3Seconds = () => {
  setTimeout(() => {
    router.push("/home")
  }, 3000)
}
```

```Javascript
const goToFirstPost = () => {
  router.push({
    name: 'postDetail',
    params: {
      id: 'id1'
    }
  })
}
```
# Install Vue Router Manually

Note:
: The best way to install vue router on a new project is to install it as part of the setup.   
These notes are useful for if you need to add it to an existing project that does not include it.

If the project is running press ctrl + c to stop it

Enter the following in the command line to install the latest version of vue router -

```Bash
npm install vue-router@4
```

Note:
: Check website to ensure this is still the latest version -   
[router.vujs.org](https://router.vuejs.org/installation)

Next we need to import it into our project

Open the main.js file and add the import statement createRouter from vue-router -

```Javascript
import { createRouter} from "vue-router";
```

create a const called router and assign it to createRouter -

```Javascript
const router = createRouter()
```

Pass in an object to this function, add the key history with a value of createWebHistory (this will also
need to be added to the import statement, but this should happen automatically), and pass in a key of routes with an 
empty array as the value -

```Javascript
const router = createRouter({
    history: createWebHashHistory(),
    routes: []
})
```

Add the using statement to the createApp method -

```Javascript
createApp(App).use(router).mount('#app')
```
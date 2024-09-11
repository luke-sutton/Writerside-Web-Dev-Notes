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

### Create a View or Views

Create a folder called views and then add some view files (these can just contain a div with an h1 for example)

### Add a Router file

Create a new folder in the project called router, then add a file called index.js

import createRouter and createWebHashHistory from vue- router

Open the main.js file and add the import statement createRouter from vue-router -

```Javascript
import { createRouter} from "vue-router";
```

Then import your views -

```Javascript
import ViewNotes from "@/views/ViewNotes.vue";
import ViewStats from "@/views/ViewStats.vue";
```

create a const called router and assign it to createRouter -

```Javascript
const router = createRouter()
```

Pass in an object to this function, add the key history with a value of createWebHistory (this will also
need to be added to the import statement, but this should happen automatically), and pass in a key of routes -

```Javascript
const router = createRouter({
    history: createWebHashHistory(),
    routes
})
```

Create a const called routes, and set it to an array -

```Javascript
const routes = []
```

Then update the routes array with the details for the routes. The routes are added as objects with 3 values - path,
name and component -

```Javascript
const routes = [
    {
        path: '/',
        name: 'notes',
        component: ViewNotes
    },
    {
        path: '/stats',
        name: 'stats',
        component: ViewStats
    }
]
```

path is the route for the view, so '/' will indicate that is to the home page   
name is used so that we can refer to the route programmatically
component is the view file we wish to load at this route

Finally export the component -

```Javascript
export default router
```

Example completed router file -

```Javascript
import {createRouter, createWebHashHistory} from "vue-router";
import ViewNotes from "@/views/ViewNotes.vue";
import ViewStats from "@/views/ViewStats.vue";

const routes = [
    {
        path: '/',
        name: 'notes',
        component: ViewNotes
    },
    {
        path: '/stats',
        name: 'stats',
        component: ViewStats
    }
]

const router = createRouter({
    history: createWebHashHistory(),
    routes
})

export default router
```


### update main.js

Import the router -

```Javascript
import router from "@/router/index.js";
```

Add the using statement to the createApp method -

```Javascript
createApp(App).use(router).mount('#app')
```

### Use the Router

Go to App.vue and add the router view component to the template -

```Javascript
<template>
    <router-view></router-view>
</template>
```

Note:
: The component can be named in either pascal case or kebab case

Add links to the views using router link tags -

```Javascript
<template>
  <router-link to="/">Notes</router-link>
  <router-link to="/stats">Stats</router-link>

  <router-view />
</template>
```
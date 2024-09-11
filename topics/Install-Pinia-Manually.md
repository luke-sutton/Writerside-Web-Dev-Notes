# Install Pinia Manually

Note:
: The best way to install vue router on a new project is to install it as part of the setup.   
These notes are useful for if you need to add it to an existing project that does not include it.

If the project is running press ctrl + c to stop it

Enter the following in the command line to install the latest version of vue router -

```Bash
npm install pinia
```

### Update main.js

In the main.js file add the following to import Pinia -

```Javascript
import { createPinia } from "pinia";
```

And then add the using statement to the createApp method -

```Javascript
createApp(App).use(createPinia()).use(router).mount('#app')
```

Add a Store

Create a new folder within the src folder called stores

Then add a JS file with an appropriate name for your store (e.g. storeNotes.js)

Add a import statement and then add the scaffolding for the store, the official website has an example that can be
copied -

```Javascript
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
    state: () => ({ count: 0, name: 'Eduardo' }),
    getters: {
        doubleCount: (state) => state.count * 2,
    },
    actions: {
        increment() {
            this.count++
        },
    },
})
```

Note:
: Official website guide -   
[Pinia](https://pinia.vuejs.org/core-concepts/)

Then update the names used as appropriate and remove the example methods -

```Javascript
import {defineStore} from 'pinia'

export const useStoreNotes = defineStore('storeNotes', {
    state: () => {
        return {
            
        }
    }
})
```

Add data to the store -

```Javascript
import {defineStore} from 'pinia'

export const useStoreNotes = defineStore('storeNotes', {
    state: () => {
        return {
            notes: [
                {
                    id: 'id1',
                    content: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ad assumenda eius hic laudantium modi nemo' +
                        ' nihil non nulla numquam obcaecati officiis, provident quasi sed ullam unde velit veniam vero voluptas.',
                },
                {
                    id: 'id2',
                    content: 'This is a shorter note',
                }
            ]
        }
    }
})
```

### Use the Store

Within the component file you wish to access the stores data from, add an import statement -

```Javascript
import {useStoreNotes} from "@/stores/storeNotes.js";
```

Then create a new const and assign it to the store -

```Javascript
const storeNotes = useStoreNotes()
```

You should now be able to see the store on the page using the vue dev tools
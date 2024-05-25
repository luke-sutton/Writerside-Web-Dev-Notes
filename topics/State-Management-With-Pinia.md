# State Management With Pinia

State management allows you to store all of an apps data and functions related to that data
in one centralized place, which is outside all the components but that all the components can access.
This place is called a store.

State management can be implemented using composables (as mentioned in the composables section) by placing
the reactive object outside the composable function, making it global.

But using a package such as pinia allows for a more standardised approach with more debugging tools using the
vue dev tools.

### Creating a Pinia Store

First (if not there already) create a folder called stores, then create a JS file with the appropriate name for
your store. e.g. for a store to hold data for a counter - counter.js.   

Within the file you should import the defineStore function from pinia,   
Then add a const with the name of the store in camelcase with use at the
beginning and Store at the end (e.g. useCounterStore).   
Add export to the beginning of the function and add a return object at the end -

```Javascript
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', () => {
  }

  return {}
})
```

To access a store in a component you just need to import it and then assign it to a variable -

```Javascript
import {useCounterStore} from "@/stores/counter.js";

const counter = useCounterStore();
```

### State

To add a reactive object to a store you just create them as normal and place them within the stores main
function , and add their names to the return object -

```Javascript
import { ref } from 'vue'
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)
  const title = ref('My Counter Title')

  return { count, title }
})
```

These can then be access with dot notation in the components' template -

```HTML
<h3>{{ counter.title }}:</h3>

<div>
    <span class="counter">{{counter.count}}</span>
</div>
```

### Functions / Actions

These are any methods that modify the stores reactive data.   
These just need to be added as usual to the function and to the return object -

```Javascript
import {ref} from 'vue'
import {defineStore} from 'pinia'

export const useCounterStore = defineStore('counter', () => {
    const count = ref(0)
    const title = ref('My Counter Title')

    function increaseCounter() {
        count.value++
    }

    function decreaseCounter() {
        count.value--
    }

    return {count, title, increaseCounter, decreaseCounter}
})
```

These can then be access in your templates -

```HTML
<button @click="counter.increaseCounter(1)" class="btn">+</button>
<button @click="counter.decreaseCounter(2)" class="btn">--</button>
```

### Getters

Getters allow us to get some data from our state and modify it.   
These can be added by using computed properties and adding them in the usual way -

```Javascript
import {ref, computed} from 'vue'
import {defineStore} from 'pinia'

export const useCounterStore = defineStore('counter', () => {
    const count = ref(0)
    const title = ref('My Counter Title')

    const oddOrEven = computed(() => count.value % 2 === 0 ? 'Even' : 'Odd')

    function increaseCounter(amount) {
        count.value += amount
    }

    function decreaseCounter(amount) {
        count.value -= amount
    }

    return {count, title, oddOrEven, increaseCounter, decreaseCounter}
})
```

```HTML
<p>This counter is {{ counter.oddOrEven }} </p>
```
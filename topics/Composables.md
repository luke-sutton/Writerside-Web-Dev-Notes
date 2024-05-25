# Composables

Composables are reusable functions that are in their own separate template that can be imported and
used in multiple other components.

### Create a Composable

The convention for creating and naming composables is to place them inside a folder named use, and to 
name the file in camel case begining with the word use. e.g. useCounter.js

within the file export a function with the appropriate name -

```Javascript
export function useCounter() {}
```

Then you just need to add the data and functions required within this function, import the necessary
methods from vue before the function, Then at the end of the function export the objects and
methods required -

```Javascript
import {computed, nextTick, reactive, watch} from "vue";

export function useCounter() {
    const counterData = reactive({
        count: 0,
        title: 'My Counter'
    })

    watch(() => counterData.count, (newCount) => {
        if (newCount === 20) {
            alert('Way to go! You made it to 20!')
        }
    })

    const oddOrEven = computed(() => {
        if (counterData.count % 2 === 0) return 'even'
        return 'odd'
    })

    const increaseCounter = (amount) => {
        counterData.count += amount
        nextTick(() => {
            console.log('do something when counter has updated in the dom')
        })
    }

    const decreaseCounter = amount => {
        counterData.count -= amount
    }

    return {
        counterData,
        oddOrEven,
        increaseCounter,
        decreaseCounter
    }
}
```

If you would like data to persist between components that use the composable, this can be achieved
by simply moving the reactive data out of the function and placed above it -

```Javascript
import {computed, nextTick, reactive, watch} from "vue";

const counterData = reactive({
    count: 0,
    title: 'My Counter'
})

export function useCounter() {

    watch(() => counterData.count, (newCount) => {
        if (newCount === 20) {
            alert('Way to go! You made it to 20!')
        }
    })

    const oddOrEven = computed(() => {
        if (counterData.count % 2 === 0) return 'even'
        return 'odd'
    })

    const increaseCounter = (amount) => {
        counterData.count += amount
        nextTick(() => {
            console.log('do something when counter has updated in the dom')
        })
    }

    const decreaseCounter = amount => {
        counterData.count -= amount
    }

    return {
        counterData,
        oddOrEven,
        increaseCounter,
        decreaseCounter
    }
}
```

SO in the above example counterData will keep its value when moving between components.

### Use a Composable

To use a composable within a component it must first be imported into that component -

```Javascript
import {useCounter} from "@/use/useCounter.js";
```

In order to use the composable you must assign it to a variable.

If you have moved functionality out of the component and into the composable you are importing,
you can return the functionality without altering the template by creating a const with an object containing
the names of the objects and functions and assigning it to the value of the imported composable -

```Javascript
const {counterData, oddOrEven, increaseCounter, decreaseCounter} = useCounter()

```

Alternatively you can assign it a new name, and then prepend the references in the template with
this name using dot notation -

```Javascript
const counter = useCounter()
```

```HTML
<div>
    <button @click="counter.decreaseCounter(2)" class="btn">--</button>
    <button @click="counter.decreaseCounter(1)" class="btn">-</button>
    <span class="counter">{{counter.counterData.count}}</span>
    <button @click="counter.increaseCounter(1)" class="btn">+</button>
    <button @click="counter.increaseCounter(2)" class="btn">++</button>
</div>
```

The first approach is often preferred as it makes it clear in the script section exactly what is being
imported.

### VueUse

VueUse is a library of ready to use composables.

[https://vueuse.org/guide/](https://vueuse.org/guide/)
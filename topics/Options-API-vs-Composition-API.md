# Options API vs Composition API

There are 3 different setups that can be used to create vue apps -

* Options API
* Composition API
* Composition API with script setup tag

The different setup types only change the way the javascript is used within the script tags.   
The contents of the template and style tags will be the same for each.

The following examples will show the different approaches and will all use the following 
html template for a simple app that has a dynamic counter -

```Javascript
<template>
<div class="home">
  <div>
    <button @click="decreaseCounter" class="btn">-</button>
    <span class="counter">{{counter}}</span>
    <button @click="increaseCounter" class="btn">+</button>
  </div>
</div>
</template>
```

### Options API

This is the original setup and is the one used in vue 2 apps and earlier, but can still
be used in vue 3.

```Javascript
<script>
export default {
  data() {
    return {
      counter: 0
    }
  },
  methods: {
    increaseCounter() {
      this.counter++
    },
    decreaseCounter() {
      this.counter--
    }
  }
}
</script>
```

In this approach objects are created to hold data and methods etc and they are all wrapped in
an export function.

### Composition API

This method was introduced in vue 3. It enables code to be reused more easily and allows
for cleaner code as related data and functions can be grouped together as they do not need to
be placed in specific objects for that data type.   
In this approach variables and methods are declared using standard javascript notation, these are wrapped
in a setup function and a return statement is used to return the results of the data, and again this 
is all wrapped in an export function.

```Javascript
<script>
import {ref} from "vue";

export default {
  setup() {
    const counter = ref(0)

    const increaseCounter = () => {
      counter.value++
    }

    const decreaseCounter = () => {
      counter.value--
    }

    return {
      counter,
      increaseCounter,
      decreaseCounter
    }
  }
}
</script>
```

### Composition API With Setup Script Tag

This is the latest approach and is the recommended approach for new projects.   
This approach is the same as the composition API, but it removes the need for the export, setup
and return statement.

```Javascript
<script setup>
import {ref} from "vue";

const counter = ref(0)

const increaseCounter = () => {
  counter.value++
}

const decreaseCounter = () => {
  counter.value--
}
</script>
```

This has all the benefits of the composition API while also reducing the amount of code required
and improving readability.
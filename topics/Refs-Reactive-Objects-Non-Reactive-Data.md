# Refs, Reactive Objects &amp; Non-Reactive Data

### Refs

Refs are the method used to make variables reactive.

First you need to import the ref method from vue -

```Javascript
import {ref} from "vue"
```

Then when you declare a variable and assign it to the ref method and pass in its initial value -

```Javascript
const variableName = ref(initialValue)

const counter = ref(0)
```

This changes the variable into an object so to access it you need to use .value -

```Javascript
const increaseCounter = () => {
  counter.value++
}
```

Multiple refs can be created, you can use shorthand when grouping these by only using 1 const declaration and
then separating them with a comma -

```Javascript
const counter = ref(0),
    counterTitle = ref('My Counter')
```

### 2 Way Data Binding

To enable 2 way data binding you simply use v-model and assign it the relevant ref.


For example the following code has text displayed using a ref -
```Javascript
<template>
  <h3>{{counterTitle}}</h3>
</template>

<script setup>
const counterTitle = ref('My Counter')
</script>
```

We can enable 2 way binding by adding an input field and assigning the ref to v-model -

```Javascript
<template>
  <h3>{{counterTitle}}</h3>
  <input v-model="counterTitle" type="text">
</template>

<script setup>
const counterTitle = ref('My Counter')
</script>
```

This will add an input field to the page displaying the same default value assigned to the ref 
that is used in the text field.   
Changing the text in the input field will also update the text field (2 way binding).

### Reactive Objects

Rather than creating individual refs you can group related variables into a reactive object
using the reactive keyword

So for example rather than creating the 2 refs here -

```Javascript
import {ref} from "vue"

const counter = ref(0),
    counterTitle = ref('My Counter')
```

You can create a reactive object to hold both variables (you must first import the reactive method
from vue) and access them in your template using dot notation -

```Javascript
<script setup>
import {reactive} from "vue"

const counterData = reactive({
  count: 0,
  title: 'My Counter'
})

const increaseCounter = () => {
  counterData.count++
}

const decreaseCounter = () => {
  counterData.count--
}
</script>

<template>
<div class="home">

  <h3>{{counterData.title}}:</h3>

  <div>
    <button @click="decreaseCounter" class="btn">-</button>
    <span class="counter">{{counterData.count}}</span>
    <button @click="increaseCounter" class="btn">+</button>
  </div>

  <div class="edit">
    <h4>Edit counter title:</h4>
    <input v-model="counterData.title" type="text">
  </div>
</div>
</template>
```

Note that in the methods you do not need to use .value to manipulate the data in 
a reactive object.

### Non-Reactive Data

ANy data that you use that does not need to be reactive should be made non-reactive to improve performance.

To create non-reactive data you just declare it without using the ref keyword -

```Javascript
const nonReactiveTitle = 'This value is not reactive'
```
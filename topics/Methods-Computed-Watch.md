# Methods, Computed &amp; Watch

### Methods

Methods are written with standard javascript notation.

Example on how to pass arguments to methods in vue -

```Javascript
<template>
<div class="home">

  <h3>{{counterData.title}}:</h3>

  <div>
    <button @click="decreaseCounter(2)" class="btn">--</button>
    <button @click="decreaseCounter(1)" class="btn">-</button>
    <span class="counter">{{counterData.count}}</span>
    <button @click="increaseCounter(1)" class="btn">+</button>
    <button @click="increaseCounter(2)" class="btn">++</button>
  </div>

</div>
</template>

<script setup>
import {reactive} from "vue"

const counterData = reactive({
  count: 0,
  title: 'My Counter'
})

const increaseCounter = (amount) => {
  counterData.count += amount
}

const decreaseCounter = amount => {
  counterData.count -= amount
}
</script>
```

### Computed Properties

Computed properties are properties whose value is only calculated when a change in a data property
is detected.

To use a computed property you must import the computed method from vue, then assign the computed method
to a variable -

```Javascript
import {computed} from "vue"

const computedPropertyName = computed()
```

You must then pass in a function to the computed property -

```Javascript
const computedPropertyName = computed(() => {})
```

Then the logic and a return statement must be added -

```Javascript
const computedPropertyName = computed(() => {
    // some logic
    return 'return value'
})
```

Here is an example of a simple counter with a computed property to state if the value is
odd or even, the computed property method will only be called when the value of the counter changes -

```Javascript
<template>
<div class="home">

  <h3>{{counterData.title}}:</h3>

  <div>
    <button @click="decreaseCounter(2)" class="btn">--</button>
    <button @click="decreaseCounter(1)" class="btn">-</button>
    <span class="counter">{{counterData.count}}</span>
    <button @click="increaseCounter(1)" class="btn">+</button>
    <button @click="increaseCounter(2)" class="btn">++</button>
  </div>

  <p>This counter is {{oddOrEven}} </p>

  <div class="edit">
    <h4>Edit counter title:</h4>
    <input v-model="counterData.title" type="text">
  </div>
</div>
</template>

<script setup>
import {reactive, computed} from "vue"

const counterData = reactive({
  count: 0,
  title: 'My Counter'
})

const oddOrEven = computed(() => {
  if (counterData.count % 2 === 0) return 'even'
  return 'odd'
})

const increaseCounter = (amount) => {
  counterData.count += amount
}

const decreaseCounter = amount => {
  counterData.count -= amount
}
</script>
```

### Watch

Watchers allow us to watch a reactive data property and then perform an action whenever
its value changes.

To create a watch you must first import the watch method from vue -

```Javascript
import {watch} from "vue"
```

You should then pass in the value to be watched. If the value comes from a ref you can add
the value directly, but if it is from a reactive object you must use a method to act as a
getter -

```Javascript
watch(refProperty)

watch(() => reactiveObject.property)
```

You then pass in the 2 arguments which you can name however you please to track the 
old and new values -

```Javascript
watch(() => reactiveObject.property, (newValue, oldValue))
```

Then you add the function you would like to perform -

```Javascript
watch(() => counterData.count, (newValue, oldValue)) => {
  console.log('oldValue', oldValue)
  console.log('newValue', newValue)
})
```

If you are not using the old value it can be removed as an argument -

```Javascript
watch(() => counterData.count, (newValue)) => {
  console.log('newValue', newValue)
})
```
# Lists, Teleport, Template Refs &amp; nextTick

### Lists (v-for)

You can use the v-for directive in a template to loop through data.   
For example rather than this hard coded list -

```html
<ul>
  <li>
    <RouterLink to="/postDetail/id1">Post 1</RouterLink>
  </li>
  <li>
    <RouterLink to="/postDetail/id2">Post 2</RouterLink>
  </li>
  <li>
    <RouterLink to="/postDetail/id3">Post 3</RouterLink>
  </li>
</ul>
```

You can create an object to hold the data and then use v-for to create the items dynamically -

```Javascript
<script setup>
import {ref} from "vue";

const posts = ref([
  {
    id: 'id1',
    title: 'Post 1',
  },
  {
    id: 'id2',
    title: 'Post 2',
  },
  {
    id: 'id3',
    title: 'Post 3',
  },
])
</script>
```

```html
<ul>
  <li v-for="post in posts" :key="post.id">
    <RouterLink :to="`/postDetail/${ post.id }`">{{ post.title }}</RouterLink>
  </li>
</ul>
```

### Template Refs

Template refs are simply refs (reactive data) that are created within a template (rather than in setup).

To create a ref you use the ref directive and the naming convention is to end the name with Ref -

```html
<h2 ref="appTitleRef">{{ appTitle }}</h2>
```

You should then create a variable with the same name and assign it to a ref with a value of null -

```Javascript
const appTitleRef = ref(null)
```

You can then access the template ref within the normal lifecycle hooks -

```Javascript
onMounted(() => {
  console.log(`The app title is ${appTitleRef.value.offsetWidth}px wide`)
})
```

In this example the width of the title is printed to the console, and as it is a reactive property
the width will update in the console when the width changes.

### Next Tick

Next tick allows us to wait for the DOM to do something and then perform an action once the DOM has updated.

```Javascript
import { nextTick } from "vue"

nextTick(() => {
    console.log('do something when counter has updated in the dom')
  })
```


### Teleport

Teleporting allows us to move an element from its default place in the DOM to somewhere else in the DOM.

In order to teleport an element you need to surround the element with a teleport tag, and specify where 
in the DOM it should be placed -

```html
<template>
  <div class="modals">
    <h1>Modals</h1>
    <button @click="showModal = true">Show modal</button>
    <teleport to="body">
      <div v-if="showModal" class="modal">
        <h1>This is a modal</h1>
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Beatae consequuntur dolor eaque in, ipsa laborum
          natus nobis, nostrum omnis rem, repudiandae sequi voluptatibus? Atque beatae dignissimos earum, exercitationem fugiat quibusdam?
        </p>
        <button @click="showModal = false">Hide modal</button>
      </div>
    </teleport>
  </div>
</template>
```
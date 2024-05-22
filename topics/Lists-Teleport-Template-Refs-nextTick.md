# Lists, Teleport, Template Refs &amp; nextTick

### Lists (v-for)

You can use the v-for directive in a template to loop through data.   
For example rather than this hard coded list -

```Javascript
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

```Javascript
<ul>
  <li v-for="post in posts" :key="post.id">
    <RouterLink :to="`/postDetail/${ post.id }`">{{ post.title }}</RouterLink>
  </li>
</ul>
```

### Template Refs

Template refs are simply refs (reactive data) that are created within a template (rather than in setup).

To create a ref you use the ref directive and the naming convention is to end the name with Ref -

```Javascript
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
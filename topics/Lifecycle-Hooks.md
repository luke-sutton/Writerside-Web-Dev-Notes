# Lifecycle Hooks

Lifecycle hooks allow for code to run at different stages of an applications lifecycle.

Lifecycle hooks need to be imported from vue -

```Javascript
import {onBeforeMount, onMounted, onBeforeUnmount, onUnmounted} from "vue"
```

### Mounted Hooks

* Before Mount

Code will run just before the component is loaded to the browser

```Javascript
onBeforeMount(() => {
  console.log("beforeMount")
})
```

* On Mount

Code will run when the component is loaded

```Javascript
onMounted(() => {
  console.log("onMounted")
})
```

* Before Mount

Code will run just before the component is removed from the browser

```Javascript
onBeforeUnmount(() => {
  console.log("onBeforeUnmount")
})
```

* On Unmount

Code will run after component has been removed from the browser

```Javascript
onUnmounted(() => {
  console.log("onUnmounted")
})
```

### Activated Hooks

In order to use these hooks a 'keep alive' tag needs to be used in the main template where
the router is used -

To achieve this replace this -

```Javascript
<RouterView />
```

With this -

```Javascript
<router-view v-slot="{ Component }">
    <keep-alive>
      <component :is="Component" />
    </keep-alive>
  </router-view>
```

* Activated Hook

This hook will only run if the component is being actively kept alive

```Javascript
onActivated(() => {
  console.log("onActivated")
})
```

* Deactivated Hook

This hook will run when the component is no longer being kept alive

```Javascript
onDeactivated(() => {
  console.log("onDeactivated")
})
```

### Updated Hooks

These hooks run whenever a component is updated.

* Before Update

This hook will run when a component is updated just before the change occurs on screen

```Javascript
onBeforeUpdate(() => {
  console.log('onBeforeUpdate')
})
```

* Updated

This hook will run the moment a component is updated

```Javascript
onUpdated(() => {
  console.log('onUpdated')
})
```
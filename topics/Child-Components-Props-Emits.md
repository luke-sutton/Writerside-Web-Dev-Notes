# Child Components, Props &amp; Emits

### Child Components

To create a child component add the template to a file within the components' folder.   
Then import that file into your view.

In the following example a Modal child component is imported and used within the template -

```HTML
<template>
  <div class="modals">
    <h1>Modals</h1>
    <button @click="showModal = true">Show modal</button>
    <Modal v-if="showModal"></Modal>
  </div>
</template>

<script setup>
import Modal from '@/components/Modal.vue'
import { ref } from 'vue';

const showModal = ref(false);
</script>
```

### Slots

Slots are used to pass html elements down from the parent to the child component.

To do this just place the html to be passed down between the template tags in the parent template,
and then place a slot tag where that data should be placed within the child component.

Example parent template -

```HTML
<template>
  <div class="modals">
    <h1>Modals</h1>
    <button @click="showModal = true">Show modal</button>
    <Modal v-if="showModal">
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Beatae consequuntur dolor eaque in, ipsa laborum
        natus nobis, nostrum omnis rem, repudiandae sequi voluptatibus? Atque beatae dignissimos earum, exercitationem fugiat quibusdam?
      </p>
    </Modal>
  </div>
</template>
```

Example child component template -

```HTML
<template>
    <div class="modal">
      <h1>This is a modal</h1>
      <slot/>
      <button>Hide modal</button>
    </div>
</template>

```

In this example the p tag containing the lorem ipsum text will be placed between the h1 and button tags
in the child component.

### Named Slots

You can use named slots to specify exactly where data should be passed to.

To achieve this, in the child component add a slot where the data is to be passed to and give it a name -

```HTML
<div class="modal">
      <h1><slot name="title"/></h1>
      <slot/>
      <button>Hide modal</button>
    </div>
```

In the parent component, within the named child component tag add a template tag
containing the data and add a v-slot directive with the name given to the slot in the child component -

```HTML
<div class="modals">
    <h1>Modals</h1>
    <button @click="showModal = true">Show modal</button>
    <Modal v-if="showModal">
      <template v-slot:title>My new title</template>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Beatae consequuntur dolor eaque in, ipsa laborum
        natus nobis, nostrum omnis rem, repudiandae sequi voluptatibus? Atque beatae dignissimos earum, exercitationem fugiat quibusdam?
      </p>
    </Modal>
  </div>
```

Or you can use the shorthand and replace v-slot: with #

```HTML
<template #title>My new title</template>
```

### Accessing Data in Slots

To access data within slots in a template you use $slots -

```HTML
<div class="modal">
  <h1><slot name="title"/></h1>
  <slot/>
  <pre>{{ $slots.title() }}</pre>
  <button>Hide modal</button>
</div>
```

To access data within slots programmatically (within setup) you need to import the method useSlots
from vue and then assign it to a variable -

```Javascript
<script setup>
import {useSlots} from "vue";

const slots = useSlots()

console.log(slots.title())
</script>
```

### Props

Props are used to pass data down from a parent to a child component.

To create a prop you add it to the tag for the child component   
In this example we are passing the title as a prop -

```HTML
<Modal v-if="showModal" title="My modal title (via prop)">
  <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Beatae consequuntur dolor eaque in, ipsa laborum
    natus nobis, nostrum omnis rem, repudiandae sequi voluptatibus? Atque beatae dignissimos earum, exercitationem fugiat quibusdam?
  </p>
</Modal>
```

Within the child component you must then create a variable called props and assign it the define props method -

```Javascript
const props = defineProps()
```

Then the relevent prop details should be passed in, either as an array -

```Javascript
const props = defineProps(['title'])
```

Or as an object -

```Javascript
const props = defineProps({
  title: {
    type: String,
    default: 'No title specified'
  }
})
```

And then access it within the template -

```HTML
<h1>{{ props.title }}</h1>
// or
<h1>{{ title }}</h1>
```

### Emits

Emits are used to pass events up from a child component to its parent.

To create an emit within the child component you must create an emit variable assigned 
to the defineEmits method -

```Javascript
const emit = defineEmits()
```

And then pass in the appropriate emits -

```Javascript
const emit = defineEmits(['hideModal'])
```

Then within the template these can be assigned to a click function using $emit -

```HTML
<button @click="$emit('hideModal')">Hide modal</button>
```

Then within the parent component you listen for the emit by using @ and the emits name within the tag
for the child component -

```HTML
<div class="modals">
<h1>Modals</h1>
<button @click="showModal = true">Show modal</button>
<Modal v-if="showModal"
       title="My modal title (via prop)"
       @hideModal="showModal = false">
  <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Beatae consequuntur dolor eaque in, ipsa laborum
    natus nobis, nostrum omnis rem, repudiandae sequi voluptatibus? Atque beatae dignissimos earum, exercitationem fugiat quibusdam?
  </p>
</Modal>
</div>
```

To emit an event programmatically from the script section you can trigger it from a method -

```Javascript
const handleButtonClick = () => {
  emit('hideModal');
}
```

### modelValue

modelValue is an alternative way of updating values in a parent component from a child component by
giving them direct access to the appropriate data.

To achieve this, in the parent template you use the v-model directive in the child component tag and assign it
to the ref you wish the child component to have access to -

```HTML
<template>
    <div class="modals">
        <h1>Modals</h1>
        <button @click="showModal = true">Show modal</button>
        <Modal v-model="showModal"
               title="My modal title (via prop)">
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Beatae consequuntur dolor eaque in, ipsa laborum
                natus nobis, nostrum omnis rem, repudiandae sequi voluptatibus? Atque beatae dignissimos earum, exercitationem fugiat quibusdam?
            </p>
        </Modal>
    </div>
</template>

<script setup>
    import Modal from '@/components/Modal.vue'
    import { ref } from 'vue';
    
    const showModal = ref(false);
</script>
```

Then within the child component you must add a specific type of prop called modelValue -

```Javascript
const props = defineProps({
  modelValue: {
    type: Boolean,
    default: false
  },
  title: {
    type: String,
    default: 'No title specified'
  }
})
```

You should then add an emit called update:modelvalue -

```Javascript
const emit = defineEmits(['update:modelValue'])
```

You can then access the data property within your template directives using modelValue, and
you can update the value by using an emit and passing in update:modelValue along with the new value
for the property -

```HTML
<template>
  <teleport to=".modals-container">
    <div v-if="modelValue" class="modal">
      <h1>{{ props.title }}</h1>
      <slot/>
      <button @click="emit('update:modelValue', false)">Hide modal</button>
    </div>
  </teleport>
</template>
```

### Dynamic Components

Dynamic components allow us to switch out components that is being used in a particular part of an app.

Within the parent component you should use the component tag instead of template, then use the :is
directive to set it to the appropriate template.

Here ia an example that uses a checkbox to determine what template is loaded -

```HTML
<template>
    <div class="modals">
        <h1>Modals</h1>
        <div>
            <label>Show dark modals?</label>
            <input v-model="showDarkModals" type="checkbox">
        </div>
        <button @click="showModal = true">Show modal</button>
        <component :is="showDarkModals ? ModalDark : Modal"
                   v-model="showModal"
                   title="My modal title (via prop)">
            <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Beatae consequuntur dolor eaque in, ipsa laborum
                natus nobis, nostrum omnis rem, repudiandae sequi voluptatibus? Atque beatae dignissimos earum, exercitationem
                fugiat quibusdam?
            </p>
        </component>
    </div>
</template>

<script setup>
import Modal from '@/components/Modal.vue'
import ModalDark from '@/components/ModalDark.vue'
import {ref} from 'vue';

const showDarkModals = ref(false);
const showModal = ref(false);
</script>
```

### Provide / Inject

Provide / Inject can be used to pass data from a prent component to a deeply nested child component.

In the parent component import provide from vue, create the reactive object you wish the nested child
component to have access to, then call the provide method and pass in a name for the object (commonly the 
name of the object) and the object itself -

```Javascript
<script setup>
import {reactive,provide} from "vue";

const userData = reactive({
  name: 'Luke',
  username: 'lamdaluke'
})

provide('userData', userData)
</script>
```

You can then access the data in the nested child component by importing inject from vue, then creating a
variable and assigning it to the inject method and passing in the name given in the provide method -

```Javascript
<script setup>
import {inject} from "vue";

const userData = inject('userData')
</script>
```

You can then access the object in the template -

```HTML
<div>Username is: {{userData.username}}</div>
```
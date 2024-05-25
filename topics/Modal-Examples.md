# Modal Examples

### Simple Modal Example

Here is an example of how to create a simple modal that opens in the center of the screen
with a button to close it -

```HTML
<template>
  <div class="modals">
    <h1>Modals</h1>
    <button @click="showModal = true">Show modal</button>
      <div v-if="showModal" class="modal">
        <h1>This is a modal</h1>
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Beatae consequuntur dolor eaque in, ipsa laborum
          natus nobis, nostrum omnis rem, repudiandae sequi voluptatibus? Atque beatae dignissimos earum, exercitationem fugiat quibusdam?
        </p>
        <button @click="showModal = false">Hide modal</button>
      </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const showModal = ref(false);
</script>

<style>
.modal {
  background-color: beige;
  color: black;
  padding: 10px;
  position: absolute;
  left: 25%;
  top: 25%;
  width: 50%;
  height: 50%;
  z-index: 1;
}
</style>
```

### Example Using Child Components, Props & modelValue

Parent component -

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

Child component -

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

<script setup>
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

const emit = defineEmits(['update:modelValue'])
</script>

<style>
.modal {
  background-color: beige;
  color: black;
  padding: 10px;
  position: absolute;
  left: 25%;
  top: 25%;
  width: 50%;
  height: 50%;
  z-index: 1;
}
</style>
```
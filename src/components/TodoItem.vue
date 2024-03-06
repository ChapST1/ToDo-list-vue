<script setup>
import { ref } from "vue";

/**
 * Component props
 */
const props = defineProps(["todoContent", "id"])

/**
 * Local state
 */
const isEdit = ref(false)
const newEditValue = ref(props.todoContent)

/**
 * All events functions
 */
const emits = defineEmits(["edit", "delete"])

/**
 * Handler functions
 */
const onSubmit = () => {
  emits('edit', props.id, newEditValue.value)
  isEdit.value = !isEdit.value
}

const onDelete = () => emits("delete", props.id)

</script>

<template>
  <div>
    <span>
      {{ todoContent }}
    </span>
    <div class="group space">
      <button class="secondary" @click="isEdit = !isEdit">edit</button>
      <button class="secondary" @click="onDelete">Delete</button>
    </div>
  </div>

  <form v-if="isEdit" @submit.prevent="onSubmit">
    <label>
      <input type="text" v-model="newEditValue">
    </label>
    <button>Update</button>
  </form>
</template>

<style scoped>
span {
  flex-grow: 1;
}

div {
  display: flex;
  position: relative;

  & button {
    font-size: 13px;
    padding: 5px 10px;
  }
}

.space {
  display: flex;
  gap: 5px;
}

.hidden {
  opacity: 0.5;
}
</style>
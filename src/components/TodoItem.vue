<template>
  <q-item clickable :class="{ 'q-item--active': todo.done }" class="todo-item q-pa-sm">
    <q-item-section avatar>
      <q-checkbox v-model="todo.done" color="primary" @update:model-value="$emit('toggle')" />
    </q-item-section>
    <q-item-section>
      <q-item-label>{{ todo.text }}</q-item-label>
    </q-item-section>
    <q-item-section side>
      <q-btn @click="$emit('remove', index)" color="negative" icon="cancel" flat />
    </q-item-section>
  </q-item>
</template>

<script>
export default {
  props: {
    todo: Object,
    index: Number,
  },
  emits: ['remove', 'toggle'],
};
</script>

<style scoped>
.todo-item {
  background: #f0f0f0; /* Light gray background */
  color: black;
  border-radius: 8px;
  margin-bottom: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.todo-item:hover {
  background: #d3d3d3; /* Darker gray on hover */
}

.q-item--active .q-item__label {
  text-decoration: line-through;
  color: #757575; /* Gray color for completed items */
}

.q-checkbox input:checked ~ .q-checkbox__bg {
  background-color: #757575; /* Gray color for checked checkbox */
  border-color: #757575;
}
</style>

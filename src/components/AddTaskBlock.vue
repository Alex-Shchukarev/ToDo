<template>
  <div class="form-control">
    <label class="label" for="search">Введите задачу:</label>
    <input type="text" :value="modelValue" id="search" @input="$emit('update:modelValue', $event.target.value)"/>
    <button class="btn" @click="$emit('add')" :disabled="lengthInput">Добавить задачу</button>
  </div>
</template>

<script>
export default {
  props: ['modelValue'],
  emits: ['update:modelValue', 'add'],
  computed: {
    lengthInput() {
      return this.modelValue.length <= 5;
    }
  }
}
</script>
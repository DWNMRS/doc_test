<template>
  <div class="form-editor">
    <div class="form-element">
      <div class="move-controls">
        <label>Ширина: <select v-model.number="selectedElementWidth" :disabled="!selectedElement" @change="changeWidth"
            max="12" min="1">
            <option>1</option>
            <option>2</option>
            <option>3</option>
            <option>4</option>
          </select></label>
        <button @click="moveElement('up')" :disabled="!selectedElement">Вверх</button>
        <button @click="moveElement('down')" :disabled="!selectedElement">Вниз</button>
        <button @click="deleteElement" :disabled="!selectedElement">Удалить</button>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, watch } from 'vue';

export default defineComponent({
  name: 'FormEditor',
  props: {
    selectedElement: {
      type: Object,
      required: false,
      default: null
    }
  },
  setup(props, { emit }) {
    const selectedElementWidth = ref(props.selectedElement ? props.selectedElement.width : 0);

    watch(() => props.selectedElement, (newVal) => {
      if (newVal) {
        selectedElementWidth.value = newVal.width;
      }
    });

    const changeWidth = () => {
      if (props.selectedElement) {
        const updatedElement = { ...props.selectedElement, width: selectedElementWidth.value };
        emit('changeWidth', updatedElement);
      }
    };

    const moveElement = (direction) => {
      emit('move-element', direction);
    };

    const deleteElement = () => {
      emit('delete-element');
    };

    return {
      selectedElementWidth,
      changeWidth,
      moveElement,
      deleteElement
    };
  }
});
</script>

<style scoped>
.form-editor {
  padding: 8px;
  background-color: #f5f5f5;
}

.controls {
  display: flex;
  gap: 10px;
}

.move-controls {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 8px;
}

.move-controls button {
  margin-right: 10px;
}
</style>

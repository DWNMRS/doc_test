<template>
  <div ref="formViewRef" class="form-view">
    <div v-for="(element, index) in formElements" :key="element.uniqueId"
      :class="['form-element', { 'form-element--selected': element.selected }]"
      :style="{ width: ((formViewWidth / element.width) - 6) + 'px' }" @click.stop="selectElement(element)">
      <input class="form-element__input" v-if="element.type === 'string'" :placeholder="element.placeholder" />
      <input class="form-element__input" v-if="element.type === 'date'" type="date" />
      <select class="form-element__input" v-if="element.type === 'ref.users'">
        <option v-for="option in element.options" :key="option">{{ option }}</option>
      </select>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, onBeforeUnmount } from 'vue';

export default defineComponent({
  name: 'FormView',
  props: {
    formElements: {
      type: Array,
      required: true
    }
  },
  setup(props, { emit }) {
    const formElements = ref(props.formElements);
    const formViewRef = ref(null);
    const formViewWidth = ref(0);

    const updateWidth = () => {
      if (formViewRef.value) {
        formViewWidth.value = formViewRef.value.offsetWidth - 40;
      }
    };

    let resizeObserver = null;

    onMounted(() => {
      updateWidth();
      resizeObserver = new ResizeObserver(() => {
        updateWidth();
      });
      resizeObserver.observe(formViewRef.value);
    });

    onBeforeUnmount(() => {
      if (resizeObserver) {
        resizeObserver.disconnect();
      }
    });

    const selectElement = (element) => {
      emit('select-element', element);
    };

    return {
      formElements,
      formViewRef,
      formViewWidth,
      selectElement
    };
  }
});
</script>

<style lang="scss" scoped>
.form-view {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  padding: 20px;
}

.form-element {
  cursor: pointer;
  border: 2px solid #eeeeee;
  border-radius: 4px;
  transition: 0.2s;

  &--selected {
    border: 2px solid #7ffaf4;
  }
}

.form-element__input {
  width: 100%;
  border-radius: 4px;
  border: none;
  margin: 0;
  padding: 4px;
  outline: none;
}

.form-element:hover {
  background-color: #f0f0f0;
  border: 2px solid #7ffaf4;
}
</style>

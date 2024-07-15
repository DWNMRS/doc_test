<template>
  <div class="form-element-list">
    <ul>
      <li v-for="element in elements" :key="element.id" @click="addElement(element)">
        {{ element.name }}
      </li>
    </ul>
  </div>
</template>

<script>
import { defineComponent, toRefs } from 'vue';

export default defineComponent({
  name: 'FormElementList',
  props: {
    elements: {
      type: Array,
      required: true
    }
  },
  setup(props, { emit }) {
    const { elements } = toRefs(props);

    const addElement = (element) => {
      emit('add-element', element);
    };

    return {
      elements,
      addElement
    };
  }
});
</script>

<style scoped>
.form-element-list {
  padding: 10px;
}

.form-element-list ul {
  list-style-type: none;
  padding: 0;
}

.form-element-list li {
  cursor: pointer;
  padding: 5px 0;
}

.form-element-list li:hover {
  background-color: #e0e0e0;
}
</style>

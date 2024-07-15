<template>
  <div id="app" class="app" @click="unselectedElements">
    <div class="sidebar">
      <FormElementList :elements="availableElements" @add-element="addElementToForm" />
    </div>
    <div class="form-area">
      <FormEditor :selectedElement="selectedElement" @changeWidth="updateElement" @move-element="moveElement"
        @delete-element="deleteElement" @click.stop />
      <FormView :formElements="formElements" @select-element="selectElementForEdit" />

    </div>

  </div>
</template>

<script>
import { ref } from 'vue';
import { v4 as uuidv4 } from 'uuid';
import FormElementList from './components/FormElementList.vue';
import FormEditor from './components/FormEditor.vue';
import FormView from './components/FormView.vue';

export default {
  name: 'App',
  components: {
    FormElementList,
    FormEditor,
    FormView
  },
  setup() {
    const availableElements = ref([
      {
        id: 1,
        name: 'Тема',
        type: 'string',
        placeholder: 'Введите тему'
      },
      {
        id: 2,
        name: 'Согласующие',
        type: 'ref.users',
        placeholder: 'Выберите согласующих',
        options: ['Иван Петров', 'Алексей Смирнов', 'Дмитрий Иванов', 'Сергей Кузнецов', 'Андрей Соколов', 'Николай Попов', 'Михаил Лебедев', 'Владимир Новиков', 'Александр Морозов', 'Евгений Волков']
      },
      {
        id: 3,
        name: 'Дата документа',
        type: 'date',
        placeholder: 'Выберите дату'
      },
    ]);

    const formElements = ref([]);
    const selectedElement = ref(null);

    const addElementToForm = (element) => {
      formElements.value.push({ ...element, uniqueId: uuidv4(), width: 4, top: 0 });
    };

    const updateElement = (updatedElement) => {
      const index = formElements.value.findIndex(el => el.uniqueId === updatedElement.uniqueId);
      if (index !== -1) {
        formElements.value[index] = updatedElement;
      }
    };

    const selectElementForEdit = (element) => {
      formElements.value.forEach(el => {
        el.selected = false;
      });
      element.selected = true;
      selectedElement.value = element;
    };

    const moveElement = (direction) => {
      const index = formElements.value.findIndex(el => el.uniqueId === selectedElement.value.uniqueId);
      if (index !== -1) {
        if (direction === 'up' && index > 0) {
          const temp = formElements.value[index];
          formElements.value[index] = formElements.value[index - 1];
          formElements.value[index - 1] = temp;
        } else if (direction === 'down' && index < formElements.value.length - 1) {
          const temp = formElements.value[index];
          formElements.value[index] = formElements.value[index + 1];
          formElements.value[index + 1] = temp;
        }
      }
    };

    const deleteElement = () => {
      const index = formElements.value.findIndex(el => el.uniqueId === selectedElement.value.uniqueId);
      if (index !== -1) {
        formElements.value.splice(index, 1);
        selectedElement.value = null;
      }
    };

    const unselectedElements = () => {
      formElements.value.forEach(item => {
        item.selected = false;
      });
      selectedElement.value = null;
    };

    return {
      availableElements,
      formElements,
      selectedElement,
      addElementToForm,
      updateElement,
      selectElementForEdit,
      moveElement,
      deleteElement,
      unselectedElements
    };
  }
};
</script>

<style>
.app {
  display: flex;
  height: 100vh;
}

.sidebar {
  width: 20%;
  background-color: #f5f5f5;
}

.form-area {
  width: 80%;
  background-color: #fff;
}
</style>

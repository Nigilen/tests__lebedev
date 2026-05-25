<script setup lang="ts">
import { ref } from 'vue';
import FormGenerator from './components/FormGenerator.vue';
import AppPopup from './components/AppPopup.vue';
import data from './data.json';

const fields = data.fields;

const formData = ref<Record<string, string | boolean>>({});

const handleSubmit = () => {
  console.log(JSON.stringify(formData.value, null, 2));
  formData.value = {};
  
  openPopup();
};

const isPopupOpen = ref(false);

const openPopup = () => {
  isPopupOpen.value = true;
};

const closePopup = () => {
  isPopupOpen.value = false;
};

</script>

<template>
  <FormGenerator 
    :fields="fields" 
    v-model:formData="formData"
    @submit="handleSubmit" 
  />
  <div class="form-data">
    <h2 class="form-data__title">
      Введенные данные
    </h2>
    <p v-for="field in fields" :key="field.model" class="form-data__item">
      {{ field.label }}: {{ formData[field.model] }}
    </p>
  </div>

  <Teleport to="body">
    <AppPopup 
      :isPopupOpen="isPopupOpen" 
      title="Форма отправлена" 
      @close="closePopup" 
    />
  </Teleport>
</template>

<style scoped>
.form-data {
  inline-size: clamp(300px, 90vi, 400px);
  display: flex;
  flex-direction: column;
  border: 1px solid rgba(0, 0, 0, .1);
  border-radius: 4px;
  row-gap: 8px;
  padding: 16px;
  margin-block-start: 16px;
}

.form-data__title {
  font-size: 20px;
  font-weight: bold;
  padding: 0;
  margin: 0;
  text-transform: uppercase;
}

.form-data__item {
  font-size: 16px;
  padding: 0;
  margin: 0;
}
</style>
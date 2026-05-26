<script setup lang="ts">
import { reactive, ref } from 'vue';

const props = defineProps<{
  fields: {
    type: string;
    label: string;
    model: string;
    required?: boolean;
    options?: string[];
    minLength?: number;
    maxLength?: number;
    pattern?: string;
  }[];
}>();

const emit = defineEmits<{
  (e: 'submit'): void
}>();

const formData = defineModel<Record<string, string | boolean>>('formData', 
  {
    default: () => ({}),
  }
);

const formRef = ref<HTMLFormElement | null>(null);
const errors = reactive<Record<string, string>>({});
const attemptedSubmit = ref(false);

const validateField = (model: string) => {
  const el = formRef.value?.elements.namedItem(model) as HTMLInputElement | HTMLSelectElement | null;
  if (!el) return;
  if (el.validity.valid) {
    delete errors[model];
    return;
  } 
  if (attemptedSubmit.value) {
    errors[model] = el.validationMessage
  }
}

const validateForm = () => {
  props.fields.forEach((field) => validateField(field.model));
}

const onFieldInput = (model: string) => {
  validateField(model)
}

const onSubmit = () => {
  attemptedSubmit.value = true;
  validateForm();
  if (!formRef.value?.checkValidity()) {
    return;
  }
  emit('submit');
}

</script>

<template>
  <form ref="formRef" novalidate class="form" action="" @submit.prevent="onSubmit">

    <label 
      v-for="field in fields"
      :class="['form__label', `form__label--${field.type}`]" 
      :for="field.model">
      <span :class="['form__label-text', `form__label-text--${field.type}`]">
        {{ field.label }}
      </span>
      <select 
        v-if="field.type === 'select'" 
        class="form__select" 
        :id="field.model" 
        :name="field.model" 
        :required="field.required"
        v-model="formData[field.model]"
        @change="onFieldInput(field.model)"

      >
        <option value="" disabled selected>
          Выберите {{ field.label.toLowerCase() }}
        </option>
        <option class="form__option" v-for="option in field.options" :value="option" >
          {{ option }}
        </option>
      </select>
      <input v-else 
        :class="['form__input', `form__input--${field.type}`]" 
        :type="field.type" 
        :id="field.model" 
        :name="field.model" 
        :minlength="field.minLength"
        :maxlength="field.maxLength"
        :required="field.required"
        :pattern="field.pattern"
        v-model="formData[field.model]"
        @input="onFieldInput(field.model)"
      />
      <span v-if="errors[field.model]" class="form__input-error">
        {{ errors[field.model] }}
      </span>
    </label>
    <button class="form__button" type="submit">Отправить</button>
  </form>
</template>

<style scoped>

.form {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  inline-size: clamp(300px, 90vi, 400px);
  border: 1px solid rgba(0, 0, 0, .1);
  border-radius: 4px;
  padding: 12px;
}

.form__label {
  position: relative;
  display: grid;
  grid-template-columns: 2fr 7fr;
  align-items: center;
  column-gap: 8px;
  inline-size: 100%;
}

.form__input {
  background-color: transparent;
  padding-block: 6px;
  inline-size: 100%;
  border: none;
  border-block-end: 1px solid rgba(0, 0, 0, .1);
  transition: border-block-end 0.3s ease-in-out;
}

.form__input:focus {
  border-block-end: 1px solid rgba(0, 0, 0, .8);
}

.form__input--checkbox {
  inline-size: max-content;
}

.form__label-text--checkbox {
  order: 1;
  inline-size: 100%;
}

.form__label--checkbox {
  gap: 1rem;
  grid-template-columns: auto 1fr;
}

.form__select {
  position: relative;
  inline-size: 100%;
  font: inherit;
  padding-block: 12px;
  border: none;
  border-block-end: 1px solid rgba(0, 0, 0, .1);
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  border-radius: 0;
  background-color: transparent;
  transition: border-block-end 0.3s ease-in-out;
  z-index: 1;
}

.form__label--select::after {
  content: '';
  display: block;
  position: absolute;
  inset-inline-end: 12px;
  margin-block: auto;
  inline-size: 10px;
  block-size: 10px;
  background-color: #181818;
  clip-path: polygon(0 0, 100% 0, 50% 100%);
}

.form__select:focus {
  outline: none;
  border-block-end: 1px solid rgba(0, 0, 0, .8);
}

.form__button {
  padding-block: 12px;
  background-color: #181818;
  color: #d3d3d3;
  border-radius: 4px;
  cursor: pointer;
}

.form__input-error {
  position: absolute;
  grid-column: 2 / -1;
  grid-row: 2;
  inset-block-start: 100%;
  inset-inline-start: 0;
  color: #e72424;
  font-size: 10px;
}
</style>
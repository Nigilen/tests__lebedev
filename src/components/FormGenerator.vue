<script setup lang="ts">

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

</script>

<template>
  <form class="form" action="" @submit.prevent="emit('submit')">

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
      >
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
      />

    </label>

    <button type="submit" class="form__button">Отправить</button>
  </form>
</template>

<style scoped>

.form {
  display: flex;
  flex-direction: column;
  gap: 16px;

  inline-size: clamp(300px, 90vi, 400px);
  border: 1px solid rgba(0, 0, 0, .1);
  border-radius: 4px;
  padding: 12px;
}

.form__label {
  display: flex;
  align-items: center;
  column-gap: 8px;
  inline-size: 100%;
}

.form__label-text:not(.form__label-text--checkbox) {
  flex-basis: 20%;
}

.form__input {
  padding-block: 12px;
  inline-size: 100%;
  border: none;
  border-block-end: 1px solid rgba(0, 0, 0, .1);
}



.form__input--checkbox {
  inline-size: max-content;
}

.form__label-text--checkbox {
  order: 1;
  inline-size: 100%;
}

.form__label--checkbox {
  inline-size: 100%;
  gap: 1rem;
  grid-template-columns: auto 1fr;
}

.form__select {
  inline-size: 100%;
  font: inherit;
  padding-block: 12px;
  border: none;
  border-block-end: 1px solid rgba(0, 0, 0, .1);
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  border-radius: 0;
}

.form__select:focus {
  outline: none;
}

.form__button {
  padding-block: 12px;
  background-color: #181818;
  color: #d3d3d3;
  border-radius: 4px;
  cursor: pointer;
}

</style>
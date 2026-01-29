<script lang="ts" setup>
// import { Field } from "vee-validate";

const props = defineProps<{
  label: string;
  name: string;
  error?: string;
  type?: "text" | "textarea" | "number";
  num?: string;
  disabled?: boolean;
}>();
</script>

<template>
  <fieldset class="fieldset">
    <legend class="fieldset-legend">
      {{ props.label }}
    </legend>
    <Field
      :as="type === 'textarea' ? 'textarea' : 'input' "
      :name="props.name"
      :type="props.type"
      :disabled="disabled"
      class="w-full"
      :class="{
        'input-error': props.error,
        'input': !type || type === 'text' || type === 'number',
        'textarea': type === 'textarea',
      }"
    />
    <p v-if="props.error" class="fieldset-label text-error">
      {{ props.error }}
    </p>
  </fieldset>
</template>

<template>
  <div>
    <label :for="id" v-text="props.label"></label>
    <input :id="id" :type="props.as" v-model="model" :required/>
    <small v-if="props.errors" v-text="props.errors"></small>
  </div>
</template>

<script lang="ts" setup>
  import { useId } from 'vue';
  const id = useId()
  const model = defineModel()
  const props = withDefaults(
    defineProps<{
      as?: string,
      label: string,
      errors?: string,
      required?: boolean
    }>(), {
      as: 'text',
      required: true
  })

</script>
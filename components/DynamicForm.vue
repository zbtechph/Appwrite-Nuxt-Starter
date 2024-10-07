<template>
  <div>
    <form>
      <div v-for="schema in props.schemas"
      >
        <MyInput :as="schema.as" :label="schema.label"/>
      </div>
    </form>
  </div>
</template>

<script lang="ts" setup>
  import { useForm } from 'vee-validate'
  import * as zod from 'zod';
  import { toTypedSchema } from '@vee-validate/zod';
  import type { FormItemSchema } from '~';

  const props = defineProps<{
    schemas: FormItemSchema[]
  }>()

  const { values, errors, defineField } = useForm({
    validationSchema: toTypedSchema(zod.object(props.schemas.reduce((acc: Record<string, zod.ZodTypeAny>, cv) => {
      if(!Object.keys(acc).includes(cv.name)){
        acc[cv.name] = cv.rules
      }
      return acc
    }, {})))
  })

  const fields = props.schemas.map(schema => schema.name).map(field => defineField(field)[0])


</script>
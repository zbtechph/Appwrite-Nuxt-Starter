<script lang="ts" setup>
  import * as zod from 'zod';
  import { useForm } from 'vee-validate'
  import { toTypedSchema } from '@vee-validate/zod';

  const router = useRouter()
  const account = useMyAccountStore()

  const { defineField, values, errors, setFieldError } = useForm({
    validationSchema: toTypedSchema(zod.object({
      name: zod.string().trim().min(4),
      email: zod.string().trim().email(),
      password: zod.string().min(8)
    }))
  })

  const [ name ] = defineField('name')
  const [ email ] = defineField('email')
  const [ password ] = defineField('password')

  async function createAccount(){
    if(Object.keys(errors.value).length === 0
      && values.email && values.name && values.password
    ){
      try {
        await account.createByEmailPassword(
          values.email,
          values.password,
          values.name
        )
        alert("Account created successfully!")
        navigateTo("/login")
      } catch (err) {
        if(err instanceof Error)
          setFieldError("email", err.message)
      }
    } else {
      alert("Failed Validation")
    }
  }

  const { subscribe } = useGuestWatcher()
  subscribe()

  definePageMeta({
    middleware: "guest"
  })

  useHead({
    title: "Registration"
  })

</script>

<template>
  <div>
    <h1>Create an account</h1>
    <form @submit.prevent="createAccount">
      <MyInput as="text" label="Name" v-model="name" :errors="errors.name"/>
      <MyInput as="email" label="Email" v-model="email" :errors="errors.email"/>
      <MyInput as="password" label="Password" v-model="password" :errors="errors.password"/>
      <button type="submit">Sign Up</button>
    </form>
  </div>
</template>


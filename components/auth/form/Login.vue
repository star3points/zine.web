<script lang="ts" setup>

import {type InferType, object, string} from 'yup'
import type {FormSubmitEvent} from '#ui/types'

const schema = object({
  login: string().required('Required') || string().email('Invalid email').required('Required'),
  password: string()
    .min(6, 'Must be at least 6 characters')
    .required('Required')
})

type Schema = InferType<typeof schema>

const state = reactive({
  login: undefined,
  password: undefined
})

const authStore = useAuthStore()

async function onSubmit(event: FormSubmitEvent<Schema>) {
  //TODO: error handler
  await authStore.login(event.data.login, event.data.password)
  navigateTo('/me')
}

</script>

<template>
  <UForm :schema="schema" :state="state" class="space-y-4" @submit="onSubmit">
    <UFormGroup label="Username or Email" name="credential">
      <UInput v-model="state.login"/>
    </UFormGroup>

    <UFormGroup label="Password" name="password">
      <UInput v-model="state.password" type="password"/>
    </UFormGroup>

    <UButton block type="submit">
      Login
    </UButton>
  </UForm>
</template>

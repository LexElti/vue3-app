<template>
  <form
    class="card"
    @submit.prevent="onSubmit"
  >
    <h1>Войти в систему</h1>
    <div :class="['form-control', { invalid: eError }]">
      <label for="email">Email</label>
      <input
        type="email"
        id="email"
        v-model="email"
        @blur="eBlur"
      />
      <small v-if="eError">{{ eError }}</small>
    </div>
    <div :class="['form-control', { invalid: pError }]">
      <label for="password">Пароль</label>
      <input
        type="password"
        id="password"
        v-model="password"
        @blur="pBlur"
      />
      <small v-if="pError">{{ pError }}</small>
    </div>
    <button
      type="submit"
      :disabled="isSubmitting || isTooManyAttempts"
      class="btn primary"
    >
      Войти
    </button>
    <div
      v-if="isTooManyAttempts"
      class="text-danger"
    >
      Вы слишком часто пытаетесь войти в систему. Попробуйте позже
    </div>
  </form>
</template>

<script>
import { useRoute } from 'vue-router'
import { useStore } from 'vuex'
import { error } from '../utils/error'
import { useLoginForm } from '../use/login-form'

export default {
  setup() {
    const route = useRoute()
    const store = useStore()

    if (route.query.message) {
      store.dispatch('setMessage', {
        value: error(route.query.message),
        type: 'warning'
      })
    }

    return { ...useLoginForm() }
  }
}
</script>

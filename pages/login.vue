<script setup lang="ts">
const client = useSupabaseClient()
const email = ref("")
const password = ref("")
const errorMsg = ref("")
const successMsg = ref("")
const isSignIn = ref(true)

const toggleIsSignIn = () => {
  isSignIn.value = !isSignIn.value
  errorMsg.value = ""
  successMsg.value = ""
}
async function signUp() {
  try {
    errorMsg.value = ""
    successMsg.value = ""
    const { data, error } = await client.auth.signUp({
      email: email.value,
      password: password.value,
    })
    if (error) throw error
    successMsg.value = "Sign up successful"
  } catch (error: unknown) {
    if (typeof error === "string") {
      errorMsg.value = error
    } else if (error instanceof Error) {
      errorMsg.value = error.message
    }
  }
}

async function signIn() {
  try {
    errorMsg.value = ""
    successMsg.value = ""
    const { data, error } = await client.auth.signInWithPassword({
      email: email.value,
      password: password.value,
    })
    if (error) throw error
    successMsg.value = "Sign in successful"
  } catch (error: unknown) {
    if (typeof error === "string") {
      errorMsg.value = error
    } else if (error instanceof Error) {
      errorMsg.value = error.message
    }
  }
}

const user = useSupabaseUser()

onMounted(() => {
  watchEffect(() => {
    if (user.value) {
      navigateTo("/spellbooks")
    }
  })
})

const emailRules = [(value: any) => {
  if (value) return true
  return "Email is required"
}
]
const passwordRules = [(value: any) => {
  if (value) return true
  return "Password is required"
},
(value: any) => {
  if (value?.length >= 6) return true
  return "bruh, passwords should be more than 5 characters"
}
]
</script>

<template>
  <h1>+1 Power</h1>
  <v-form @submit.prevent="isSignIn ? signIn() : signUp()" validate-on="input">
    <v-text-field :rules="emailRules" label="Email address" placeholder="johndoe@gmail.com" type="email"
      v-model="email"></v-text-field>
    <v-text-field :rules="passwordRules" label="Password" type="password" v-model="password"> </v-text-field>
    <v-btn type="submit" block>{{ isSignIn ? "Sign In" : "Sign Up" }}</v-btn>
  </v-form>
  <span>{{ successMsg ? successMsg : errorMsg }}</span>
  <button @click="toggleIsSignIn" class="w-full mt-8 text-sm text-center underline text-slate-300">
    <span v-if="isSignIn"> Create a new account </span>
    <span v-else> Have an account? Log in instead </span>
  </button>
</template>

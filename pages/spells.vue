<script setup lang="ts">
definePageMeta({
  middleware: ['auth']
})
const client = useSupabaseClient()
async function signOut() {
  try {
    const { error } = await client.auth.signOut()
    if (error) throw error
    navigateTo('/login')
  } catch (error) {
    console.log(error)
  }
}
const user = useSupabaseUser()
</script>

<template>
  <v-btn block @click="signOut">Sign Out</v-btn>
</template>

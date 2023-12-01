



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
onMounted(() => { signOut() })
</script>

<template>
  <div>See ya</div>
</template>

<template>
  <div>
    <h1>Hello this is the index.vue</h1>
  </div>
</template>

<script setup>
const user = useSupabaseUser()
onMounted(() => {
  watchEffect(() => {
    if (user.value) {
      navigateTo("/spellbooks")
    }
  })
})
</script>

<style scoped>
</style>

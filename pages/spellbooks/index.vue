<template>
  <SpellBookCard v-for="spellBook in spellBooks" :spellBook="spellBook"></SpellBookCard>
</template>

<script setup>
definePageMeta({
  middleware: ['auth']
})
const client = useSupabaseClient()
const user = useSupabaseUser()
const { data: spellBooks } = await useAsyncData('spellBooks', async () => {
  return await client.from('spell_books').select().eq('user_id', user.value.id)
}, { transform: result => result.data })

</script>

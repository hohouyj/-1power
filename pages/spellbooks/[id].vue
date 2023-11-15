<template>
  <h1>{{ spellBook?.name + " Power " + spellBook?.power }}</h1>
  <SpellCard v-for="spell in spells" :spell="spell"></SpellCard>
  <v-btn @click="addSpell">Add Spell</v-btn>
</template>

<script setup>

const spellBookId = useRoute().params.id
const client = useSupabaseClient()

const { data: spells } = await useAsyncData('spells', async () => {
  return await client.from('spells').select().eq('spell_book_id', spellBookId)
}, { transform: result => result.data })

const { data: spellBook } = await useAsyncData('spellBooks', async () => {
  return await client.from('spell_books').select().eq('id', spellBookId)
}, { transform: result => { return result.data ? result.data.at(0) : { name: "unknown", power: 0 } } })


async function addSpell() {
  try {
    const update = {
      description: "spell description",
      name: "spell name",
      rank: 0,
      spell_book_id: spellBookId,
      tradition: "Air"
    }
    const { data, error } = await client.from('spells').upsert(update)

    if (error) throw error
  } catch {
    alert(error.message)
  } finally {
    const { data: spells } = await useAsyncData('spells', async () => {
      return await client.from('spells').select().eq('spell_book_id', spellBookId)
    }, { transform: result => result.data })
  }
}


</script>

<style scoped>
</style>

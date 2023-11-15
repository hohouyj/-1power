<template>
  <div>

    <v-form v-if="isEditing">
      <v-card>
        <template v-slot:title><v-text-field v-model="name" label="Spell Name" required hide-details>
          </v-text-field></template>
        <template v-slot:subtitle>
          <v-autocomplete v-model="power" label="Power" :items="[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]">
          </v-autocomplete>
        </template>
        <v-card-actions><v-btn @click="updateSpellBook"><v-icon icon="mdi-floppy" />Save</v-btn></v-card-actions>
      </v-card>
    </v-form>
    <v-card v-else :title="name + ' Power: ' + power">
      <v-card-actions>
        <v-btn @click="isEditing = !isEditing"><v-icon icon="mdi-pencil" />Edit</v-btn>
        <v-btn @click="navigateTo('/spellbooks/' + id)"><v-icon icon="mdi-door" />Spells</v-btn>
      </v-card-actions>

    </v-card>
  </div>
</template>

<script setup>
const props = defineProps(['spellBook'])
const id = ref(props.spellBook.id)
const name = ref(props.spellBook.name)
const power = ref(props.spellBook.power)
const isEditing = ref(false)
const client = useSupabaseClient()
async function updateSpellBook() {
  try {
    const update = {
      id: id.value,
      name: name.value,
      power: power.value,
      created_at: new Date(),
    }
    const { data, error } = await client.from('spell_books').upsert(update)
    if (error) throw error
  } catch {
    alert(error.message)
  } finally {
    isEditing.value = !isEditing.value
  }
}


</script>

<style scoped>
</style>

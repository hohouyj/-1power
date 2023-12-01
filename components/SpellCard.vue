<template>
  <div>
    <v-form v-if="isEditing">
      <v-card>
        <template v-slot:title>
          <v-text-field v-model="spellName" label="Spell Name" required hide-details>
          </v-text-field>
        </template>
        <template v-slot:subtitle>
          <v-autocomplete v-model="spellTradition" label="Spell Tradition" :items="[
            'Air',
            'Alchemy',
            'Alteration',
            'Arcana',
            'Battle',
            'Blood',
            'Celestial',
            'Chaos',
            'Conjuration',
            'Curse',
            'Death',
            'Demonology',
            'Destruction',
            'Divination',
            'Earth',
            'Enchantment',
            'Fey',
            'Fire',
            'Forbidden',
            'Illusion',
            'Life',
            'Nature',
            'Necromancy',
            'Primal',
            'Protection',
            'Rune',
            'Shadow',
            'Song',
            'Spiritualism',
            'Storm',
            'Technomancy',
            'Telekinesis',
            'Telepathy',
            'Teleportation',
            'Theurgy',
            'Time',
            'Transformation',
            'Water']">
          </v-autocomplete>
          <v-autocomplete v-model="spellRank" label="Rank" :items="[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]">
          </v-autocomplete>
        </template>
        <template v-slot:text>
          <v-textarea v-model="spellDescription" label="Spell Description" required hide-details>
          </v-textarea>
        </template>
        <v-card-actions><v-btn @click="updateSpell"><v-icon icon="mdi-floppy" />Save</v-btn></v-card-actions>

      </v-card>
    </v-form>
    <v-card v-else :title="spellName" :subtitle="spellTradition + ' Rank ' + spellRank" :text="spellDescription">
      <v-card-actions><v-btn @click="isEditing = !isEditing"><v-icon icon="mdi-pencil" />Edit</v-btn></v-card-actions>
    </v-card>
    <pre>
      {{ castingsMatrix.at(power).at(spellRank) }}
    </pre>
  </div>
</template>

<script setup>

const castingsMatrix = ref(
  [
    [1],
    [2, 1],
    [3, 2, 1],
    [4, 2, 1, 1],
    [5, 2, 2, 1, 1],
    [6, 3, 2, 2, 1, 1],
    [7, 3, 2, 2, 2, 1, 1],
    [8, 3, 2, 2, 2, 1, 1, 1],
    [9, 3, 3, 2, 2, 2, 1, 1, 1],
    [10, 3, 3, 3, 2, 2, 1, 1, 1, 1],
    [11, 3, 3, 3, 3, 2, 1, 1, 1, 1, 1]
  ])

const props = defineProps(['spell', 'power'])

const isEditing = ref(false)
const spellName = ref(props.spell.name)
const spellId = ref(props.spell.id)
const spellRank = ref(props.spell.rank)
const spellBookId = ref(props.spell.spell_book_id)
const spellTradition = ref(props.spell.tradition)
const spellDescription = ref(props.spell.description)
const loading = ref(false)
const client = useSupabaseClient()
const power = ref(props.power ? props.power : 0)

async function updateSpell() {
  try {
    loading.value = true
    const update = {
      description: spellDescription.value,
      id: spellId.value,
      name: spellName.value,
      rank: spellRank.value,
      spell_book_id: spellBookId.value,
      tradition: spellTradition.value,
      created_at: new Date(),
    }
    const { data, error } = await client.from('spells').upsert(update)
    if (error) throw error
  } catch {
    alert(error.message)
  } finally {
    loading.value = false
    isEditing.value = !isEditing.value
  }
}
</script>

<style scoped>
</style>

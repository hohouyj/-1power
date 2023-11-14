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
  <div>
    <v-card title="Spellname" subtitle="SpellType and Rank" text="Spell Description">
    </v-card>
    <v-form>
      <v-card>
        <template v-slot:title>
          <v-text-field label="Spell Name" required hide-details>
          </v-text-field>
        </template>
        <template v-slot:subtitle>
          <v-autocomplete label="Spell Tradition" :items="[
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
          <v-autocomplete label="Rank" :items="['0', '1', '2', '3', '4', '5']">
          </v-autocomplete>
        </template>
        <template v-slot:text>
          <v-textarea label="Spell Description" required hide-details>
          </v-textarea>
        </template>
      </v-card>
    </v-form>
  </div>
  <pre>{{ user }}</pre>
</template>
<!--
TODO:
- Save Changes to DB
- Display from DB
-->

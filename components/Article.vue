<template>
  <div class="grid grid-cols-3 gap-8">
    <article
      v-for="character in characters.results"
      :key="character.id"
      class="flex flex-col items-start"
    >

      <h2 class="font-bold">{{ character.name }}</h2>
      <div class="text-sm text-grey-800">
        <p> <u>Genre</u>: {{ character.gender }}</p>
        <p> <u>Localisation dans la série</u>: {{ character.location.name }}</p>
      </div>

<!--      balise nuxt equivalent du a href-->
      <NuxtLink
        :to="`/character/${ character.id }`"
        class="border rounded px-2 py-1 text-gray-800 bg-green-500 text-sm hover:text-white hover:transition delay-150 duration-300 ease-in-out"
      >
        Lire plus
      </NuxtLink>
    </article>
  </div>

</template>

<script>

import gql from 'graphql-tag'

//constante avec dedans la query graph ql qui sélectionnera tout les characters avec leur id leur nom leur genre et le nom de leur localisation
const ALL_CHARACTERS_QUERY = gql`
    query ALL_CHARACTERS_QUERY{
      characters{
        results{
          id
          name
          gender
          location{
              name
          }
        }
      }
    }
  `;

export default {
  name: "Article",
  apollo: {

    //envoie de la constante ALL_CHARACTERS_QUERY qui est notre requete que nous foutons dans une variable characters pour un foreach
    characters: {
      query: ALL_CHARACTERS_QUERY,

      /*La prélecture implique l'exécution de requêtes pour les données avant que ces données ne soient rendues. Cela aide l'interface utilisateur de votre application à être plus réactive pour l'utilisateur.
      La plupart du temps, la prélecture implique d'interroger des données dès que vous pouvez deviner qu'un utilisateur en aura probablement besoin.*/
      prefetch: true,
    },
  },
}
</script>

<style scoped>

</style>

<template>
  <div class="grid grid-cols-3 gap-8">
    <article
      v-for="annonce in base_annonce"
      :key="annonce.id"
      class="flex flex-col items-start"
    >

      <div v-for="image in annonce.images">
        <img :src="require(`~/assets/img/${image.nom}`)" alt="" id="photo">
      </div>

      <div class="text-sm text-grey-800">
        <p> <u>Extrait</u>: {{ annonce.extrait }}</p>
      </div>

      <NuxtLink
        :to="`/annonce/${ annonce.id }`"
        class="border
        rounded px-2
        py-1
        text-gray-800
        bg-green-500
        text-sm
        hover:text-white
        hover:transition
        delay-150
        duration-300
        ease-in-out"
      >
        Lire plus
      </NuxtLink>
    </article>
  </div>

</template>

<script>

import gql from 'graphql-tag'

const ALL_ANNONCES_QUERY = gql`
    query ALL_ANNONCE_QUERY{
      base_annonce {
        id
        afficher
        archive
        description
        extrait
        genere
        id_categorie
        titre
        ordre
        images {
          id
          nom
          recadree
          titile
          ordre
          legende
        }
      }
    }
  `;

export default {
  name: "Article",
  data() {
    return {
      results: [],
    }
  },

  apollo: {
    base_annonce: {
      query: ALL_ANNONCES_QUERY,
      prefetch: true,
    },
  },
}
</script>

<style scoped>

</style>

<template>
  <div>
    <div>
      <Navbar />
    </div>

    <div class="flex items-stretch">
      <div class="py-12 px-20">
        <form
          @submit.prevent="search"
        >

          <input
            type="text"
            name="Search"
            id="Search"
            class="border border-gray-600 rounded py-2 px-6 "
            v-model="searchText"
            placeholder="Recherche par nom"
          >

          <button
            type="submit"
            class="bg-gray-200 px-4 py-2 border border-gray-200 rounded"
          >
            Recherche
          </button>
        </form>

        <div v-if="loading">Chargements...</div>

        <div v-if="searchResults" class="mb-8">
          <div v-if="searchResults.length">
            <p class="font-bold mb-2">Les résultats de la recherche</p>
            <div class="flex flex-wrap">
              <div
                v-for="character in searchResults"
                :key="character.id"
                class="mr-4 mb-2 flex"
              >
                <NuxtLink
                  :to="`/character/${character.id}`"
                  class="border rounded px-2 py-1 text-gray-800 border-gray-800 text-sm mt-2 whitespace-no-wrap"
                >
                  {{ character.name }}
                </NuxtLink>
              </div>
            </div>
          </div>
          <p
            class="font-bold"
            v-else
          >
            Aucun résultat trouvé
          </p>
        </div>
      </div>
      </div>

  </div>


</template>

<script>
  import gql from "graphql-tag";
  import Navbar from "../layouts/Navbar";

  const CHARACTERS_BY_NAME_QUERY = gql `
    query CHARACTERS_BY_NAME_QUERY($name: String!){
        characters(filter: { name : $name }){
            results {
                id
                name
            }
        }
    }
  `;

  export default {
    components: {Navbar},
    data() {
      return {
        searchText: '',
        searchResults: '',
        loading: false
      }
    },

    methods: {
      async search() {
        try{
          this.loading = true
          const res = await this.$apollo.query({
            query: CHARACTERS_BY_NAME_QUERY,
            variables: {
              name: this.searchText
            }
          });

          if (res){
            this.loading = false;
            const { results } = res.data.characters;
            this.searchResults = results;

          }
        } catch (err)
        {
          this.loading = false;
          this.searchResults = [];
        }
      }
    }
  }
</script>

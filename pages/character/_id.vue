<template>
  <div>
    <Navbar />
    <div class="flex flex-col items-start p-4 space-y-4">
      <NuxtLink
        to="/"
        class="border rounded px-2 py-1 text-gray-800 bg-green-500 text-sm hover:text-white hover:transition delay-150 duration-300 ease-in-out"
      >
        Retour
      </NuxtLink>
      <h1 class="font-bold text-2xl">{{ character.name }}</h1>
      <hr class="w-full">
      <div class="text-gray-800">
        <img :src="character.image" alt="" id="photo">
        <p> <u>Genre</u>: {{ character.gender }}</p>
        <p> <u>Spécimen</u>: {{ character.species }}</p>
        <p><u>Origine</u> : {{ character.origin.name }}</p>
      </div>
      <hr class="w-full">
      <h2 class="font-bold mb-2">Est apparu dans :</h2>
      <div class="grid grid-cols-4 gap-4">
        <article
          v-for="episode in character.episode"
          :key="episode.id"
        >
          <h3 class="font-bold">{{ episode.name }}</h3>
          <p> <u>Date de l'épisode</u>  : {{ episode.air_date }}</p>
        </article>
      </div>

    </div>
  </div>

</template>

<script>
  import gql from "graphql-tag";
  import Navbar from "../../layouts/Navbar";

  const SINGLE_CHARACTER_QUERY = gql`
    query SINGLE_CHARACTER_QUERY ($id: ID!){
      character(id: $id){
        name
        species
        gender
        origin{
            name
        }
        episode{
          id
          name
          air_date
        }
        image
      }
    }
  `;

  export default {
    components: {Navbar},

    //Ici, nous utilisons la méthode asyncData de Nuxt pour récupérer des données avec notre client Apollo préconfiguré. En déstructurant le paramètre ID de l'URL, nous pouvons le transmettre à la requête GraphQL dans l'objet "variables".
    //asyncData est seulement disponible pour les pages et nous n'avons donc pas accès à this à l'intérieur du hook.
    async asyncData({params, app}){

      /*Apollo Client est une bibliothèque complète de gestion d'état pour JavaScript qui vous permet de gérer à la fois les données locales et distantes avec GraphQL. Utilisez-le pour récupérer, mettre en cache et modifier les données d'application, tout en mettant automatiquement à jour votre interface utilisateur.*/
      const client = app.apolloProvider.defaultClient;

      //variable id envoyé dans l'url
      const { id } = params;


      const res = await client.query({
        query: SINGLE_CHARACTER_QUERY,
        prefetch: true,
        variables: {
          id
        }
      })

      //on a accès a data grâce à asyncData
      const { character } = res.data;

      return{
        character
      }
    }
  }
</script>

<style scoped>
  #photo{
    width: 150px;
  }
</style>

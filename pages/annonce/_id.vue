<template>
  <div>
    <Navbar />
    <div
      class="flex flex-col items-start p-4 space-y-4"
      v-for="annonce in annonce.base_annonce"
    >

      <NuxtLink
        to="/"
        class="border
        rounded
        px-2
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
        Retour
      </NuxtLink>
      <h1 class="font-bold text-2xl">{{ annonce.titre }}</h1>
      <hr class="w-full">
      <div
        class="text-gray-800"
        v-for="image in annonce.images"
        :key="image.id"
      >
        <img :src="require(`~/assets/img/${image.nom}`)" alt="" id="photo">
      </div>
      <p v-html="annonce.description"></p>
      <hr class="w-full">
    </div>
  </div>

</template>

<script>
  import gql from "graphql-tag";
  import Navbar from "../../layouts/Navbar";

  const SINGLE_ANNONCE_QUERY = gql`
    query SINGLE_ANNONCE_QUERY ($id: Int!){
      base_annonce(where: {id: {_eq: $id}}) {
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
    components: {Navbar},

    //Ici, nous utilisons la méthode asyncData de Nuxt pour récupérer des données avec notre client Apollo préconfiguré. En déstructurant le paramètre ID de l'URL, nous pouvons le transmettre à la requête GraphQL dans l'objet "variables".
    //asyncData est seulement disponible pour les pages et nous n'avons donc pas accès à this à l'intérieur du hook.
    async asyncData({params, app}){

      /*Apollo Client est une bibliothèque complète de gestion d'état pour JavaScript qui vous permet de gérer à la fois les données locales et distantes avec GraphQL. Utilisez-le pour récupérer, mettre en cache et modifier les données d'application, tout en mettant automatiquement à jour votre interface utilisateur.*/
      const client = app.apolloProvider.defaultClient;

      //variable id envoyé dans l'url
      const { id } = params;

      const res = await client.query({
        query: SINGLE_ANNONCE_QUERY,
        prefetch: true,
        variables: {
          id
        }
      })

      //console.log(res.data.base_annonce[0].id)
      //on a accès a data grâce à asyncData
      const annonce = res.data;

      return{
        annonce
      }
    }
  }
</script>

<style scoped>
  #photo{
    width: 150px;
  }
</style>

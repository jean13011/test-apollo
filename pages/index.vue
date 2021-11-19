<template>
  <div>
    <Navbar />
    <div class="p-8">
      <h1 class="font-bold text-2xl mb-8">Annonce JT</h1>
      <div>
        <Article/>
      </div>
    </div>
  </div>

</template>

<script>
import Navbar from "../layouts/Navbar";
import Article from "../components/Article";

export default {
  components: {Navbar, Article}
}
</script>

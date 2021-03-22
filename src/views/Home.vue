<template>
  <div class="conts-main">
    <div class="container">
      <CardMovies />
    </div>
  </div>
</template>

<script>
import CardMovies from "../components/CardMovies";

export default {
  name: "Home",
  components: {
    CardMovies,
  },
};
</script>

<template>
  <div>
    Some
  </div>
</template>

<script>
export default {
  name: "Characters",
  data: () => ({
    api: "https://rickandmortyapi.com/api/character/"
  }),
  mounted: function() {
    this.$nextTick(function() {
      // Code that will run only after the
      // entire view has been rendered
      this.getAllLocations();
    });
  },
  methods: {
    getAllCharacters: async function() {
      const response = await fetch(this.api);
      const data = await response.json();
      this.list = data.results;
    }
  }
};
</script>

<style>
div {
  color: red;
}
</style>

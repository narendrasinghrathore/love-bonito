<template>
  <main>
    <header>
      Home
    </header>
    <section>
      <div class="loading" v-if="loading">
        <div>
          <v-progress-linear
            buffer-value="55"
            color="success"
            stream
            value="30"
          ></v-progress-linear>
        </div>
      </div>
      <div class="data" v-if="!loading">
        <v-chip
          close-icon="mdi-delete"
          v-bind:key="item.id"
          v-for="item in list"
          class="chip"
        >
          {{ item.name }}</v-chip
        >
      </div>
    </section>
    <footer>
      <v-btn
        elevation="2"
        v-if="prev"
        v-on:click="getAllLocations(prev)"
        class="btn"
        >Prev</v-btn
      >
      <v-btn
        elevation="2"
        v-if="next"
        v-on:click="getAllLocations(next)"
        class="btn"
        >Next</v-btn
      >
    </footer>
  </main>
</template>

<script>
export default {
  name: "HelloWorld",
  data: function() {
    return {
      api: "https://rickandmortyapi.com/api/location/",
      list: [],
      next: "",
      prev: "",
      loading: false
    };
  },
  props: {
    msg: String
  },
  mounted: function() {
    this.$nextTick(function() {
      // Code that will run only after the
      // entire view has been rendered
      this.getAllLocations();
    });
  },
  methods: {
    getAllLocations: async function(n) {
      this.loading = true;
      try {
        const api = n || this.api;
        const response = await fetch(api);
        const data = await response.json();
        this.list = data.results;
        const { next, prev } = data.info;
        this.next = next;
        this.prev = prev;
      } catch (e) {
        console.warn(e);
      } finally {
        this.loading = false;
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.loading {
  display: flex;
  justify-content: center;
  min-height: 200px;
  div {
    background-color: #dedede;
  }
}
.data {
  min-height: 200px;
}
.chip {
  color: darkcyan;
  margin: 3px;
}
.btn {
  margin: 3px;
}
</style>

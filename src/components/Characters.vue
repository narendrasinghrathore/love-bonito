<template>
  <div class="character-container">
    <v-card class="mx-auto" max-width="400">
      <div v-if="loading">
        <v-progress-circular
          v-if="loading"
          indeterminate
          color="primary"
          class="chip-progress"
          size="30"
        ></v-progress-circular>
      </div>
      <v-list-item two-line>
        <v-list-item-content>
          <v-list-item-title class="headline">
            {{ character.name }}
          </v-list-item-title>
          <v-list-item-subtitle>{{ character.status }}</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-card-text>
        <v-row align="center">
          <v-col class="display-3" cols="6">
            {{ character.gender }}
          </v-col>
          <v-col cols="6">
            <v-img
              v-bind:src="character.image"
              v-bind:alt="character.name"
              width="92"
            ></v-img>
          </v-col>
        </v-row>
      </v-card-text>

      <v-list class="transparent">
        <v-list-item v-for="item in character.episode" :key="item">
          <v-list-item-title>Episode</v-list-item-title>

          <v-list-item-subtitle class="text-right">
            {{ item }}
          </v-list-item-subtitle>
        </v-list-item>
      </v-list>

      <v-divider></v-divider>

      <v-card-actions>
        <v-btn v-on:click="back" text>
          Back
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
export default {
  name: "Characters",
  computed: {
    id() {
      // We will see what `params` is shortly
      return this.$route.params.id;
    }
  },
  data: () => ({
    api: "https://rickandmortyapi.com/api/character/",
    character: {},
    loading: false
  }),
  mounted: function() {
    this.$nextTick(function() {
      // Code that will run only after the
      // entire view has been rendered
      this.getCharacter();
    });
  },
  methods: {
    getCharacter: async function() {
      this.loading = true;
      try {
        const response = await fetch(`${this.api}${this.id}`);
        const data = await response.json();
        this.character = data;
      } finally {
        this.loading = false;
      }
    },
    back: function() {
      this.$router.back();
    }
  }
};
</script>

<style lang="scss" scoped>
div.character-container {
  max-width: 400px;
}
.transparent {
  max-height: 200px;
  overflow-y: auto;
}
</style>

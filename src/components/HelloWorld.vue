<template>
  <main>
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
          v-on:click="getCharactersList(item.characters, item.id, item.name)"
          v-bind:key="item.id"
          v-for="item in list"
          class="chip"
        >
          {{ item.name }}
          <v-progress-circular
            v-if="chipLoadingId === item.id"
            indeterminate
            color="primary"
            class="chip-progress"
            size="30"
          ></v-progress-circular
        ></v-chip>
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

    <div class="text-center">
      <v-dialog v-model="dialog" max-width="80vw">
        <v-card>
          <v-card-title class="headline grey lighten-2 ">
            Characters
          </v-card-title>
          <div class="main-card">
            <v-card v-bind:key="item.id" v-for="item in characters" outlined>
              <v-list-item three-line>
                <v-list-item-content>
                  <div class="overline mb-4">
                    {{ item.species }}
                  </div>
                  <v-list-item-title class="headline mb-1">
                    {{ item.name }}
                  </v-list-item-title>
                  <v-list-item-subtitle>{{ item.status }}</v-list-item-subtitle>
                </v-list-item-content>

                <v-list-item-avatar tile size="80" color="grey">
                  <img width="80" loading="lazy" v-bind:src="item.image" />
                </v-list-item-avatar>
              </v-list-item>

              <v-card-actions>
                <v-btn outlined rounded text v-on:click="exploreMore(item.id)">
                  Explore
                </v-btn>
              </v-card-actions>
            </v-card>
          </div>
          <v-divider></v-divider>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" text @click="dialog = false">
              Close
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
    <v-snackbar v-model="snackbar">
      {{ message }}

      <template v-slot:action="{ attrs }">
        <v-btn color="pink" text v-bind="attrs" @click="closeSnackbar">
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </main>
</template>

<script>
export default {
  name: "HelloWorld",
  data: function() {
    return {
      api: "https://rickandmortyapi.com/api/location/",
      charactersAPI: "https://rickandmortyapi.com/api/character/",
      list: [],
      next: "",
      prev: "",
      loading: false,
      characters: [],
      dialog: false,
      chipLoadingId: "",
      snackbar: false,
      message: ""
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
        this.list = data.results.map(item => {
          const characters = item.residents.map(char =>
            char.slice(char.lastIndexOf("/") + 1)
          );
          return { ...item, characters };
        });
        const { next, prev } = data.info;
        this.next = next;
        this.prev = prev;
        console.log(this.list);
      } catch (e) {
        console.warn(e);
      } finally {
        this.loading = false;
      }
    },
    getCharactersList: async function(list, id, name) {
      if (list.length === 0) {
        this.showMessage(name);
        return;
      }
      try {
        this.chipLoadingId = id;
        const response = await fetch(`${this.charactersAPI}${list.join()}`);
        const data = await response.json();
        this.characters = data.length ? data : [data];
        this.dialog = true;
      } catch (e) {
        console.warn(e);
      } finally {
        this.chipLoadingId = null;
      }
    },
    exploreMore: function(id) {
      this.$router.push(`/about/${id}`);
    },
    showMessage: function(name) {
      this.snackbar = true;
      this.message = `No characters for given location i.e.${name}`;
    },
    closeSnackbar: function() {
      this.snackbar = false;
      this.message = "";
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
  margin: 10px auto;
}
.chip {
  color: darkcyan;
  margin: 3px;
  .chip-progress {
    margin: auto auto auto 13px;
    color: blue;
  }
}
.btn {
  margin: 3px;
}
.main-card {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  .v-card {
    flex: 1;
    margin: 8px;
    width: 200px;
    background-color: #f3f0f0;

    text-align: start;
  }
}
</style>

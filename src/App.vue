<template>
  <div id="app">
    <div class="container mt-5">
      <div class="row">
        <div
          v-for="(pokemon, index) in pokemons"
          :key="'poke' + index"
          class="col-md-4"
        >
          <b-card
            :img-src="imageUrl + pokemon.id + '.png'"
            :title="pokemon.name"
            img-alt="Image"
            img-top
            tag="article"
            style="max-width: 20rem; "
            class="mb-2"
          >
          </b-card>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data: () => {
    return {
      imageUrl:
        'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/',
      apiUrl: 'https://pokeapi.co/api/v2/pokemon/?offset=20&limit=300',
      pokemons: [],
      pokemonUrl: '',
      showDetail: false
    };
  },
  methods: {
    fetchData() {
      let req = new Request(this.apiUrl);
      fetch(req)
        .then(resp => {
          if (resp.status === 200) return resp.json();
        })
        .then(data => {
          data.results.forEach(pokemon => {
            pokemon.id = pokemon.url
              .split('/')
              .filter(function(part) {
                return !!part;
              })
              .pop();
            this.pokemons.push(pokemon);
          });
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  created() {
    this.currentUrl = this.apiUrl;
    this.fetchData();
  }
};
</script>

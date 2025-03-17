import { defineComponent, ref, computed, onMounted } from "vue";
import { NCard, NTag, NInput } from "naive-ui";
import axios from "axios";

export default defineComponent({
  name: "PokemonList",
  components: { NCard, NTag, NInput },
  setup() {
    const searchQuery = ref("");
    const pokemons = ref([]);

    // Fetch les Pokémons depuis l'API
    const fetchPokemons = async () => {
      try {
        const response = await axios.get("https://api.seyrinian.com/pokemon");
        pokemons.value = response.data;
      } catch (error) {
        console.error("Erreur lors du chargement des Pokémons", error);
      }
    };

    onMounted(fetchPokemons);

    // Filtrage des Pokémons en fonction du champ de recherche
    const filteredPokemons = computed(() => {
      return pokemons.value.filter(pokemon =>
        pokemon.name.toLowerCase().includes(searchQuery.value.toLowerCase())
      );
    });

    return {
      searchQuery,
      filteredPokemons,
    };
  },
});

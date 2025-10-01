<script>
  import { onMount } from "svelte";

  let pokemons = [];

  // タイプごとの色
  const typeColors = {
    grass: "#78C850",
    fire: "#F08030",
    water: "#6890F0",
    bug: "#A8B820",
    normal: "#A8A878",
    poison: "#A040A0",
    electric: "#F8D030",
    ground: "#E0C068",
    fairy: "#EE99AC",
    fighting: "#C03028",
    psychic: "#F85888",
    rock: "#B8A038",
    ghost: "#705898",
    ice: "#98D8D8",
    dragon: "#7038F8",
    dark: "#705848",
    steel: "#B8B8D0",
    flying: "#A890F0",
  };

  onMount(async () => {
    const res = await fetch("https://pokeapi.co/api/v2/pokemon?limit=151");
    const data = await res.json();
    const results = data.results;

    // 詳細データをまとめて取得
    pokemons = await Promise.all(
      results.map(async (p) => {
        const res = await fetch(p.url);
        const detail = await res.json();
        return {
          id: detail.id,
          name: detail.name,
          types: detail.types.map((t) => t.type.name),
        };
      }),
    );
  });
</script>

<h1>ポケモン図鑑（1〜151）</h1>
<div class="pokemon-grid">
  {#each pokemons as pokemon}
    <div class="pokemon-card">
      <img
        src={`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png`}
        alt={pokemon.name}
      />
      <div class="pokemon-name">{pokemon.name}</div>
      <div>
        {#each pokemon.types as type}
          <span
            class="type"
            style="background-color: {typeColors[type] || '#777'}"
          >
            {type}
          </span>
        {/each}
      </div>
    </div>
  {/each}
</div>

<style>
  .pokemon-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
    gap: 1rem;
  }

  .pokemon-card {
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 0.5rem;
    text-align: center;
    background-color: #f8f8f8;
  }

  .pokemon-name {
    font-weight: bold;
    text-transform: capitalize;
  }

  .type {
    display: inline-block;
    padding: 2px 6px;
    border-radius: 12px;
    color: white;
    font-size: 0.75rem;
    margin: 0.5px;
    text-transform: capitalize;
  }
</style>

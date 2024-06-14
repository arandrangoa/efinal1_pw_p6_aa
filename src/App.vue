<template>
  <div class="app">
    <div v-if="juego">
      <div class="enunciados">
        <p>Puntos: {{ puntos }}</p>
        <p>Intentos: {{ intentos }}</p>
      </div>

      <div class="imagenes">
        <Pokemon :img="pokemon1.imagen" :text="pokemon1.text" />
        <Pokemon :img="pokemon1.imagen" :text="pokemon1.text" />
        <Pokemon :img="pokemon1.imagen" :text="pokemon1.text" />
      </div>
      <button @click="jugar">JUGAR</button>
    </div>
    <div v-else>
      <button @click="nuevoJuego">NUEVO JUEGO</button>
      <p v-if="intentos >= 5 && puntos < 10" class="mensajeRojo">
        Has utilizado tus 5 intentos. El juego ha terminado
      </p>
      <p v-if="puntos >= 10" class="mensajeAzul">
        Puntaje: {{ puntos }} Felicidades has ganado un premio de $10
      </p>
    </div>
  </div>
</template>

<script>
import Pokemon from "./components/Pokemon.vue";

export default {
  name: "App",
  components: {
    Pokemon,
  },
  data() {
    return {
      pokemon1: {
        imagen: "https://via.placeholder.com/250",
        text: "XXXXXXXXXX",
      },
      pokemon2: {
        imagen: "https://via.placeholder.com/250",
        text: "XXXXXXXXXX",
      },
      pokemon3: {
        imagen: "https://via.placeholder.com/250",
        text: "XXXXXXXXXX",
      },
      intentos: 0,
      puntos: 0,
      juego: true,
    };
  },

  methods: {
    async jugar() {
      this.intentos++;
      let conteoRepe = 0;

      const pokemons = [this.pokemon1, this.pokemon2, this.pokemon3];

      for (let i = 0; i < pokemons.length; i++) {
        const data = await fetch("https://pokeapi.co/api/v2/pokemon/1").then(
          (respuesta) => respuesta.json()
        );
        const { name, url } = data;
        pokemons[i].text = name;

        if (name === pokemons[1].text) {
          conteoRepe++;
        } else if (name === pokemons[2].text) {
          conteoRepe++;
        }

        if (this.intentos >= 5 || this.puntos > 10) {
          this.juego = false;
        }
      }

      this.actualizarPuntos(conteoRepe);
    },

    actualizarPuntos(conteoRepe) {
      if (conteoRepe === 3) {
        this.puntos += 5;
      } else if (conteoRepe === 2) {
        this.puntos += 2;
      } else if (conteoRepe === 1) {
        this.puntos += 1;
      }
    },

    nuevoJuego() {
      (this.pokemon1 = {
        imagen: "https://via.placeholder.com/250",
        text: "XXXXXXXXXX",
      }),
        (this.pokemon2 = {
          imagen: "https://via.placeholder.com/250",
          text: "XXXXXXXXXX",
        }),
        (this.pokemon3 = {
          imagen: "https://via.placeholder.com/250",
          text: "XXXXXXXXXX",
        }),
        (this.puntos = 0);
      this.intentos = 0;
      this.juego = true;
    },
  },
};
</script>

<style>
.app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.imagenes {
  display: flex;
  justify-content: center;
}

.enunciados {
  display: flex;
  justify-content: center;
}

p {
  margin: 50px;
}

.mensajeAzul {
  color: blue;
}

.mensajeRojo {
  color: red;
}
</style>




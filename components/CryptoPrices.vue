<template>
  <div class="app">
    <div class="primeraSeccion">
      <div class="titulo">Crypto Prices</div>
      <select class="selector" name="MonedaSeleccionada2" id="coinSelector" @change="onChange($event)">
        <option
          :value="item"
          v-for="(item, index) in listaMonedas"
          :key="`coin-${index}`"
          
        >
          {{ item }}
        </option>
      </select>
    </div>
    <div class="infoMoneda">
      <div>
        {{tituloDeSeccionInfo}}
      </div>
      <img :src="objMonedaSeleccionada.image.large" alt="" />
      <div class="precios">
        <div class="precioTitulo">Price:</div>
        <div class="preciovalor">
          {{
            this.objMonedaSeleccionada.market_data.current_price.usd.toLocaleString(
              "en-US",
              {
                style: "currency",
                currency: "USD",
              }
            )
          }}
        </div>
      </div>
      <div class="descripcion">
        <div class="desctitulo">Description</div>
        <div class="descvalor">
          {{ this.objMonedaSeleccionada.description.en }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
export default Vue.extend({
  data() {
    return {
      Lista: [],
      precios: [],
      listaMonedas: [],
      MonedaSeleccionada: "bitcoin",
      objMonedaSeleccionada: {
        image: { large: null },
        description: { en: null },
        market_data: { current_price: { usd: 0 } },
      },
      urlBase: "https://api.coingecko.com/api/v3/coins",
      monedaDefaultEndpoint: "/bitcoin",
    };
  },

  async mounted() {

    axios.get(`${this.urlBase}${this.monedaDefaultEndpoint}`).then((resp) => {
      this.objMonedaSeleccionada = resp.data;
    });
    axios.get(this.urlBase).then((resp) => {
      this.listaMonedas = resp.data.map((moneda) => moneda.id);
      console.log(this.listaMonedas);
    });
  },
   methods: {
    onChange(event) {
      axios
        .get(`${this.urlBase}/${event.target.value}`)
        .then((resp) => {
          this.objMonedaSeleccionada = resp.data;
          console.log(this.objMonedaSeleccionada);
        });
            console.log(event.target.value)
        }
  },
  computed: {
    tituloDeSeccionInfo() {
      return this.objMonedaSeleccionada.id
            ? this.objMonedaSeleccionada.id.toUpperCase()
            : "BITCOIN"
    }

  }


});
</script>

<style scoped>
.app {
  margin: 0px 200px;
}
.primeraSeccion {
  width: 100%;
  padding: auto;
  display: flex;
  flex-direction: row;
  margin-top: 40px;
  margin-bottom: 40px;
}
.titulo {
  margin: auto;
  width: auto;
  font-size: 50px;
  font-style: bolder;
  text-align: center;
  color: green;
}
.selector {
  margin: auto;
  text-align: center;
  background-color: rgb(222, 255, 155);
  font-size: 30px;
}
.infoMoneda {
  display: flex;
  flex-direction: column;
  width: 100%;
  text-align: center;
}

.infoMoneda img {
  width: 20%;
  margin: auto;
  margin: 40px auto;
}
.precios {
  display: flex;
  flex-direction: row;
  margin: auto;
  font-size: 40px;
}
.precioTitulo {
  font-style: bolder;
  margin-right: 20px;
  margin-bottom: 50px;
  font-weight: 900;
}
.descripcion {
  text-align: left;
  background-color: rgb(204, 241, 204);
  border-radius: 35px;
  padding: 20px;
}
.desctitulo {
  margin-bottom: 25px;
  font-size: 35px;
  font-weight: 400;
}
/* media queries:  (480px, 768px, 1024px, and 1280px) */

@media (max-width: 1024px) {
  .app {
    margin: 0px;
  }
  .descripcion {
    border-radius: 0px;
    text-align: center;
  }
}

@media (max-width: 1280px) {
  .primeraSeccion {
    flex-direction: column;
  }
  .titulo {
    margin-bottom: 30px;
    width: auto;
    font-size: 45px;
    font-style: bolder;
    text-align: center;
    color: green;
  }
}

@media (min-width: 1024px) {
  .app {
    margin: 0px 10.8vw;
  }
}
</style>

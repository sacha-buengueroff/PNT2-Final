<template>
  <div>
    <h1>Conversor a dólares</h1>
    <label for="pesos">Ingrese monto $ </label>
    <input id="pesos" class="form-control" v-model="pesos" />

    <br />
    <br />

    <label for="dolar">Valor del dolar en $ </label>
    <input id="dolar" class="form-control" v-model="dolar" />

    <label for="automatico"> - Actualización </label>
    <input
      type="checkbox"
      id="automatico"
      class="form-control"
      v-model="automatico"
      @click="setearTimer()"
    />
    <b v-if="automatico">{{ fechaActualizacion }}</b>

    <br />
    <br />

    <div>Valor convertido USD {{ valorConvertido }}</div>
  </div>
</template>

<script>
export default {
  name: "src-components-conversor",
  props: [],
  mounted() {

  },
  data() {
    return {
      pesos: 0,
      dolar: 0,
      automatico: false,
      url: "https://www.dolarsi.com/api/api.php?type=valoresprincipales",
      segundos: 2,
      timer: null,
      fechaActualizacion: null,
    };
  },
  methods: {
    async obtenerDolarActualizado() {
      try {
        let res = await this.axios(this.url);
        let dolares = res.data;
        let dolar = dolares.filter(
          (elem) => elem["casa"]["nombre"] == "Dolar Blue"
        );
        this.dolar = parseInt(dolar[0]["casa"]["venta"]);
        this.fechaActualizacion = new Date().toLocaleString();
      } catch (error) {
        console.log(error);
      }
    },
    setearTimer() {
      if (!this.automatico) {
        this.timer = setInterval(() => {
          this.obtenerDolarActualizado();
        }, this.segundos * 1000);
      }
      else {
        this.fechaActualizacion = ""
        this.timer = clearInterval(this.timer)
      }
    }
  },
  computed: {
    valorConvertido() {
      return this.pesos / this.dolar;
    },
  },
};
</script>

<style scoped lang="css">
</style>

<template>
  <v-container>
    <h1>Pošalji zahtjeve i prikaži ih u tablici</h1>
    <v-form>
      <div class="center">
        <v-text-field label="Upišite neko ime" v-model="userIme"></v-text-field>
      </div>

      <v-btn class="tipka" @click="getData()">Potvrdi</v-btn>
    </v-form>

    <v-data-table
      :headers="headers"
      :items="imeData"
      class="elevation-1"
      style="margin-top: 5rem"
    ></v-data-table>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  name: "Home",
  data() {
    return {
      headers: [
        {
          text: "Ime",
          align: "start",
          sortable: false,
          value: "ime",
        },
        {
          text: "Godina",
          value: "godinaKor",
        },
        {
          text: "Spol",
          value: "spolKor",
        },
        {
          text: "Nacionalnost",
          value: "nacionalnostKor",
        },
      ],

      imeData: [],

      godina: "",
      spol: "",
      nacionalnost: "",
      userIme: "",
    };
  },

  methods: {
    async getData() {
      const godData = await axios.get(
        "https://api.agify.io?name=" + this.userIme
      );
      this.godina = godData.data;
      const spolData = await axios.get(
        "https://api.genderize.io?name=" + this.userIme
      );
      this.spol = spolData.data;
      const nacData = await axios.get(
        "https://api.nationalize.io?name=" + this.userIme
      );
      this.nacionalnost = nacData.data;

      this.imeData.push({
        ime: this.userIme,
        godinaKor: this.godina.age,
        nacionalnostKor:
          this.nacionalnost.country[1].country_id +
          " (" +
          (this.nacionalnost.country[1].probability*100).toFixed(0) + "%)",
        spolKor: this.spol.gender +
          " (" +
          (this.spol.probability*100).toFixed(0) + "%)",
      });
    },
  },
  mounted() {},
};
</script>

<style scoped>
h1 {
  text-align: center;
}

.center {
  display: flex;
  margin: 0 auto;
  width: 40%;
  margin-top: 2rem;
}
.tipka {
  display: flex;
  justify-content: center;
  margin: 0 auto;
  margin-top: 1rem;
}
</style>
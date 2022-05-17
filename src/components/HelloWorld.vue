<template>
  <div class="hello box-border mx-10 font-sans">
    <h1 class="text-2xl font-bold text-center m-5">Corona-Dashboard</h1>
    <p class="text-center m-5">Anzeige Land,Datum</p>

    <div class="border-2 border-amber-800">
      <table class="table-fixed w-full text-center">
        <thead>
          <tr>
            <th class="bg-blue-500 py-5">
              Zusammenfassung der aktuellen Corona-Inzidenz-Zahlen
            </th>

            <th class="bg-blue-500 py-5">Liste der letzten Fallzahlen</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>{{ cases.confirmed }}</td>
            <td>{{ vaccines.people_vaccinated }}</td>
          </tr>
          <tr>
            <td>test</td>
            <td>test</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import "@/assets/style.css";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },

  data() {
    return {
      cases: {},
      vaccines: {},
    };
  },
  methods: {
    loadData() {
      console.log("data");
      fetch("https://covid-api.mmediagroup.fr/v1/cases")
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.cases = data.Germany.All;
          console.log(data.Germany.All);
        });
    },
    loadVaccines() {
      fetch("https://covid-api.mmediagroup.fr/v1/vaccines")
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.vaccines = data.Germany.All;
        });
    },
  },
  mounted() {
    this.loadData();
    this.loadVaccines();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

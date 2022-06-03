<template>
  <div class="hello box-border mx-10 font-sans">
    <h1 class="text-4xl font-bold text-center m-7">Corona-Dashboard</h1>

    <div class="flex flex-col items-center m-4">
      <h2 class="text-center font-bold mb-3">Dein Benutzer</h2>

      <p class="text-center" v-for="item of items" :key="item.id">
        {{ item.vorname + " " + item.nachname }}
      </p>
      <p class="text-center" v-for="item of items" :key="item.id">
        {{ item.straße + " " + item.PLZ + " " + item.stadt }}
      </p>
      <p class="text-center" v-for="item of items" :key="item.id">
        {{ item.email }}
      </p>
      <input
        class="shadow appearance-none border border-gray-300 rounded-lg focus:ring-blue-500/50 p-1.5 w-1/4 my-1.5"
        type="text"
        placeholder="Vorname"
        v-model="itemVorname"
      />
      <input
        class="shadow appearance-none border border-gray-300 rounded-lg focus:ring-blue-500/50 p-1.5 w-1/4 my-1.5"
        type="text"
        placeholder="Nachname"
        v-model="itemNachname"
      />
      <input
        class="shadow appearance-none border border-gray-300 rounded-lg focus:ring-blue-500/50 p-1.5 w-1/4 my-1.5"
        type="text"
        placeholder="Straße"
        v-model="itemStraße"
      />
      <input
        class="shadow appearance-none border border-gray-300 rounded-lg focus:ring-blue-500/50 p-1.5 w-1/4 my-1.5"
        type="text"
        placeholder="PLZ"
        v-model="itemPlz"
      />
      <input
        class="shadow appearance-none border border-gray-300 rounded-lg focus:ring-blue-500/50 p-1.5 w-1/4 my-1.5"
        type="text"
        placeholder="Stadt"
        v-model="itemStadt"
      />
      <input
        class="shadow appearance-none border border-gray-300 rounded-lg focus:ring-blue-500/50 p-1.5 w-1/4 my-1.5"
        type="text"
        placeholder="E-Mail-Adresse"
        v-model="itemEmail"
        @change="checkEmail()"
      />
      <div class="flex pt-3">
        <button
          class="bg-red-300 p-2 m-1 rounded shadow-lg"
          @click="
            resetInput();
            clearItems();
            deleteItem(id);
          "
        >
          Löschen
        </button>
        <button
          class="bg-green-300 p-2 m-1 rounded shadow-lg"
          @click="
            /* addItem(); */
            updateItem()
          "
        >
          Speichern
        </button>
      </div>
    </div>

    <div>
      <p class="text-red-500 text-center">{{ mailMessage }}</p>
    </div>

    <div class="flex flex-col">
      <p class="text-center mt-12 text-xl">
        {{ cases.country }} am {{ updated }}
      </p>

      <div class="border-2 m-12">
        <table class="table-fixed w-full text-center">
          <thead>
            <tr>
              <th class="bg-slate-400 py-5 px-3">
                Zusammenfassung der aktuellen Corona-Inzidenz-Zahlen
              </th>

              <th class="bg-slate-400 py-5 px-3">
                Vollständig geimpfte Personen
              </th>

              <th class="bg-slate-400 py-5 px-3">Zum teil geimpfte Personen</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td class="py-8 text-xl">{{ cases.confirmed }}</td>
              <td class="py-8 text-xl">{{ vaccines.people_vaccinated }}</td>
              <td class="py-8 text-xl">
                {{ vaccines.people_partially_vaccinated }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import "@/assets/style.css";
import axios from "axios";
/*
import Vue from "vue";
import VueFormulate from "@braid/vue-formulate";

Vue.use(VueFormulate); */

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },

  data() {
    return {
      cases: {},
      vaccines: {},
      updated: {},
      items: [],
      itemVorname: "",
      itemNachname: "",
      itemStraße: "",
      itemStadt: "",
      itemPlz: "",
      itemEmail: "",
      errors: [],
    };
  },

  async created() {
    try {
      const res = await axios.get(`http://localhost:3000/items`);
      this.items = res.data;
    } catch (error) {
      console.log(error);
    }
  },

  methods: {
    loadData() {
      fetch("https://covid-api.mmediagroup.fr/v1/cases")
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.cases = data.Germany.All;
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

    loadPartVaccines() {
      fetch("https://covid-api.mmediagroup.fr/v1/vaccines")
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.vaccines = data.Germany.All;
        });
    },

    loadCountry() {
      fetch("https://covid-api.mmediagroup.fr/v1/cases")
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.country = data.Germany.All.country;
        });
    },

    loadDate() {
      fetch("https://covid-api.mmediagroup.fr/v1/cases")
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.updated = data.Germany.Unknown.updated;
        });
    },

    async addItem() {
      const res = await axios.post("http://localhost:3000/items", {
        vorname: this.itemVorname,
        nachname: this.itemNachname,
        straße: this.itemStraße,
        PLZ: this.itemPlz,
        stadt: this.itemStadt,
        email: this.itemEmail,
      });
      this.items = [...this.items, res.data];
      this.itemVorname = "";
      this.itemNachname = "";
      this.itemStraße = "";
      this.itemPlz = "";
      this.itemStadt = "";
      this.itemEmail = "";
    },

    resetInput() {
      this.itemVorname = "";
      this.itemNachname = "";
      this.itemStraße = "";
      this.itemPlz = "";
      this.itemStadt = "";
      this.email = "";
    },

    clearItems() {
      this.items = "";
    },

    checkEmail() {
      var re =
        /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      if (re.test(this.itemEmail) === false) {
        this.mailMessage = "Die Email-Adresse ist nicht gültig.";
      } else {
        this.mailMessage = " ";
      }
    },

    async deleteItem() {
      await axios.delete(
        "http://localhost:3000/items"

        /*  this.items = this.items.filter((item) => item.id !== id); */
      );
    },

    async updateItem() {
      const res = await axios.patch("http://localhost:3000/items/0", {
        vorname: this.itemVorname,
        nachname: this.itemNachname,
        straße: this.itemStraße,
        PLZ: this.itemPlz,
        stadt: this.itemStadt,
        email: this.itemEmail,
      });
      this.items = [...this.items, res.data];
      this.itemVorname = "";
      this.itemNachname = "";
      this.itemStraße = "";
      this.itemPlz = "";
      this.itemStadt = "";
      this.itemEmail = "";
    },
  },

  mounted() {
    this.loadData();
    this.loadVaccines();
    this.loadDate();
    this.loadCountry();
    this.loadPartVaccines();
    this.resetInput();
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

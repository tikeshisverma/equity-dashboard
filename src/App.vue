<template>
  <div id="app">
    <div class="search-wrapper">
      <input type="text" class="sumbol" v-model="symbol" v-on:keyup.enter="getCompanyData" />
      <button @click="getCompanyData">Search</button>
    </div>
    <div class="details" v-if="companyData && !isLoading">
      <div class="card">
        <h3>Company information</h3>
        <div class="error" v-if="!Object.keys(companyData.info).length">Sorry no data found.</div>
        <ul>
          <li v-for="(value, key) in companyData.info" :key="key">
            <span class="bold">{{ key }}</span> :
            <span>{{ value }}</span>
          </li>
        </ul>
      </div>

      <div class="card">
        <h3>Corporate Actions</h3>
        <div class="error" v-if="!Object.keys(companyData.action).length">Sorry no data found.</div>
        <ul>
          <li v-for="(value, key) in companyData.action" :key="key">
            <span class="bold">{{ key }}</span> :
            <span>{{ value }}</span>
          </li>
        </ul>
      </div>

      <div class="card">
        <h3>Announcements</h3>
        <div class="error" v-if="!Object.keys(companyData.announce).length">Sorry no data found.</div>
        <ul>
          <li v-for="(value, key) in companyData.announce" :key="key">
            <span class="bold">{{ value }}</span>
          </li>
        </ul>
      </div>
    </div>
    <div class="loader" v-if="isLoading">
      <Loader />
    </div>
  </div>
</template>

<script>
import Loader from "./components/loader.vue";
export default {
  name: "app",
  components: {
    Loader
  },
  data() {
    return {
      companyData: null,
      symbol: "infy",
      isLoading: false
    };
  },
  mounted() {
    this.getCompanyData();
  },
  methods: {
    getCompanyData() {
      this.isLoading = true;
      fetch(`http://15.206.111.119/api/search?symbol=${this.symbol}`)
        .then(res => res.json())
        .then(res => {
          this.isLoading = false;
          this.companyData = res;
        })
        .catch(() => {
          this.isLoading = false;
          this.companyData = {};
          alert("Something went wrong! Please retry");
        });
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  align-items: center;
  flex-direction: column;
}
html,
body {
  height: 100%;
  background: aliceblue;
}
.search-wrapper {
  width: 60%;
  display: flex;
}
.search-wrapper input {
  border: 1px solid #3a73f3;
  border-right: 0;
  border-radius: 10px 0 0 10px;
  padding: 1em;
  width: 100%;
  text-transform: uppercase;
}
.search-wrapper button {
  border: 1px solid #3a73f3;
  border-left: 0;
  margin: 0 1em 0 0;
  font-size: 14px;
  padding: 10px 16px;
  cursor: pointer;
  background: #3a73f3;
  color: #fff;
  border-radius: 0 10px 10px 0;
}
.details {
  display: flex;
  width: 80%;
  margin-top: 40px;
}
.card {
  box-shadow: 0 13px 27px -5px rgba(50, 50, 93, 0.1),
    0 8px 16px -8px rgba(0, 0, 0, 0.1), 0 -6px 16px -6px rgba(0, 0, 0, 0.01);
  border-radius: 4px;
  width: 35%;
  padding: 1.5% 4%;
  margin: 1%;
  background: white;
}
.card:hover {
  box-shadow: 0 30px 60px -12px rgba(50, 50, 93, 0.1),
    0 18px 36px -18px rgba(0, 0, 0, 0.1), 0 -12px 36px -8px rgba(0, 0, 0, 0.01);
}
.bold {
  font-weight: bold;
}
.loader {
  margin: 4em;
}
ul {
  list-style: none;
  text-align: left;
  padding: 0;
}

@media only screen and (max-width: 720px) {
  .details {
    flex-direction: column;
    align-items: center;
    margin-top: 3em;
    width: 100%;
  }

  .search-wrapper {
    width: 100%;
  }
  .search-wrapper button {
    margin-right: 0;
  }
  .card {
    width: 85%;
    font-size: 12px;
  }
}
</style>

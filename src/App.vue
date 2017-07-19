<template>
  <div id="app">
    <h1>{{ msg }}</h1>
    <ul>
      <li v-for="ad of ads">
        {{ ad.id }}: {{ ad.height }}x{{ ad.columns }} - {{ ad.business }} by {{ ad.rep }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data: () => ({
    ads: [],
    errors: [],
    msg: "Ad list"
  }),

  created() {
    axios.get('http://localhost:9393/api/ads')
      .then(response => {
        this.ads = response.data
        console.log(response.data)
      })
      .catch(e => {
        this.errors.push(e)
      })
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>

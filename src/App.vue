<template>
  <div id="app">
    <div class="container">
      <h1 class="title">{{ msg }}</h1>

      <table class="table is-narrow">
        <thead>
          <tr>
            <th>ID</th>
            <th>Business</th>
            <th>Rep</th>
            <th>Height</th>
            <th>Columns</th>
            <th>Position</th>
            <th>Info</th>
            <th>Repeat</th>
            <th>Payment</th>
            <th>Price</th>
            <th></th>
            <th></th>
          </tr>
        </thead>
        <tfoot>
          <tr>
            <th>ID</th>
            <th>Business</th>
            <th>Rep</th>
            <th>Height</th>
            <th>Columns</th>
            <th>Position</th>
            <th>Info</th>
            <th>Repeat</th>
            <th></th>
            <th></th>
          </tr>
        </tfoot>
        <tbody>
          <tr v-for="ad of ads" v-if="ad.placed == true">
            <td>{{ ad.id }}</td>
            <td>{{ ad.customer }}</td>
            <td>{{ ad.rep }}</td>
            <td>{{ ad.height }}</td>
            <td>{{ ad.columns }}</td>
            <td>{{ ad.position }}</td>
            <td>?</td>
            <td>?</td>
            <td>
              <a class="button is-small is-info">
                Approve
              </a>
            </td>
            <td>
              <a class="button is-small is-primary">
                Place
              </a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
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
      })
      .catch(e => {
        this.errors.push(e)
      })
  }
}
</script>

<style lang="sass">
.table
  font-size: .9em
</style>

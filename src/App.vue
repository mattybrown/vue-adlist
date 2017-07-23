<template>
  <div id="app">
    <div class="container">
      <article class="message is-danger" v-for="error of errors" v-if="errors">
        <div class="message-header">
          <p>Error: {{ error.message }}</p>
          <button class="delete"></button>
        </div>
        <div class="message-body">
          <p>Response: {{ error.response }}</p>
          <p>Request: {{ error.request }}</p>
          <p>{{ error }}</p>
        </div>
      </article>
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
          <tr v-for="ad of ads">
            <template v-if="ad.placed == false">
              <td>{{ ad.id }}</td>
              <td>{{ ad.customer }}</td>
              <td>{{ ad.rep }}</td>
              <td>{{ ad.height }}</td>
              <td>{{ ad.columns }}</td>
              <td>{{ ad.position }}</td>
              <td>?</td>
              <td>?</td>
              <td>
                <a class="button is-small is-info" v-on:click="approveAd(ad.id)">
                  Approve
                </a>
              </td>
              <td>
                <a class="button is-small is-primary">
                  Place
                </a>
              </td>
            </template>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import {HTTP} from './http-common';

export default {
  data: function () {
    return {
      ads: [],
      errors: [],
      msg: "Ad list"
    }
  },
  methods: {
    loadData: function () {
      HTTP.get('ads')
        .then(response => {
          this.ads = response.data
        })
        .catch(e => {
          this.errors.push(e)
        })
    },
    approveAd: function (id) {
      HTTP.post('ad/approve/', {
        ad_id: id
      }).then(response => {
        console.log(response)
      }).catch(e => {
        this.errors.push(e)
      })
    },
  },
  mounted: function () {
    this.loadData();
  }
}
</script>

<style lang="sass">
.table
  font-size: .9em
</style>

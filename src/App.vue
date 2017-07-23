<template>
  <div id="app">
    <div class="container">
      <transition mode="out-in" name="error-message">
        <article class="message is-danger" v-for="error of errors" v-if="message">
          <div class="message-header">
            <p>Error: {{ error.message }}</p>
            <button class="delete" v-on:click="message = !message"></button>
          </div>
          <div class="message-body">
            <p>Response: {{ error.response }}</p>
            <p>Request: {{ error.request }}</p>
            <p>{{ error }}</p>
          </div>
        </article>
      </transition>
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
          <template v-for="ad of ads">
            <tr v-bind:class="{ 'table-is-approved': ad.approved }">
              <td>{{ ad.id }}</td>
              <td>{{ ad.customer }}</td>
              <td>{{ ad.rep }}</td>
              <td>{{ ad.height }}</td>
              <td>{{ ad.columns }}</td>
              <td>{{ ad.position }}</td>
              <td>?</td>
              <td>?</td>
              <td>
                <a class="button is-small is-info" v-bind:class="{ 'is-inverted': ad.approved }" v-on:click="approveAd(ad.id)">
                  <template v-if="ad.approved">
                    Approved
                  </template>
                  <template v-else>
                    Approve
                  </template>
                </a>
              </td>
              <td>
                <a class="button is-small is-primary">
                  Place
                </a>
              </td>
            </tr>
          </template>
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
      msg: "Ad list",
      message: true
    }
  },
  methods: {
    loadData: function () {
      HTTP.get('ads')
        .then(response => {
          this.ads = response.data
        })
        .catch(e => {
          this.errors.push(e);
          this.message = true
        })
    },
    approveAd: function (id) {
      HTTP.post('ad/approve', {
        ad_id: id
      }).then(response => {
        console.log(response);
        this.loadData();
      }).catch(e => {
        this.errors.push(e);
        this.message = true
      })
    }
  },
  mounted: function () {
    this.loadData();
  }
}
</script>

<style lang="sass">

$white-ter:    hsl(0, 0%, 96%) !default
$white-bis:    hsl(0, 0%, 98%) !default
$white:        hsl(0, 0%, 100%) !default

$green:        hsl(141, 71%,  48%) !default
$turquoise:    hsl(171, 100%, 41%) !default
$blue:         hsl(217, 71%,  53%) !default

.table
  font-size: .9em

.error-message-enter-active, .error-message-leave-active
  transition: all .5s ease

.error-message-enter, .error-message-leave-to
  opacity: 0

tr
  &.table-is-approved
    background-color: $blue
    color: $white-ter
    &:hover
      background-color: $blue


</style>

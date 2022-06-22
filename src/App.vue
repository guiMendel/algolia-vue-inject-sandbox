<template>
  <!-- Instant search wrapper -->
  <ais-instant-search :search-client="searchClient" index-name="demo_ecommerce">
    <!-- Searchbox -->
    <ais-search-box />

    <!-- Results -->
    <ais-hits>
      <template v-slot:item="{ item }">
        <h2>{{ item.name }}</h2>
      </template>
    </ais-hits>
  </ais-instant-search>
</template>

<script>
import algoliasearch from 'algoliasearch/lite'
import 'instantsearch.css/themes/satellite-min.css'
import config from './config'

export default {
  setup() {
    // Build the client
    const searchClient = algoliasearch(config.appId, config.apiKey)

    return { searchClient }
  },
}
</script>

<style>
body {
  font-family: sans-serif;
  padding: 1em;
}
</style>
<template>
  <!-- Instant search wrapper -->
  <ais-instant-search :search-client="searchClient" :index-name="indexName">
    <!-- Searchbox -->
    <ais-search-box />

    <!-- Results -->
    <Results />
  </ais-instant-search>
</template>

<script>
import algoliasearch from 'algoliasearch/lite'
import config from './config'
import Results from './components/Results'

export default {
  setup() {
    // Build the client
    const searchClient = algoliasearch(config.appId, config.apiKey)

    return { searchClient, indexName: config.index }
  },

  components: { Results },
}
</script>

<style lang="scss">
/* Get fonts */
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;1,300;1,400;1,500;1,600;1,700&display=swap');

/* CSS Reset */
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

/* General styling */
body {
  --main: rgb(46, 112, 255);

  font-family: 'Open Sans', sans-serif;
  padding: 3vw;
  background-color: rgb(250, 250, 250);

  color: rgb(112, 112, 112);
}

/* Highlighting */
.ais-Highlight-highlighted {
  background-color: rgb(46, 112, 255, 0.5);
}

/* Searchbox */
.ais-SearchBox-form {
  display: flex;
  align-items: center;
  justify-content: center;

  margin-bottom: 3rem;

  .ais-SearchBox-input {
    border: none;

    padding: 1rem 1.5rem;

    font-size: 1.1rem;
    z-index: 10;

    width: 100%;
    max-width: 30rem;

    box-shadow: inset 0 0 4px 1px rgb(148, 148, 148, 0.3);

    outline-color: var(--main);
  }

  .ais-SearchBox-submit,
  .ais-SearchBox-reset {
    display: none;
  }
}
</style>
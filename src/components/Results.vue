<template>
  <ais-infinite-hits class="results-container">
    <template v-slot="{ items, refineNext }">
      <!-- Results -->
      <div class="items">
        <ResultItem v-for="item in items" :key="item.objectID" :item="item" />
      </div>

      <!-- Show more -->
      <button @click="refineNext">Show More</button>
    </template>
  </ais-infinite-hits>
</template>

<script>
import ResultItem from './ResultItem'

export default {
  components: { ResultItem },
}
</script>

<style lang="scss" scoped>
.results-container {
  display: flex;
  flex-direction: column;
  align-items: center;

  gap: 2rem;

  .items {
    display: flex;
    flex-wrap: wrap;

    gap: 4rem;

    align-items: stretch;
    justify-content: center;
  }

  > button {
    border: none;
    background-color: var(--main);
    box-shadow: 0 0 4px 1px rgb(148, 148, 148, 0.3);

    padding: 1rem 1.5rem;

    color: white;
    font-size: inherit;

    cursor: pointer;
  }
}
</style>
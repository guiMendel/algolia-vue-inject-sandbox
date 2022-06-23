<template>
  <!-- Widget to get the query results -->
  <ais-infinite-hits class="results-container">
    <template v-slot="{ items: queryResults, refineNext }">
      <!-- Widget to get custom data from rules -->
      <ais-query-rule-custom-data>
        <template v-slot="{ items: customContent }">
          <!-- Results -->
          <div class="items">
            <!-- Instead of looping through the bare items, we first inject the content in them with this method -->
            <!-- We use a template here so that this container doesn't get rendered, only it's child -->
            <template
              v-for="item in inject(queryResults, customContent)"
              :key="item.objectID"
            >
              <!-- We need to check whether this item is an injected content or a regulat hit-->
              <!-- When we inject content with the "inject" method above, we add the "isInjectedContent" label, so we can just check that -->
              <InjectedContent v-if="item.isInjectedContent" :item="item" />

              <!-- In case it's just a regular hit, use the hit component -->
              <QueryResult v-else :item="item" />
            </template>
          </div>

          <!-- Show more -->
          <button @click="refineNext">Show More</button>
        </template>
      </ais-query-rule-custom-data>
    </template>
  </ais-infinite-hits>
</template>

<script>
import QueryResult from './QueryResult'
import InjectedContent from './InjectedContent'

export default {
  components: { QueryResult, InjectedContent },

  methods: {
    // Injects the custom content in the queryResults
    inject(queryResults, customContent) {
      // Copy the queryResults so that we don't accidentally alter them
      // If you're not familair with JS's spread syntax (the following ellipsis), read this: https://www.javascripttutorial.net/es-next/javascript-object-spread/
      const newItems = [...queryResults]

      // Loops through each of the content, getting the "injectContent" property
      for (const { injectContent } of customContent) {
        // We only care for content that actually has the "injectContent" property
        if (injectContent == null) continue

        // Inject the content at the requested position (defaults to first position)
        newItems.splice(injectContent.position ?? 0, 0, {
          // To the data returned from the API, we need to add a label to indicate in the vue template that this is injected content, and not a regular hit
          ...injectContent,
          isInjectedContent: true,
          objectID: `injected-content-${injectContent.title}`,
        })
      }

      return newItems
    },
  },
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
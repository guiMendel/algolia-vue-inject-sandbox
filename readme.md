# Vue Injection Guide

Welcome to this injection guide!

Here you'll learn how to inject content to your **Vue Instantearch** apps in a simple and effective way.

This readme explains in fine detail the whole process, but in case you want to see it in action, the sandbox should fit your needs.

## 🚗 Where to Start

There's 2 things that need to happen for content injection to work:

1. You need to **set up a rule** that returns **custom data** from the dashboard, which must contain whatever data it is you are trying to inject.
2. You must **catch** this custom data in the frontend (here) and **display** it.
   > In our case, we are using the `Vue Instantsearch` component `ais-query-rule-custom-data` to catch it for us.

## 📏 Setting Up Custom Data Rules

In order to achieve step number one (setting up a _custom data_ rule), you need to access your app's dashboard, and add a **manual rule**.

You can set up whichever trigger you find best.

As a consequence, you must select **Return Custom Data**, which will allow you to input some `JSON` to be returned when this rule triggers.

You can return anything you'd like: text, image links, booleans...

Here's a good example: this is the custom data being returned in this sandbox on an _empty query trigger_:

```json
{
  "injectContent": {
    "title": "Injecting Made Easy",
    "image": "https://source.unsplash.com/random/300x300",
    "position": 4
  }
}
```

## ⚾️ Catching & Displaying the Custom Data

On the frontend, catching custom data is made very easy by the Instantsearch `ais-query-rule-custom-data` component with the default Vue slot.

> In case you're not familiar with Vue slots, it is highly recommended you [read this article](https://vuejs.org/guide/components/slots.html#scoped-slots). They are extensively used in this sandbox.

Once you have access to the custom data provided by the dashboard, you have to **insert it** in the array of query results.

### Content Injection

Let's say our query results are in a variable called `queryResults`, and we want to inject our content at an index stored in a variable called `injectPosition`.

In order to do this, you might simply call splice [(documentation)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice):

```js
queryResults.splice(injectPosition, 0, injectedContent)
```

The only problem is, when you try to display the results, there will be no way to **differentiate** between the content that was injected and the regular hits. It will have been _camouflaged_ among them.

Not only that, but since `v-for` requires a `:key` property, we need to pass in some kind of ID too!

In order to address these issues, one could simply **add to the injected content** an `objectID` property; as well as some other property that can be checked for later, in order to **identify** this record as injected content — let us call this property `isInjectedContent`.

Now, our content injection call becomes:

```js
queryResults.splice(injectPosition, 0, {
  ...injectedContent,
  isInjectedContent: true,
  objectID: `injected-content-${injectContent.title}`,
})
```

> Note that we have used the spread operator in order to create a new object with the same properties as `injectedContent`, as well as the new property. In case you're not familiar with this syntax, [take a look at this article](https://www.javascripttutorial.net/es-next/javascript-object-spread/).

But when should we actually perform this injection?

### Content Display

Let's say your Vue template for displaying the query results looks something like this:

```jsx
<template v-for="hit in queryResults" :key="hit.objectID">
	<QueryResult :hit="hit" />
</template>
```

> We are using the `template` element so that no container is rendered, only it's contents. This will be useful later!

We will devise a **method** that will take in these results and the content to be injected and return the final, injected results:

```js
methods: {
    inject(queryResults, injectContent) {
	  // Copy the results
      const newItems = [...queryResults]

      newItems.splice(injectContent.position ?? 0, 0, {
        ...injectContent,
        isInjectedContent: true,
        objectID: `injected-content-${injectContent.title}`
      })

      return newItems
    },
},
```

There's a couple of things to be commented on this method.

**Firstly**, you will notice we **copied** the contents of queryResults before modifying them. This is done so that the result of this method is **never fed back to it** — that is, it doesn't accidentally inject to an array that was already injected. It always starts off from the bare, uninjected query results.

**Secondly**, we have assumed that the `injectContent` data may provide a `position` property, which would **specify the index** where to inject it to. But just in case it doesn't, we have passed it through the coalescing operator, defaulting to 0: `??` [(documentation)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator).

**Thirdly**, we have assumed that `injectContent` is already the content we want to inject to the query results. This **is not** the same object that `query-rule-custom-data` provides. What it provides is in fact an array of all the custom data objects that were triggered for the current search parameters. Please [refer to it's documentation](https://www.algolia.com/doc/api-reference/widgets/query-rule-custom-data/vue/#customize-the-ui) if this is not clear, and also the sandbox code.

Now that we are properly armed, let us finally apply the injection:

```jsx
<div v-for="hit in inject(queryResults, injectContent)" :key="hit.objectID">
	<InjectedContent v-if="hit.isInjectedContent" :hit="hit" />
	<QueryResult v-else :hit="hit" />
</div>
```

> The `InjectedContent` component is just responsible for displaying the inject content's properties.

We've introduced two changes:

1. Instead of looping through the bare `queryResults` array, we now pass it through the `inject` method so that we loop through the **results & injected** content array.
2. Inside of the `template` element, we now perform a `v-if` to check whether we are **displaying injected content or a regular hit**, so that we can call the proper component.

And hey, would you look at that: we're done!

Thank you so much for taking your time to read through this article 🤓

## Git Commit Emoji Reference

| Emoji 😊 |        Meaning        |
| :------: | :-------------------: |
|    ✨    |      New Feature      |
|    🔨    | Existing Feature Task |
|    🧹    |      Refactoring      |
|    🐛    |        Bug Fix        |
|    ✅    |     Test Related      |
|    📃    |     Documentation     |
|    🎨    |      Aesthetics       |
|    🃏    |         Meta          |

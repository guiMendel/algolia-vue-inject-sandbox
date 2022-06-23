<template>
  <!-- Item container -->
  <div class="item">
    <!-- Picture -->
    <img :src="item.image" alt="" />

    <!-- Brand -->
    <ais-highlight class="brand" attribute="brand" :hit="item" />

    <!-- Name -->
    <ais-highlight class="title" attribute="name" :hit="item" />
  </div>
</template>

<script>
export default {
  props: { item: { type: Object, required: true } },
}
</script>

<style lang="scss" scoped>
.item {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  gap: 0.3rem;

  align-items: center;

  width: 15rem;
  text-align: center;

  background-color: white;

  padding: 1rem;

  box-shadow: 0 0 4px 1px rgb(148, 148, 148, 0.2);

  img {
    margin-bottom: 0.3rem;
  }

  .brand {
    text-transform: uppercase;
    font-weight: 300;
    font-size: 0.8rem;
  }

  .title {
    font-size: 1rem;
    font-weight: 400;
  }
}
</style>
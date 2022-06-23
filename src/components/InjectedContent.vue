<template>
  <!-- Item container -->
  <div class="item">
    <!-- Title -->
    <h1 class="title">{{ item.title }}</h1>

    <!-- Picture -->
    <img :src="item.image" alt="" />
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

  gap: 1rem;

  align-items: center;

  width: 15rem;
  text-align: center;

  background-color: white;

  padding-top: 1rem;

  box-shadow: 0 0 4px 1px rgb(148, 148, 148, 0.2);

  .title {
    font-size: 1.2rem;
    font-weight: 700;
    margin-inline: 1rem;
  }

  img {
    width: 100%;
  }
}
</style>
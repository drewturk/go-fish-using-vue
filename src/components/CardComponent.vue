<template>
  <div class="card container">
    <h2>{{ title }}</h2>
    <div class="body">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardComponent",
  props: {
    title: String,
  },
};
</script>

<style lang="scss">
.card {
  margin: 16px;
  max-width: 400px;
  min-height: 150px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  border-radius: 5px;
  &:hover {
    box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
  }

  .body {
    display: flex;
    flex-direction: column;
    align-items: start;
  }

  ul {
    list-style: none;
    padding-left: 0;
    text-align: left;
    li {
      margin-bottom: 12px;
    }
  }
}
</style>

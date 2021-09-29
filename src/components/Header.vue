<template>
  <h1>{{ title }}</h1>
</template>

<script>
export default {
  name: "Header",
  props: {
    title: String,
  },
};
</script>

<style scoped>
h1 {
  margin: 0;
  padding: 2px;
  text-align: center;
}
</style>

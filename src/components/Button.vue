<template>
  <div class="button">
    <button @click="CheckScore" class="btn">Test Me!</button>
  </div>
</template>

<script>
export default {
  name: "Button",
  methods: {
    CheckScore() {
      this.$emit("check-score");
    },
  },
};
</script>

<style scoped>
.button {
  display: flex;
  justify-content: center;
}

.btn {
  margin: 5px;
  -webkit-border-radius: 28;
  -moz-border-radius: 28;
  border-radius: 28px;
  font-family: Arial;
  color: #ffffff;
  font-size: 17px;
  background: #50626e;
  padding: 10px 20px 10px 20px;
  text-decoration: none;
}

.btn:hover {
  background: #586e7c;
  text-decoration: none;
}
</style>

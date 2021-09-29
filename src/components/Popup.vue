<template>
  <div class="popup">
    <div class="popup-inner">
      <slot />
    </div>
  </div>
</template>

<script>
export default {
  name: "Popup",
};
</script>

<style scoped>
.popup {
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 99;
  background: rgba(0, 0, 0, 0.2);
}

.popup-inner {
  border: 0.5px solid black;
  border-radius: 24px;
  background: white;
  padding: 34px;
}
</style>

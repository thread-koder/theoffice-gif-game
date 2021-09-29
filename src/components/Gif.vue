<template>
  <img :src="gif.image" :alt="gif.type" />
</template>

<script>
export default {
  name: "Gif",
  props: {
    gif: Object,
  },
  methods: {
    DragStart(event) {
      const target = event.target;

      event.dataTransfer.setData("gif_id", target.id);

      setTimeout(() => {
        target.style.display = "none";
      }, 0);
    },
  },
};
</script>

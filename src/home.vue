<template>
  <div class="home-wrap">
    <Icons :query="query"></Icons>
  </div>
</template>

<script>
import Icons from "./icons.vue";
export default {
  components: {
    Icons,
  },
  data() {
    return {
      query: "",
    };
  },
};
</script>

<style scoped lang="scss">
.home-wrap {
  max-width: 1320px;
  margin: 0 auto;
}
</style>

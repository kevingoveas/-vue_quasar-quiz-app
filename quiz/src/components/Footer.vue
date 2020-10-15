<template>
  <div align="right" class="right">
    <label class="progressColor">Progress : {{ correct }}/{{ total }}</label>
  </div>
</template>
<script>
export default {
  name: "Footer",
  props: ["correct", "total"]
};
</script>

<style lang="sass" scoped>
.right
  margin:  0 10px 0 0
  margin-top: 10px;
.progressColor
  color:  blue
</style>

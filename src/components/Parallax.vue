<template>
 <div class="parallax"></div>
</template>

<script>
export default {
  name: "Parallax",
  props: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
.parallax {
  background-image: url("../assets/img/bg1.jpg");
  background-attachment: fixed;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  height: 200px;
}
</style>

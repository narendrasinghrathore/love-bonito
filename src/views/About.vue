<template>
  <div class="about-container">
    <Characters />
  </div>
</template>

<script>
import Characters from "@/components/Characters.vue";
export default {
  name: "About",
  components: {
    Characters
  }
};
</script>

<style lang="scss" scoped>
.about-container {
  background-color: #dedede;
  height: 100vh;
  box-sizing: border-box;
  align-items: center;
  justify-content: center;
  display: flex;
}
</style>

<template>
  <div class="unknown-tab">
    Something written here
    <img src="../../public/close.png" class="crossmark" @click="closeTab" />
  </div>
</template>

<script setup>
function closeTab(event) {
  event.target.parentNode.style.display = "none";
}
</script>

<style lang="scss">
@import "../assets/variables.scss";
.unknown-tab {
  border-radius: 12px;
  border: 1px solid $border-color;
  background: $bg-color;
  grid-column: 1 / -1;
  position: relative;
  padding: 18px;
  text-align: center;
}
</style>

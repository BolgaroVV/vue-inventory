<template>
    <div class="inventory-dsc">
      <img src="../../public/inventory.png" />
      <h3>Inventory</h3>
      <p>Here you can sort your mined resources or discard unwanted ones</p>
    </div>
</template>

<style lang='scss'>
@import '../assets/variables.scss';
.inventory-dsc {
  border-radius: 12px;
  border: 1px solid $border-color;
  background: $bg-color;
  padding: 18px;
  h3 {
    text-align: center;
    font-weight: 500;
    margin: 10px;
  }
}
</style>
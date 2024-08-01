<template>
  <div class="container">
    <InventoryDsc/>
    <div class="cages">
      <div
        class="cage"
        v-for="(cage, index) in cages"
        :key="index"
        @drop="onDrop($event, index)"
        @dragover.prevent
        @dragenter.prevent
      >
        <div
          class="item"
          v-for="item in items.filter((x) => x.cageId === index)"
          :key="item.id"
          @dragstart="onDragStart($event, item)"
          draggable="true"
          @click="openDsc"
          :id="item.id"
        >
          <div v-if="item.quantity > 0">
            <img :src="item.imgUrl" class="item-img" />
            <span class="item-quantity">{{ item.quantity }}</span>
          </div>
        </div>
      </div>
    </div>
    <div class="item-dsc">
      <img :src="dscItem.imgUrl" alt="" class="item-dsc-img" />
      <div class="item-dsc-content">
        <h2>{{ dscItem.name }}</h2>
        <p>{{ dscItem.dsc }}</p>
      </div>
      <button class="delete-item red-btn" @click="toggleDeleteMenu">
        Удалить предмет
      </button>
      <div class="delete-menu">
        <input
          type="number"
          placeholder="Введите количество"
          v-model="deleteQuantity"
          min="0"
        />
        <div class="delete-menu-buttons">
          <button class="white-btn" @click="toggleDeleteMenu">Отмена</button>
          <button class="red-btn" @click="deleteItem">Подтвердить</button>
        </div>
      </div>
      <img
        src="../public/close.png"
        alt=""
        class="crossmark"
        @click="closeDsc"
      />
    </div>
    <UnknownTab/>
  </div>
</template>

<script setup>
// Imports
import { ref, onMounted } from "vue";
import axios from "axios";
import InventoryDsc from "./components/InventoryDsc.vue";
import UnknownTab from "./components/UnknownTab.vue";

// Variables
const dscItem = ref([]);
const deleteQuantity = ref(0);
const cages = ref(25);
const items = ref([]);

// Functions
function onDragStart(e, item) {
  e.dataTransfer.dropEffect = "move";
  e.dataTransfer.effectAllowed = "move";
  e.dataTransfer.setData("itemId", item.id.toString());
}

function onDrop(e, categoryId) {
  const itemId = parseInt(e.dataTransfer.getData("itemId"));
  items.value = items.value.map((x) => {
    if (x.id == itemId) x.cageId = categoryId;
    return x;
  });
  axios.patch("https://d1db762bf614aa72.mokky.dev/items", items.value);
}

function openDsc(event) {
  document.querySelector(".item-dsc").classList.add('item-dsc-active');
  dscItem.value = items.value.find((el) => el.id == event.currentTarget.id);
}

function deleteItem() {
  dscItem.value.quantity -= deleteQuantity.value;
  axios.patch("https://d1db762bf614aa72.mokky.dev/items", items.value);
}

function closeDsc() {
  document.querySelector(".item-dsc").classList.remove('item-dsc-active');
}

function toggleDeleteMenu() {
  document.querySelector(".delete-menu").classList.toggle("delete-menu-active");
}

onMounted(async () => {
  const response = await axios.get("https://d1db762bf614aa72.mokky.dev/items");
  items.value = response.data;
});
</script>

<style lang="scss">
@import './assets/variables.scss';
.container {
  position: absolute;
  display: grid;
  gap: 24px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  grid-template-columns: 236px 1fr;
  overflow: hidden;
}

.cage {
  width: 100px;
  aspect-ratio: 1;
  background: $bg-color;
  border: 1px solid $border-color;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.cages {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  width: 510px;
}

.item-quantity {
  position: absolute;
  bottom: 0;
  right: 0;
  border-top-left-radius: 6px;
  border: 1px solid $border-color;
  padding: 2px 4px;
  font-size: 10px;
  font-weight: 500;
  color: #7d7d7d;
}

.crossmark {
  position: absolute;
  top: 8px;
  right: 8px;
}
.item-dsc {
  position: absolute;
  width: 250px;
  height: 510px;
  background: $bg-color;
  border: 1px solid $border-color;
  top: 0;
  right: -250px;
  padding: 55px 15px 18px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;
  transition: all 0.2s ease-in;
  h2 {
    text-align: center;
    margin-bottom: 16px;
  }
  &-active {
    right: 0;
  }
}

.item-dsc-content {
  border-top: 1px solid $border-color;
  border-bottom: 1px solid $border-color;
  padding: 16px 0 24px;
  margin-top: 20px;
}

.item-dsc-img {
  width: 130px;
  aspect-ratio: 1;
}

.delete-item {
  padding: 11px 50px !important;
  position: absolute;
  bottom: 18px;
}

.delete-menu {
  z-index: 100;
  width: 100%;
  box-sizing: border-box;
  border: 1px solid $border-color;
  padding: 20px;
  background: $bg-color;
  position: absolute;
  bottom: 0;
  display: none;
  &-active {
    display: block;
  }
  input {
    border-radius: 4px;
    border: 1px solid $border-color;
    background: $bg-color;
    padding: 11px;
    outline: none;
    width: 100%;
    box-sizing: border-box;
  }
}

.white-btn,
.red-btn {
  font-family: -apple-system, BlinkMacSystemFont, sans-serif;
  padding: 8px 14px;
  text-align: center;
  border-radius: 8px;
  font-size: 14px;
  border: none;
}

.white-btn {
  background: white;
  color: black;
}

.red-btn {
  background: #fa7272;
}

.delete-menu-buttons {
  display: flex;
  gap: 10px;
  margin-top: 20px;
  width: 100%;
  box-sizing: border-box;
}
</style>

<script setup lang="ts">
import { leftBlock, rightBlock } from './data'
import { ref } from 'vue'

type Item = { id: number, name: string }
const leftSelected = ref<Item[]>([])
const rightSelected = ref<Item | null>(null)

const selectItem = (item: Item, column: string) => {
  if (column === 'left') {
    const index = leftSelected.value.findIndex(el => el.id === item.id)
    if (index > -1) {
      leftSelected.value.splice(index, 1)
    } else if (leftSelected.value.length < 6) {
      leftSelected.value.push(item)
    }
  } else if (column === 'right') {
    rightSelected.value = item
  }
}

</script>

<template>
  <div class="container">
    <div class="top-panels">
      <div class="top-panel top-panel_left border-div">
        <div class="top-panel__list">
          <div class="top-panel__item border-div" v-if="leftSelected.length > 0" v-for="item in leftSelected"
            :key="item.id" @click="selectItem(item, 'left')">
            {{ item.name }}
          </div>
        </div>
        <span class="top-panel__info">{{ `selected: ${leftSelected.length}/6` }}</span>
      </div>
      <div class="top-panel top-panel_right border-div">
        <div class="top-panel__item border-div" v-if="rightSelected" @click="selectItem(rightSelected, 'right')">
          {{ rightSelected.name }}
        </div>
      </div>
    </div>
    <div class="content-blocks">
      <div class="content-block border-div">
        <div class="content-block__list">
          <div
            :class="{ 'content-block__item border-div': true, 'content-block__item_disabled': leftSelected.length === 6, 'content-block__item_active': item.id === leftSelected.find(el => el.id === item.id)?.id }"
            v-for="item in leftBlock" :key="item.id" @click="selectItem(item, 'left')">
            {{ item.name }}
          </div>
        </div>
      </div>
      <div class="content-block border-div">
        <div class="content-block__list">
          <div
            :class="{ 'content-block__item border-div': true, 'content-block__item_active': item.id === rightSelected?.id }"
            v-for="item in rightBlock" :key="item.id" @click="selectItem(item, 'right')">
            {{ item.name }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  padding: 20px 40px;
}

.border-div {
  border: 1px solid #000
}

.top-panels {
  display: grid;
  grid-template-columns: 1fr 1fr;
  column-gap: 40px;
}

.top-panel {
  padding: 20px;
  min-width: 200px;
  min-height: 80px;
  width: fit-content;
}

.top-panel_left {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.top-panel_right {
  justify-self: flex-end;
}

.top-panel__info {
  margin-top: 10px;
}

.top-panel__list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px
}

.top-panel__item {
  position: relative;
  padding: 10px;
  cursor: pointer;
}

.top-panel__item:hover::after {
  content: 'X';
  position: absolute;
  top: 2px;
  right: 2px;
  font-size: 12px;
  font-weight: 500;
  color: #ff0000;
}

.content-blocks {
  margin-top: 40px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  column-gap: 40px;
}

.content-block {
  padding: 20px;
}

.content-block__list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px
}

.content-block__item {
  padding: 10px;
  cursor: pointer;
  transition: 0.2s background-color;
}

.content-block__item:hover {
  background-color: #acacac7c;
}

.content-block__item_active {
  background-color: #acacac7c;
  pointer-events: none;
}

.content-block__item_disabled {
  pointer-events: none;
}
</style>

<script setup>
import { ref } from "@vue/reactivity";

const props = defineProps({
  item: Object,
  search: String,
});

const isDraggable = ref(false);
const isOpen = ref(false);

const listItemClickHandler = () => {
  isOpen.value = !isOpen.value;
  if (isOpen.value) isDraggable.value = false;
};

const listItemMoveHandler = () => {
  isDraggable.value = !isDraggable.value;
  if (isDraggable.value) isOpen.value = false;
};
</script>
<template>
  <li :draggable="isDraggable">
    <div :class="{
      'category-list-item': item.list,
      'list-item': !item.list,
      empty: !item.list || !item.list.length,
      open: search || isOpen,
    }" @click="listItemClickHandler">
      <span :class="{
        'category-list-title': item.list,
        'list-title': !item.list,
      }">{{ item.title }}</span>
      <span v-if="item.types.length" class="list-color">
        <i v-for="t in item.types" class="circle" :class="t" :key="t"></i>
      </span>
      <span v-if="item.description" class="list-description">{{
        item.description
      }}</span>
      <span class="list-action">
        <i class="action-edit"></i>
        <i class="action-delete"></i>
        <i class="action-move" @click.stop="listItemMoveHandler"></i>
      </span>
    </div>
    <slot></slot>
  </li>
</template>

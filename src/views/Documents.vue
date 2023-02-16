<script setup>
import { ref } from '@vue/reactivity';
import { computed, onMounted } from '@vue/runtime-core';
import Header from '../components/Header.vue';
import Search from '../components/Search.vue';
import ListItem from '../components/ListItem.vue';
import DOCUMENT_JSON from '../store/data.json';

const categories = ref(DOCUMENT_JSON.CATEGORIES);
const list = ref(DOCUMENT_JSON.LIST);

const search = ref('');
const draggableElement = ref('');
const coloredElement = ref('');

const filteredList = computed(() =>
  list.value.filter(
    (c) => ~c.title.toLowerCase().indexOf(search.value.toLowerCase())
  )
);
const filteredCategories = computed(() => {
  return categories.value.filter((c) => {
    if (~c.title.toLowerCase().indexOf(search.value.toLowerCase())) {
      return c;
    } else {
      if (
        c.list.filter(
          (child) =>
            ~child.title.toLowerCase().indexOf(search.value.toLowerCase())
        ).length
      ) {
        return c;
      }
    }
  });
});

const startDragHandler = (e, item, list) => {
  draggableElement.value = item.id;
  console.log('draggableEl: ', draggableElement.value);

  e.dataTransfer.dropEffect = 'move';
  e.dataTransfer.effectAllowed = 'move';
  e.dataTransfer.setData('item', JSON.stringify(item));
  e.dataTransfer.setData('name', list);
};

const onDropHandler = (e, name) => {
  const itemString = e.dataTransfer.getData('item');
  const listName = e.dataTransfer.getData('name');
  const item = JSON.parse(itemString);

  if (!name.find((i) => i.id === item.id)) {
    switch (listName) {
      case 'categories':
        categories.value = categories.value.filter((i) => i.id !== item.id);
        list.value.push(item);
        break;
      case 'list':
        list.value = list.value.filter((i) => i.id !== item.id);
        categories.value.push(item);
        break;
    }
  } else {
    const ids = name.map((i) => i.id);

    [name[ids.indexOf(item.id)], name[ids.indexOf(coloredElement.value)]] = [
      name[ids.indexOf(coloredElement.value)],
      name[ids.indexOf(item.id)],
    ];
  }

  const ul = e.currentTarget;
  ul.querySelectorAll('li').forEach((li) => li.classList.remove('drop-area'));
};

const onDragEnterHandler = (e, id) => {
  const li = e.currentTarget ? e.currentTarget : '';

  if (draggableElement.value !== id && li) {
    coloredElement.value = id;
    li.classList.add('drop-area');
  }
};

const onDragLeaveHandler = (e, id) => {
  const li = e.currentTarget.closest('li');

  if (coloredElement.value === id && li) li.classList.remove('drop-area');
};

onMounted(() => {});
</script>
<template>
  <Header />
  <main class="main">
    <div class="main-top">
      <Search v-model="search" />
    </div>
    <div class="main-body">
      <ul
        class="category-list list"
        @drop="onDropHandler($event, categories)"
        @dragenter.prevent
        @dragover.prevent
      >
        <ListItem
          v-for="category in filteredCategories"
          :item="category"
          :search="search"
          :key="category.id"
          draggable="true"
          @dragstart="startDragHandler($event, category, 'categories')"
          @dragenter="onDragEnterHandler($event, category.id)"
          @dragleave="onDragLeaveHandler($event, category.id)"
        >
          <div
            v-if="category.list && category.list.length"
            class="sub-list-item"
          >
            <ul
              class="list"
              @drop.stop="onDropHandler($event, list)"
              @dragenter.prevent
              @dragover.prevent
            >
              <ListItem
                v-for="item in category.list"
                :item="item"
                :search="search"
                :key="item.id"
                draggable="true"
                @dragstart="startDragHandler($event, item, 'list')"
                @dragenter="onDragEnterHandler($event, item.id)"
                @dragleave="onDragLeaveHandler($event, item.id)"
              />
            </ul>
          </div>
        </ListItem>
      </ul>
      <ul
        class="list"
        @drop="onDropHandler($event, list)"
        @dragenter.prevent
        @dragover.prevent
      >
        <ListItem
          v-for="item in filteredList"
          :item="item"
          :search="search"
          :key="item.id"
          draggable="true"
          @dragstart="startDragHandler($event, item, 'list')"
          @dragenter="onDragEnterHandler($event, item.id)"
          @dragleave="onDragLeaveHandler($event, item.id)"
        />
      </ul>
    </div>
  </main>
</template>

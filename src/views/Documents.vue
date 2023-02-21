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
  list.value.filter((c) => {
    return ~c.title.toLowerCase().indexOf(search.value.toLowerCase());
  })
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
  draggableElement.value = item;

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
    const type = !!name.filter((n) => n.list).length;
    const listType = !!item.list;

    switch (listName) {
      case 'categories':
        if (type) {
          categories.value = categories.value.filter((i) => i.id !== item.id);
          list.value.push(item);
        }
        break;
      case 'list':
        if (listType) {
          list.value = list.value.filter((i) => i.id !== item.id);
          categories.value.push(item);
        } else if (coloredElement.value) {

          const ids = name.map((i) => i.id);
          const index = ids.indexOf(coloredElement.value) + 1;
          const cids = categories.value.map((ci, index) => {
            return { index, ci: ci.list.map(j => j.id) }
          });

          let cindex;

          if (cids.find(f => f.ci.includes(item.id))) {
            cindex = cids.find(f => f.ci.includes(item.id)).index;

            list.value.splice(index, 0, item);
            categories.value[cindex].list = categories.value[cindex].list.filter(k => k.id !== item.id);
          } else {
            cindex = cids.find(f => f.ci.includes(coloredElement.value)).index;

            categories.value[cindex].list.splice(index, 0, item);
            list.value = list.value.filter(i => i.id !== item.id);
          }
        }
        break;
    }
  } else {
    const ids = name.map((i) => i.id);

    if (item.id && coloredElement.value) {
      [name[ids.indexOf(item.id)], name[ids.indexOf(coloredElement.value)]] = [
        name[ids.indexOf(coloredElement.value)],
        name[ids.indexOf(item.id)],
      ];
    }
  }

  coloredElement.value = '';
};

const onDragEnterHandler = (item) => {
  const isList = !!item.list;
  const isDraggableList = !!draggableElement.value.list;

  if (draggableElement.value.id !== item.id && isList === isDraggableList) {
    if (coloredElement.value !== item.id) {
      coloredElement.value = item.id;
    }
  } else {
    coloredElement.value = '';
  }
};
</script>
<template>
  <Header />
  <main class="main">
    <div class="main-top">
      <Search v-model="search" />
    </div>
    <div class="main-body">
      <ul class="category-list list" @drop="onDropHandler($event, categories)" @dragenter.prevent @dragover.prevent>
        <ListItem v-for="category in filteredCategories" :class="{ 'drop-area': coloredElement === category.id }"
          :item="category" :search="search" :key="category.id" 
          @dragstart.stop="startDragHandler($event, category, 'categories')"
          @dragenter.stop="onDragEnterHandler(category)">
          <div v-if="category.list && category.list.length" class="sub-list-item">
            <ul class="list" @drop.stop="onDropHandler($event, category.list)" @dragenter.prevent @dragover.prevent>
              <ListItem v-for="item in category.list" :class="{ 'drop-area': coloredElement === item.id }" :item="item"
                :search="search" :key="item.id"  @dragstart.stop="startDragHandler($event, item, 'list')"
                @dragenter.stop="onDragEnterHandler(item)"/>
            </ul>
          </div>
        </ListItem>
      </ul>
      <ul class="list" @drop="onDropHandler($event, list)" @dragenter.prevent @dragover.prevent>
        <ListItem v-for="item in filteredList" :class="{ 'drop-area': coloredElement === item.id }" :item="item"
          :search="search" :key="item.id"  @dragstart="startDragHandler($event, item, 'list')"
          @dragenter="onDragEnterHandler(item)"/>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from '@vue/reactivity';
import { computed } from '@vue/runtime-core';
import Header from '../components/Header.vue';
import Search from '../components/Search.vue';

const DOCUMENT_JSON = {
  CATEGORIES: [
    {
      id: '1117',
      title: 'Обязательные для всех',
      types: ['pink', 'yellow', 'orange'],
      description:
        'Документы, обязательные для всех сотрудников без исключения',
      list: [
        {
          id: '3687',
          title: 'Паспорт',
          types: ['sky'],
          format: 'Обязательный',
          description: 'Для всех',
        },
        {
          id: '4658',
          title: 'Инн',
          types: [],
          format: 'Обязательный',
          description: 'Для всех',
        },
      ],
    },
    {
      id: '9889',
      title: 'Обязательные для трудоустройства',
      types: [],
      description:
        'Документы, без которых невозможно трудоустройство человека на какую бы то ни было должность в компании вне ависимости от граж',
      list: [],
    },
    {
      id: '0665',
      title: 'Специальные',
      types: [],
      description: '',
      list: [],
    },
  ],
  LIST: [
    {
      id: '7183',
      title: 'Тестовое задание кандидата',
      types: [],
      format: '',
      description:
        'Россия, Белоруссия, Украина, администратор филиала, повар-сушист, повар-пиццмейкер, повар горячего цеха',
    },
    {
      id: '9764',
      title: 'Трудовой договор',
      types: ['blue', 'grey'],
      format: '',
      description: '',
    },
    {
      id: '2372',
      title: 'Мед. книжка',
      types: [],
      format: '',
      description: '',
    },
  ],
};

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

const listItemClickHandler = (e) => {
  const el = e.currentTarget;

  if (!el.classList.contains('empty')) {
    el.classList.contains('open')
      ? el.classList.remove('open')
      : el.classList.add('open');
  }
};

const listItemMoveHandler = (e) => {
  const el = e.currentTarget.closest('li');

  el.classList.contains('clone')
    ? el.classList.remove('clone')
    : el.classList.add('clone');
};

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
  }
};

const onDragEnterHandler = (e, id) => {
  const li = e.fromElement ? e.target.closest('li') : '';

  console.log('enter: ', `draggableEl - ${draggableElement.value}, to: ${id}`);

  if ((draggableElement.value !== id) && li) {
    coloredElement.value = id;
    li.classList.add('drop-area');
  }
};
const onDragOverHandler = (e) => {
  //const li = e.fromElement.closest('li');
  //if (li) li.classList.remove('drop-area');
};

const onDragLeaveHandler = (e, id) => {
  const li = e.target.closest('li');

  console.log('leave: ', `draggableEl - ${draggableElement.value}, to: ${id}`);
  if ((coloredElement.value === id) && li) li.classList.remove('drop-area');
};

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
        @dragenter.prevent="onDragEnterHandler($event, category.id)"
        @dragover.prevent="onDragLeaveHandler($event, category.id)"
      >
        <li v-for="category in filteredCategories" :key="category.id">
          <div
            class="category-list-item"
            :class="{
              empty: !category.list || !category.list.length,
              open: search,
            }"
            @click="listItemClickHandler"
            draggable="true"
            @dragstart="startDragHandler($event, category, 'categories')"
          >
            <span class="category-list-title">{{ category.title }}</span>
            <span v-if="category.types.length" class="list-color">
              <i
                v-for="t in category.types"
                class="circle"
                :class="t"
                :key="t"
              ></i>
            </span>
            <span v-if="category.description" class="list-description">{{
              category.description
            }}</span>
            <span class="list-action">
              <i class="action-edit"></i>
              <i class="action-delete"></i>
              <i class="action-move" @click.stop="listItemMoveHandler"></i>
            </span>
          </div>
          <div
            v-if="category.list && category.list.length"
            class="sub-list-item"
          >
            <ul class="list">
              <li v-for="l in category.list" :key="l.id">
                <div class="list-item">
                  <span class="list-title">{{ l.title }}</span>
                  <span class="list-color">
                    <i
                      v-for="t in l.types"
                      class="circle"
                      :class="t"
                      :key="t"
                    ></i>
                  </span>
                  <span v-if="l.format" class="list-description pink-color">{{
                    l.format
                  }}</span>
                  <span v-if="l.description" class="list-description">{{
                    l.description
                  }}</span>
                  <span class="list-action">
                    <i class="action-edit"></i>
                    <i class="action-delete"></i>
                    <i class="action-move" @click="listItemMoveHandler"></i>
                  </span>
                </div>
              </li>
            </ul>
          </div>
        </li>
      </ul>
      <ul
        class="list"
        @drop="onDropHandler($event, list)"
        @dragenter.prevent
        @dragover.prevent
      >
        <li
          v-for="item in filteredList"
          :key="item.id"
          draggable="true"
          @dragstart="startDragHandler($event, item, 'list')"
        >
          <div class="list-item">
            <span class="list-title">{{ item.title }}</span>
            <span v-if="item.types.length" class="list-color">
              <i v-for="t in item.types" class="circle" :class="t" :key="t"></i>
            </span>
            <span class="list-description">{{ item.description }}</span>
            <span class="list-action">
              <i class="action-edit"></i>
              <i class="action-delete"></i>
              <i class="action-move" @click="listItemMoveHandler"></i>
            </span>
          </div>
        </li>
      </ul>
    </div>
  </main>
</template>

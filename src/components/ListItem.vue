<script setup>
const props = defineProps({
  item: Object,
  search: String,
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
</script>
<template>
  <li>
    <div
      :class="{
        'category-list-item': item.list,
        'list-item': !item.list,
        empty: !item.list || !item.list.length,
        open: search,
      }"
      @click="listItemClickHandler"
    >
      <span
        :class="{
          'category-list-title': item.list,
          'list-title': !item.list,
        }"
        >{{ item.title }}</span
      >
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
    <!-- <div v-if="item.list && item.list.length" class="sub-list-item">
      <ul class="list">
        <li v-for="l in item.list" :key="l.id">
          <div class="list-item">
            <span class="list-title">{{ l.title }}</span>
            <span class="list-color">
              <i v-for="t in l.types" class="circle" :class="t" :key="t"></i>
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
              <i class="action-move" @click.stop="listItemMoveHandler"></i>
            </span>
          </div>
        </li>
      </ul>
    </div> -->
  </li>
</template>

<template>
  <li class="list-item">
    <h3>{{list.value}}</h3>
    <SortableList v-model="list.items" class="shortList">
      <SortableItem v-for="(item, index) in list.items" :key="item.id" collection="lists" :index="index" :item="item" />
    </SortableList>
  </li>
</template>

<script>
import { ContainerMixin, ElementMixin } from 'vue-slicksort';

const SortableList = {
  mixins: [ContainerMixin],
  template: `
     <div>
       <slot/>
     </div>
  `
};

const SortableItem = {
  mixins: [ElementMixin],
  props: ['item'],
  template: `
    <li class="list-item">{{ JSON.stringify(item) }}</li>
  `
};

export default {
  mixins: [ElementMixin],
  props: ['list'],
  components: {
    SortableItem,
    SortableList,
  },
  data() {
    return {
      items: this.list.items.slice(0),
    };
  },
}
</script>
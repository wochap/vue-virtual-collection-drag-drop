<template>
   <div>
      <SortableList 
        ref="virtualCollection"
        :cellSizeAndPositionGetter="cellSizeAndPositionGetter"
        :collection="itemCollection"
        class="w-list" 
        lockAxis="y" 
        v-model="itemCollection"
      >
        <template slot-scope="slotProps">
         <SortableItem 
           collection="lists"
          :index="slotProps.data.index" 
          :key="slotProps.data.index" 
          :item="slotProps.data"
        />
        </template>
        
      </SortableList>
    </div>
</template>

<script>
import Vue from 'vue';
import { ContainerMixin, ElementMixin } from 'vue-slicksort';
import InnerList from "./InnerList"

const SortableList = {
  mixins: [ContainerMixin],
  template: `
     <VirtualCollection
        :cellSizeAndPositionGetter="$attrs.cellSizeAndPositionGetter"
        :collection="$attrs.collection"
        :height="300"
        :width="400"
        :style="{ overflowX: 'hidden', overflowY: 'auto' }"
    >
        <template slot="cell" slot-scope="slotProps">
          <slot v-bind="slotProps" />
        </template>
    </VirtualCollection>
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
  components: {
    SortableItem,
    SortableList,
    InnerList,
  },
  data() {
    return {
      itemCollection: new Array(1000).fill(0).map((_, index) => ({ data: { item: '#' + index, index } }))
    };
  },
  methods: {
    cellSizeAndPositionGetter(item, index) {
        const rowWidth = 300;
        return {
            height: 100,
            y: 100 * index,
            width: rowWidth,
            x: 0
        };
    },
  }
};
</script>


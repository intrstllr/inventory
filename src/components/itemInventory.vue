<script lang="ts">
import type { ItemInventoryType } from "../types";
import { defineComponent } from "vue";
import type { PropType } from "vue";
export default defineComponent({
  props: {
    item: {
      type: Object as PropType<ItemInventoryType>,
      required: true,
    },
  },
});
</script>

<template>
  <div class="item_inventory">
    <div class="item_image_wrapper">
      <div class="item_image">
        <img :src="item.image" :alt="`image`" draggable="false" />
      </div>
    </div>
    <div class="count_wrapper">
      <div class="count">
        {{ item.count }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.item_inventory {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  height: 100%;
}
.item_image_wrapper {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: end;
}

.count_wrapper {
  height: 16px;
  width: 100%;
  display: flex;
  justify-content: end;
}
.count {
  color: #7d7d7d;
  display: flex;
  align-items: center;
  border-top-left-radius: 6px;
  border: 1px solid #4d4d4d;
  padding: 4px;
  font-size: 10px;
  font-weight: 500;
  line-height: 12.1px;
  text-align: center;
  text-underline-position: from-font;
  text-decoration-skip-ink: none;
}
</style>

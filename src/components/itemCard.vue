<script lang="ts">
import type { ItemInventoryType } from "../types";
import { defineComponent, ref } from "vue";
import type { PropType } from "vue";
export default defineComponent({
  props: {
    item: {
      type: Object as PropType<ItemInventoryType>,
      required: true,
    },
    isSuccesDeleteItem: {
      type: Boolean as PropType<boolean>,
      required: true,
    },
  },
  setup() {
    const countDelete = ref<number>(0);
    const showLabel = ref<boolean>(true);
    return { countDelete, showLabel };
  },

  computed: {},
});
</script>

<template>
  <div class="item_inventory">
    <div class="close_btn_wrapper" @click="$emit('closeCard')">
      <img class="close_btn" src="/src/assets/closeBtn.svg" />
    </div>
    <div class="item_image_wrapper" draggable="false">
      <div class="item_image" draggable="false">
        <img src="/src/assets/Item_Image_green_big.png" :alt="`image`" draggable="false"/>
      </div>
    </div>
    <div class="info_text">
      <div class="big_skeleton"></div>
      <div class="sleleton_list">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
      </div>
      <div></div>
    </div>
    <div class="empty"></div>
    <div class="delete_btn_wrapper" v-if="!isSuccesDeleteItem">
      <button @click="$emit('successDeleteItem')" class="delete_btn">
        Удалить предмет
      </button>
    </div>
    <div class="delete_success_wrapper" v-if="isSuccesDeleteItem">
      <input
        class="count_delete_item"
        v-model="countDelete"
        placeholder="Введите значение"
      />

      <div class="delete_btn_wrapper">
        <button
          @click="
            () => {
              $emit('cancellationDeleteItem');
              countDelete = 0;
            }
          "
          class="cancellation_btn"
        >
          Отмена
        </button>
        <button
          @click="$emit('deleteItem', countDelete)"
          class="delete_btn_success"
        >
          Удалить
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.item_image_wrapper {
  width: 220px;
  margin: 12px 0px;
}
.item_image {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.item_inventory {
  width: 250px;
  height: 500px;
  display: flex;
  flex-direction: column;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 10px;
  position: relative;
  top: 0;
  left: 40px;
  z-index: 1000;
  backdrop-filter: blur(10px);
  transition: left 0.4s ease;
  justify-content: space-between;
}
.sleleton_list {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 25px;
  border-top: 1px solid #4d4d4d;
  border-bottom: 1px solid #4d4d4d;
  padding: 12px 0px;
}

.sleleton_list div {
  background: linear-gradient(90deg, #3c3c3c 0%, #444444 51.04%, #333333 100%);
  border-radius: 10px;
}

.sleleton_list div:nth-child(1) {
  width: 190px;
  height: 26px;
}

.sleleton_list div:nth-child(2) {
  width: 155px;
  height: 10px;
}

.sleleton_list div:nth-child(3) {
  width: 190px;
  height: 10px;
}

.sleleton_list div:nth-child(4) {
  width: 170px;
  height: 10px;
}

.sleleton_list div:nth-child(5) {
  width: 160px;
  height: 10px;
}
.sleleton_list div:nth-child(6) {
  width: 140px;
  height: 10px;
}
.sleleton_list div:nth-child(7) {
  width: 80px;
  height: 10px;
}

.close_btn_wrapper {
  display: flex;
  justify-content: end;
}
.close_btn {
  height: 24px;
  width: 24px;
  cursor: pointer;
}
.delete_btn_wrapper {
  width: 100%;
  height: 50px;
  display: flex;
  justify-content: space-around;
  align-items: center;
}
.delete_btn {
  width: 220px;
  height: 39px;
  gap: 0px;
  opacity: 0px;
  background-color: #fa7272;
  color: white;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  font-size: 18px;
  font-weight: 400;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  cursor: pointer;
}
.cancellation_btn {
  width: 90px;
  height: 33px;
  gap: 0px;
  opacity: 0px;
  background-color: white;
  color: black;
  font-size: 16px;
  font-weight: 400;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 7px;
  cursor: pointer;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  font-size: 18px;
  font-weight: 400;
  padding: 0px 8px;
}
.delete_btn_success {
  width: 120px;
  height: 33px;
  gap: 0px;
  opacity: 0px;
  background-color: #fa7272;
  color: white;
  font-size: 18px;
  font-weight: 400;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 7px;
  cursor: pointer;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  font-size: 18px;
  font-weight: 400;
  padding: 0px 8px;
}
.delete_success_wrapper {
  background-color: #262626;
  position: absolute;
  padding: 8px 0px;
  width: 220px;
  bottom: 0;
  height: 90px;
  border-top: 1px solid #7d7d7d;
  display: flex;
  flex-direction: column;
  justify-content: end;
  align-items: center;
}
.count_delete_item {
  margin: 0px 8px;
  padding: 10px 12px;
  border-radius: 10px;
  border: 1px solid #ccc;
  width: 100%;
  max-width: 180px;
  color: #7d7d7d;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  font-size: 14px;
  font-weight: 400;
  background-color: #262626;
  border: 1px solid #7d7d7d;
}
.count_delete_item:focus {
  margin: 0px 8px;
  padding: 10px 12px;
  border-radius: 10px;
  border: 1px solid #ccc;
  width: 100%;
  max-width: 180px;
  color: #7d7d7d;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  font-size: 14px;
  font-weight: 400;
  background-color: #262626;
  border: 1px solid #7d7d7d;
}
.empty {
  width: 100%;
  height: 60px;
}
</style>

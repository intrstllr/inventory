<script lang="ts">
import type { ItemInventoryType } from "../src/types";
import ItemInventory from "./components/itemInventory.vue";
import { defineComponent, ref } from "vue";
import ItemCard from "./components/itemCard.vue";
const items = ref<ItemInventoryType[][]>([
  [
    { image: "/src/assets/Item_Image_green.png", count: 4, id: 1 },
    { image: "/src/assets/Item_Image_orange.png", count: 2, id: 2 },
    { image: "/src/assets/Item_Image_blue.png", count: 5, id: 3 },
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
  ],
  [
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
  ],
  [
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
  ],
  [
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
  ],
  [
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
    { image: "", count: 0, id: 0 },
  ],
]);

export default defineComponent({
  components: {
    ItemInventory,
    ItemCard,
  },
  setup() {
    const draggedItem = ref<ItemInventoryType>({ image: "", count: 0, id: 0 });
    const draggedFromRow = ref<number>(-1);
    const draggedFromIndex = ref<number>(-1);
    const showItemCard = ref<boolean>(false);
    const isSuccesDeleteItem = ref<boolean>(false);
    const selectedItem = ref<ItemInventoryType>({
      image: "",
      count: 0,
      id: 0,
    });
    const selectedItemRowIndex = ref<number>(-1);
    const selectedItemColIndex = ref<number>(-1);
    const dragStart = (
      event: DragEvent,
      item: ItemInventoryType,
      rowIndex: number,
      itemIndex: number
    ) => {
      console.log(event);
      const target = event.target as HTMLElement;
      target.style.border = "2px solid #4d4d4d";
      target.style.borderRadius = "10px";
      draggedItem.value = item;
      draggedFromRow.value = rowIndex;
      draggedFromIndex.value = itemIndex;
    };
    const drop = (droptoRow: number, itemIndex: number) => {
      if (items.value[droptoRow][itemIndex].id == 0) {
        items.value[droptoRow][itemIndex] = draggedItem.value;
        items.value[draggedFromRow.value][draggedFromIndex.value] = {
          image: "",
          count: 0,
          id: 0,
        };
        dragEnd();
      }
    };
    const dragEnd = () => {
      draggedItem.value = { image: "", count: 0, id: 0 };
      draggedFromRow.value = -1;
      draggedFromIndex.value = -1;
    };
    const showItem = (
      item: ItemInventoryType,
      rowIndex: number,
      itemIndex: number
    ) => {
      showItemCard.value = true;
      selectedItem.value = item;
      selectedItemRowIndex.value = rowIndex;
      selectedItemColIndex.value = itemIndex;
    };

    return {
      items,
      dragStart,
      dragEnd,
      drop,
      showItem,
      showItemCard,
      selectedItem,
      isSuccesDeleteItem,
      selectedItemRowIndex,
      selectedItemColIndex,
    };
  },
});
</script>

<template>
  <div class="content_wrapper">
    <div class="content_box">
      <div class="content">
        <div class="info_wrapper">
          <div class="info_card">
            <div class="image">
              <img src="/src/assets/info.png" alt="Product Image" />
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
          </div>
        </div>
        <div class="inventory_wrapper">
          <div class="inventory">
            <div
              v-for="(itemRow, rowIndex) in items"
              :key="rowIndex"
              class="info_item_inventory_row"
              @dragover.prevent
            >
              <div
                v-for="(item, itemIndex) in itemRow"
                :key="itemIndex"
                class="item_inventory_wrapper"
                :draggable="item.count > 0"
                @dragstart="dragStart($event, item, rowIndex, itemIndex)"
                @dragend="dragEnd"
                @drop="drop(rowIndex, itemIndex)"
                @click="
                  item.id == 1 ? showItem(item, rowIndex, itemIndex) : null
                "
                :style="item.id > 0 ? `cursor: pointer;` : null"
              >
                <ItemInventory :item="item" v-if="item.count > 0" />
              </div>
            </div>
          </div>
          <ItemCard
            v-if="selectedItem.count > 0"
            :item="selectedItem"
            :isSuccesDeleteItem="isSuccesDeleteItem"
            :style="showItemCard ? `left:-240px;` : `left: 40px;`"
            @closeCard="
              () => {
                showItemCard = false;
                isSuccesDeleteItem = false;
                selectedItemRowIndex = -1;
                selectedItemColIndex = -1;
              }
            "
            @successDeleteItem="
              () => {
                isSuccesDeleteItem = true;
              }
            "
            @cancellationDeleteItem="
              () => {
                isSuccesDeleteItem = false;
              }
            "
            @deleteItem="
              (count) => {
                items[selectedItemRowIndex][selectedItemColIndex].count =
                  items[selectedItemRowIndex][selectedItemColIndex].count -
                  Number(count);
                items[selectedItemRowIndex][selectedItemColIndex].count < 0
                  ? (items[selectedItemRowIndex][selectedItemColIndex] = {
                      count: 0,
                      image: '',
                      id: 0,
                    })
                  : null;
              }
            "
          ></ItemCard>
        </div>
      </div>
      <div class="input_skeleton_warapper">
        <div class="input_skeleton"></div>
        <div class="close_btn_wrapper">
          <img class="close_btn" src="/src/assets/closeBtn.svg" />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.content_wrapper {
  display: flex;
  justify-content: center;
  width: 100%;
  height: 100%;
}
.content_box {
  width: 920px;
  height: 100%;
  overflow: hidden;
}
.content {
  display: flex;
  justify-content: space-between;
  padding: 32px;
}
.info_wrapper {
  height: 495px;
}
.info_card {
  width: 236px;
  height: 100%;
  background: #262626;
  border: 1px solid #4d4d4d;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 18px 18px 0px 18px;
}
.image {
  backdrop-filter: blur(12px);
  width: 208px;
  height: 240px;
  border-radius: 10px;
}
.info_text {
  flex: 1 1 auto;
  width: 100%;
  padding-top: 16px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.sleleton_list {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 25px;
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
.inventory_wrapper {
  display: flex;
  width: 100vw;
  max-width: 535px;
}
.inventory {
  height: 100%;
  background: #262626;
  border-radius: 10px;
}
.info_item_inventory_row {
  width: 100%;
  display: flex;
}
.item_inventory_wrapper {
  height: 101px;
  width: 105px;
  border: 1px solid #4d4d4d;
}
.inventory
  .info_item_inventory_row:first-child
  .item_inventory_wrapper:first-child {
  border-radius: 10px 0px 0px 0px;
}
.inventory
  .info_item_inventory_row:first-child
  .item_inventory_wrapper:last-child {
  border-radius: 0px 10px 0px 0px;
}
.inventory
  .info_item_inventory_row:last-child
  .item_inventory_wrapper:first-child {
  border-radius: 0px 0px 0px 10px;
}
.inventory
  .info_item_inventory_row:last-child
  .item_inventory_wrapper:last-child {
  border-radius: 0px 0px 10px 0px;
}
.input_skeleton_warapper {
  height: 72px;
  background: #262626;
  border: 1px solid #4d4d4d;
  border-radius: 10px;
  margin: 0px 32px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  padding: 0px 16px;
  justify-content: space-between;
}
.input_skeleton {
  width: 100%;
  max-width: 770px;
  height: 36px;
  border-radius: 10px;
  background: linear-gradient(90deg, #3c3c3c 0%, #444444 51.04%, #333333 100%);
}
.close_btn_wrapper {
  height: 100%;
}
.close_btn {
  height: 24px;
  width: 24px;
  margin-top: 8px;
}
.item_inventory_wrapper {
  transition: transform 0.5s ease;
}
</style>

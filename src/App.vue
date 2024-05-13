<template>
  <VueDraggable
    ref="el"
    v-model="imagesOnPage"
    class="pics"
    ghostClass="ghost"
    :animation="150"
    @end="changeOrd"
    @move="(event) => onMove(event)"
  >
    <img
      v-for="(image, index) in imagesOnPage"
      :src="image?.url"
      alt="silly cat"
      :key="index"
      :data-dataset-id="image.id"
    />
    <br />
    <div data-dataset-action="nextPage"></div>
  </VueDraggable>
  <br />
  <div class="drag-to-next-page">
    <strong>Перетащить на следующую страницу</strong>
  </div>
  <br />
  <div>Full array: {{ images }}</div>
  <div>Page array: {{ imagesOnPage }}</div>
  <Paginator
    v-model:first="firstRow"
    :rows="rowsOnPage"
    :totalRecords="images.length"
    @page="onPage"
  />
</template>

<script setup>
import Paginator from "primevue/paginator";

import { ref } from "vue";
import { VueDraggable } from "vue-draggable-plus";

const images = ref([
  { id: 1, ord: 0, url: "\Luna.jpeg" },
  { id: 2, ord: 1, url: "\Huh.jpg" },
  { id: 3, ord: 2, url: "\Orange.jpg" },
  { id: 4, ord: 3, url: "\Sus.jpeg" },
]);

const menuItems = ref([{ label: "Переместить", icon: "pi pi-copy" }]);

const el = ref(null);
const nexPageDragging = ref(null);

const firstRow = ref(0);
const rowsOnPage = ref(2);

const imagesOnPage = ref(
  images.value.slice(firstRow.value, firstRow.value + rowsOnPage.value)
);

const changeOrd = () => {
  imagesOnPage.value.forEach((img, index) => {
    img.ord = index + firstRow.value;
    const targetImg = images.value.find((_img) => _img.id === img.id);
    targetImg.ord = img.ord;
  });

  sortByOrd();
};
const onMove = (moveEvent) => {
  if (
    moveEvent.related.dataset.datasetAction === "nextPage" &&
    firstRow.value + rowsOnPage.value + 1 < images.value.length
  ) {
    const targetImgId = moveEvent.dragged.dataset.datasetId;
    const targetImgIndex = images.value.findIndex(
      (img) => img.id == targetImgId
    );
    [
      images.value[firstRow.value + rowsOnPage.value].ord,
      images.value[targetImgIndex].ord,
    ] = [
      images.value[targetImgIndex].ord,
      images.value[firstRow.value + rowsOnPage.value].ord,
    ];
    sortByOrd();
    updatePageArray();
  }
};
const onPage = () => {
  updatePageArray();
};

const updatePageArray = () => {
  imagesOnPage.value = images.value.slice(
    firstRow.value,
    firstRow.value + rowsOnPage.value
  );
};

const sortByOrd = () => {
  images.value.sort((a, b) => a.ord - b.ord);
};
</script>

<style scoped lang="scss">
.pics {
  display: flex;
  flex-direction: column;
  width: 250px;
  padding: 25px;
}
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
.drag-to-next-page {
  width: 20%;
  padding: 10px;
  border: 1px solid black;
}
</style>

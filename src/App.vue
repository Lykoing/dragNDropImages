<template>
  <VueDraggable
    ref="el"
    v-model="links"
    class="pics"
    ghostClass="ghost"
    :animation="150"
  >
    <img v-for="link in links" :src="link" alt="silly cat" />
  </VueDraggable>
  {{ links }}
</template>

<script setup>
import { ref } from "vue";
import { VueDraggable } from "vue-draggable-plus";

const links = ref(["\Luna.jpeg", "\Huh.jpg", "\Orange.jpg"]);
const el = ref(null);
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
</style>

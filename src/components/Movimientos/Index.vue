<template>
  <div class="movements">
    <h2 class="tittle">History</h2>
    <div class="content">
      <Movimiento
        v-for="movement in movementstlist"
        :key="movement.id"
        :id="movement.id"
        :tittle="movement.tittle"
        :description="movement.description"
        :amount="movement.amount"
        @remove="remove"
      >
      </Movimiento>
    </div>
  </div>
</template>

<script setup>
import { toRefs, defineProps, defineEmits } from "vue";
import Movimiento from "./Movimiento.vue";
const props = defineProps({
  movementstlist: {
    type: Array,
    default: () => [],
  },
});
const remove = (id) => {
  console.log("la noche que te fuistererer  ", id);
  emit("remove", id);
};
const emit = defineEmits(["remove"]);
</script>

<style scoped>
.movements {
  max-height: 100%;
  padding: 0 8px;
  margin-bottom: 14px;
}
.title {
  margin: 8px 16px 24px 16px;
  color: var(--brand-blue);
}
.content {
  max-height: 68vh;
  display: flex;
  flex-direction: column;
  gap: 8px;
  overflow-y: scroll;
}
</style>

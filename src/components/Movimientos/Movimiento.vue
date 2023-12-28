<template>
  <div class="movement">
    <div class="content">
      <h4>{{ tittle }}</h4>
      <p>{{ description }}</p>
    </div>
    <div class="action">
      <img src="@/assets/trash-icon.svg" alt="trash" @click="remove" />
      <p
        :class="{
          red: amount < 0,
          green: amount > 0,
        }"
      >
        {{ amountCurrency }}
      </p>
    </div>
  </div>
</template>

<script setup>
import { defineProps, computed, defineEmits } from "vue";
const props = defineProps({
  tittle: {
    type: String,
    default: "",
  },
  description: {
    type: String,
    default: "",
  },
  amount: {
    type: Number,
    default: 0,
  },
  id: {
    type: Number,
    default: 0,
  },
});

const amountCurrency = computed(() => {
  return new Intl.NumberFormat("en-US", {
    style: "currency",
    currency: "USD",
  }).format(props.amount);
});
const remove = () => {
  console.log("remove", props.id);
  emit("remove", props.id);
};
const emit = defineEmits(["remove"]);
</script>

<style scoped>
.movement {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 16px;
  background-color: #e6f9ff;
  border-radius: 8px;
  box-sizing: border-box;
}
.movement .content {
  width: 100%;
}
.movement .action {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-direction: column;
}
h4,
p {
  margin: 0;
  padding: 0;
}
h4 {
  margin-bottom: 8px;
}
.movement .action img {
  margin-bottom: 16px;
}
.red {
  color: red;
}
.green {
  color: green;
}
</style>

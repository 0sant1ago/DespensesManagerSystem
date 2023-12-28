<template>
  <div>
    <svg
      @touchstart="tap"
      @touchmove="tap"
      @touchend="untap"
      viewBox="0 0 300 200"
    >
      <line
        stroke="#c4c4c4"
        stroke-width="2"
        x1="0"
        :y1="cero"
        x2="300"
        :y2="cero"
      />
      <polyline
        fill="none"
        stroke="#0689B0"
        stroke-width="2"
        :points="puntos"
      />
      <!-- l
            x -> , yv -->
      <line
        v-show="showPointer"
        stroke="#04b500"
        stroke-width="2"
        :x1="puntero"
        y1="0"
        :x2="puntero"
        y2="200"
      />
    </svg>
    <p>Last 30 Days</p>
  </div>
</template>

<script setup>
import { defineProps, computed, ref, defineEmits } from "vue";
// const hola="0,0 100,0 100,100 200,100 300,200"
const props = defineProps({
  amounts: {
    type: Array,
    default: () => [],
  },
});

const posEnY = (y) => {
  let mi = Math.min(...props.amounts);
  let ma = Math.max(...props.amounts);
  mi = Math.min(mi, y);
  ma = Math.max(ma, y);
  const rango = ma - mi;
  const falsoMa = ma - mi;
  const falsoMi = 0;
  const falsoCurr = y - mi;
  //falsoMA=0
  //falsoMi=200
  const dif = falsoMa - falsoCurr;
  if (y == 0) {
    console.log("ma", ma, "mi", mi);
    console.log("rangoo   ", rango);
    console.log("mi dif", dif);
  }
  //rango ->200
  //dif -> x
  const res = (dif * 200) / rango;
  return res;
};
const cero = computed(() => {
  console.log("fernet  ", posEnY(0));
  return `${posEnY(0)}`;
});
const puntos = computed(() => {
  // console.log("puntos sigan viendo");
  // return "0,50 100,0 100,100 200,100 300,200";
  // x es horizontal
  // y es vertical
  let res = "";
  const n = props.amounts.length;
  console.log(props.amounts);
  let x = 0;
  if (n > 1) x = 300 / (n - 1); //cada espacio

  console.log("somos", n);
  for (let i = 0; i < n; i++) {
    res += `${i * x},${posEnY(props.amounts[i])} `;
  }
  return res;
});

const showPointer = ref(false);
const puntero = ref(0);
const emit = defineEmits(["select"]);

const tap = ({ target, touches }) => {
  showPointer.value = true;
  const elementWith = target.getBoundingClientRect();
  const elementX = target.getBoundingClientRect().x;
  const touchX = touches[0].clientX;
  puntero.value = ((touchX - elementX) * 300) / elementWith.width;
  emit("select", puntero.value);
};
const untap = () => {
  showPointer.value = false;
};
</script>

<style scoped>
svg {
  width: 100%;
}

p {
  text-align: center;
}
</style>

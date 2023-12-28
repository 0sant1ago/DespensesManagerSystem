<template>
  <button @click="encender">Add Transaction</button>
  <teleport to="#app">
    <Modal v-show="showModal" @cerrar="showModal = false">
      <form @submit="enviar">
        <div class="field">
          <label for="">Titulo</label>
          <input type="text" v-model="titulo" />
        </div>
        <div class="field">
          <label for="">Monto</label>
          <input type="Number" v-model="monto" />
        </div>
        <div class="field">
          <label for="">Description</label>
          <textarea rows="5" v-model="descripcion"></textarea>
        </div>
        <div class="field">
          <label for="">Tio de Movimiento</label>
          <label class="radio-label">
            <input type="radio" v-model="tipo" value="Ingreso" />
            <span>Ingreso</span>
          </label>
          <label class="radio-label">
            <input type="radio" v-model="tipo" value="Gasto" />
            <span>Gasto</span>
          </label>
        </div>
        <div class="action">
          <button>Agregar Movimiento</button>
        </div>
      </form>
    </Modal>
  </teleport>
</template>

<script setup>
import Modal from "@/components/Modal.vue";
import { ref, defineEmits } from "vue";

const titulo = ref("");
const monto = ref(0);
const descripcion = ref("");
const tipo = ref("");

const emit = defineEmits(["agregar"]);

const enviar = (evento) => {
  evento.preventDefault();
  showModal.value = false;
  const monton = parseInt(monto.value);
  emit("agregar", {
    id: Math.random(),
    tittle: titulo.value,
    //amount: tipo.value = monto.value,
    amount: tipo.value == "Ingreso" ? monton : -monton,
    description: descripcion.value,
    tipo: tipo.value,
    time: new Date(),
  });
};

const showModal = ref(false);
const encender = () => {
  console.log("funcionas");
  showModal.value = true;
};
</script>

<style scoped>
button {
  color: white;
  font-size: 1.25rem;
  background-color: var(--brand-blue);
  border: none;
  width: 100%;
  padding: 24px 60px;
  border-radius: 60px;
  box-sizing: border-box;
}

form {
  font-size: 1.24rem;
  width: 100%;
}

form .action {
  padding: 0 24px;
}

.field {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  padding: 16px 24px;
}

label {
  margin-bottom: 8px;
}

input,
textarea {
  font-size: 1.24rem;
  border: 2px solid var(--brand-blue);
  border-radius: 8px;
  padding: 8px;
}

input[type="number"] {
  text-align: right;
}

.radio-label {
  display: flex;
  align-items: center;
  margin-top: 8px;
}

.radio-label span {
  margin-top: 4px;
  margin-left: 8px;
}

input[type="radio"] {
  appearance: none;
  width: 1.24rem;
  height: 1.24rem;
  color: var(--brand-blue);
  border: 2px solid var(--brand-blue);
  border-radius: 50%;
}

input[type="radio"]:checked {
  background-color: var(--brand-blue);
}
</style>

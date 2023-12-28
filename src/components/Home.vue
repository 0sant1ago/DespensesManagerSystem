<template>
  <Layout>
    <template v-slot:header>
      <Header></Header>
    </template>
    <template #resume>
      <Resume label="label" :amount="amount" :totalamount="totalamount">
        <template v-slot:graphic>
          <Graphic :amounts="amounts" />
        </template>
        <template #action>
          <Action @agregar="agregar"></Action>
        </template>
      </Resume>
    </template>
    <template #movements>
      <Movements :movementstlist="lista" @remove="remove"> > </Movements>
    </template>
  </Layout>
</template>

<script>
import Layout from "./Layout.vue";
import Header from "./Header.vue";
import Resume from "./Resume/content.vue";
import Movements from "./Movimientos/Index.vue";
import Action from "./Action.vue";
import Graphic from "./Resume/Graphic.vue";

export default {
  name: "Home",
  components: {
    Layout,
    Header,
    Resume,
    Movements,
    Action,
    Graphic,
  },
  data() {
    return {
      label: "Amount?",
      amount: null,
      lista: [
        {
          id: 0,
          tittle: "tittle1",
          amount: 10,
          description: "description1",
          time: new Date("12-28-2023"),
        },
        {
          id: 1,
          tittle: "tittle2",
          amount: 20,
          description: "description2",
          time: new Date("12-28-2023"),
        },
      ],
    };
  },

  computed: {
    amounts() {
      const filtrado = this.lista.filter((curr) => {
        const today = new Date();
        const oldDate = today.setDate(today.getDate() - 30);
        return curr.time >= oldDate;
      });
      console.log("filtrado", filtrado);
      const res = filtrado.map((curr) => {
        return curr.amount;
      });
      console.log("numeros", res);
      return res.map((m, i) => {
        const anteriores = res.slice(0, i);
        const suma = anteriores.reduce((acc, curr) => {
          return acc + curr;
        }, 0);
        return suma + m;
      });
    },
    totalamount() {
      const res = this.lista.map((curr) => {
        return curr.amount;
      });
      const suma = res.reduce((acc, curr) => {
        return acc + curr;
      }, 0);
      return suma;
    },
  },
  methods: {
    agregar(movement) {
      console.log("agregar", movement);
      this.lista.push(movement);
    },
    remove(id) {
      console.log("removee en home", id);
      const index = this.lista.findIndex((curr) => {
        return curr.id === id;
      });
      this.lista.splice(index, 1);
    },
    pruebita() {
      console.log("prueba");
    },
  },
};
</script>

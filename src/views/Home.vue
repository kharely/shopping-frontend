<template>
  <div class="row justify-content-center">
    <div class="col-10 mt-md-5 ">
      <form @submit.prevent="getCost">
        <div class="row justify-content-center">
          <div class="col-12">
          <h3 class="text-dark">List of Products</h3>
            <table class="table table-dark">
              <thead class="bg-success">
                <tr>
                  <th scope="col">Name</th>
                  <th scope="col">Price</th>
                  <th scope="col-1"></th>
                </tr>
              </thead>
              <tbody>
              <tr v-for="item in items" v-bind:key="item.CODE">
                <th scope=row>{{item.NAME}}</th>
                <th>{{item.PRICE }} €</th>
                <th><button class="btn btn-success mr-3" @click="increment(item.CODE)">
                  <i class="fas fa-plus-circle"></i>
                </button>
                <button class="btn btn-success" @click="decrement(item.CODE)">
                  <i class="fas fa-minus-circle"></i>
                </button>
                </th>
              </tr>
              </tbody>
            </table>
          </div>
        </div>
      </form>
      <div class="row justify-content-center mt-4">
        <div class="col-12 col-md-6">
          <h3 class="text-dark">Items</h3>
          <div class="row">
            <p class="text-monospace col-12">Cantidad de Pants: {{shopping.pants}}</p>
            <p class="text-monospace col-12">Cantidad de T-shirt: {{shopping.tshirt}}</p>
            <p class="text-monospace col-12">Cantidad de Hat: {{shopping.hat}}</p>
          </div>
        </div>
        <div class="col-12 col-md-6">
          <h3 class="text-dark">Checkout</h3>
          <table class="table table-dark">
            <thead class="bg-success">
              <tr>
                <th scope="col">Name</th>
                <th scope="col">Price</th>
              </tr>
            </thead>
            <tbody class="text-capitalize">
            <tr v-for="(value, key) in totals" v-bind:key="key">
                <th v-if="key != 'total'" scope=row>{{key}}</th>
                <th v-if="key != 'total'">{{ value }}€</th>
            </tr>
            <tr>
              <th scope=row>TOTAL</th>
              <th>{{totals.total}}€</th>
            </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const API_URL = 'http://localhost:3000';

export default {
  name: 'home',
  data: () => ({
    error: '',
    items: [],
    shopping: {
      pants: 0,
      tshirt: 0,
      hat: 0,
    },
    totals: [],
  }),

  mounted() {
    fetch(API_URL)
      .then(response => response.json())
      .then((response) => {
        this.items = response;
      });
  },

  methods: {
    increment(code) {
      this.shopping[code.toLowerCase()] += 1;
    },
    decrement(code) {
      if (this.shopping[code.toLowerCase()] > 0) {
        this.shopping[code.toLowerCase()] -= 1;
      }
    },
    getCost() {
      fetch(API_URL, {
        method: 'POST',
        body: JSON.stringify(this.shopping),
        headers: {
          'content-type': 'application/json',
        },
      })
        .then(response => response.json())
        .then((result) => {
          if (!result.details) {
            this.totals = result;
          }
        });
    },
  },
};

</script>

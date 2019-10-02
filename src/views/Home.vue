<template>
  <div class="row justify-content-center">
    <div class="col-10 mt-md-5 ">
      <div class="row justify-content-center">
        <div class="col">
          <table class="table">
            <thead class="table-success">
              <tr>
                <th scope="col">Name</th>
                <th scope="col">Price</th>
                <th scope="col-1"></th>
              </tr>
            </thead>
            <tbody>
            <tr v-for="item in items" v-bind:key="item.CODE">
              <th scope=row>{{item.NAME}}</th>
              <th>$ {{item.PRICE}}</th>
              <th><button class="btn btn-success mr-1" @click="increment">
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
      <button class="btn btn-primary">Enviar</button>
      <div class="row justify-content-center">
        <div class="col-12 col-md-5">
          <h3>Items</h3>
          <div class="row">
            <p>{{shopping}}</p>
          </div>
        </div>
        <div class="col-12 col-md-5">
          <h3>Checkout</h3>
          <table class="table">
            <thead class="table-success">
              <tr>
                <th scope="col">Name</th>
                <th scope="col">Price</th>
              </tr>
            </thead>
            <tbody>
            <tr v-for="item in items" v-bind:key="item.CODE">
              <th scope=row>{{item.NAME}}</th>
              <th>$ {{item.PRICE }}</th>
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
      hat: 0,
      tshirt: 0,
    },
  }),

  mounted() {
    fetch(API_URL)
      .then(response => response.json())
      .then((response) => {
        this.items = response;
      });
  },

  methods: {
    increment(name) {
      this.shopping[name.toLowerCase()] += 1;
    },
    decrement(name) {
      const nameLower = name.toLowerCase();
      if (this.shopping[nameLower].counter > 0) {
        this.shopping[nameLower] -= 1;
      }
    },
  },
};

</script>

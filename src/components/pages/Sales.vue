<template>
  <div class="sales">
    <h1>Caisse</h1>

    <section v-for="category in categories">
      <h2>{{ category.name }}</h2>
      <div class="tiles">
        <div class="tile" v-for="product in category.products" @click="$store.dispatch('sales/addProduct', product.id)">
          <p>{{ product.name }}</p>
          <p>{{ product.price.toFixed(2) }} €</p>
        </div>
      </div>
    </section>

    <h2>Commande :</h2>
    <md-table>
      <md-table-header>
        <md-table-row>
          <md-table-head>Nom du produit</md-table-head>
          <md-table-head>Prix</md-table-head>
          <md-table-head>Action</md-table-head>
        </md-table-row>
      </md-table-header>
      <md-table-body>
        <md-table-row v-for="item in cart">
          <md-table-cell>{{ item.product.name }} (x{{ item.quantity }})</md-table-cell>
          <md-table-cell>{{ item.price.toFixed(2) }} €</md-table-cell>
          <md-table-cell><md-button class="md-icon-button" @click="$store.dispatch('sales/removeProduct', item.id)"><md-icon>remove_shopping_cart</md-icon></md-table-cell>
        </md-table-row>
        <md-table-row>
          <md-table-cell><strong>TOTAL :</strong></md-table-cell>
          <md-table-cell><strong>{{ total.toFixed(2) }} €</strong></md-table-cell>
          <md-table-cell>
            <md-button class="md-icon-button" @click="$store.dispatch('sales/emptyCart')"><md-icon>delete</md-icon></md-button>
            <md-button class="md-icon-button" @click="$store.dispatch('sales/submitCart')"><md-icon class="md-primary">check</md-icon></md-button>
          </md-table-cell>
        </md-table-row>
      </md-table-body>
    </md-table>
  </div>
</template>


<script>
import { mapGetters } from 'vuex';

export default {
  name: 'sales',
  created() {
    this.$store.dispatch('sales/getProducts');
    this.$store.dispatch('sales/getCategories');
  },
  computed: {
    ...mapGetters('sales', [
      'pending',
      'errored',
      'categories',
      'cart',
      'total',
    ]),
  },
};
</script>

<style scoped>
.tiles {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-around;
}
.tile {
  user-select: none;
  padding: 25px;
  margin-bottom: 12px;
  background-color: #3f51b5;
  color: #fff;
  text-align: center;
  cursor: pointer;
}
.tile > p:nth-child(2) {
  font-size: 24px;
}
</style>

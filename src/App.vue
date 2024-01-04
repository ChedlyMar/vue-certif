<script setup>
import { ref, computed } from 'vue'

const productList = ref([
  {
    id: 1,
    title: 'Mouse',
    unitPrice: 12,
    qte: 0,
    totalPrice: 0,
  },
  {
    id: 2,
    title: 'Key board',
    unitPrice: 22,
    qte: 0,
    totalPrice: 0,
  },
  {
    id: 3,
    title: 'WebCam',
    unitPrice: 2,
    qte: 0,
    totalPrice: 0,
  },
])

const totalPrice = computed(() => {
  return productList.value.reduce(
    (acc, cur) => acc + cur.qte * cur.unitPrice,
    0
  )
})

const addOne = (product) => {
  product.qte++
  product.totalPrice = product.unitPrice * product.qte
}

const subtractOne = (product) => {
  if (product.qte > 0) {
    product.qte--
    product.totalPrice = product.unitPrice * product.qte
  }
}
</script>

<template>
  <h1>app-certif</h1>
  <div>
    <h2>Shopping Card</h2>
    <div>
      <table>
        <tbody>
          <tr v-for="product in productList" :key="product.id">
            <td>{{ product.title }}</td>
            <td>{{ product.unitPrice }}</td>
            <td>
              <button @click="subtractOne(product)">-</button>
              {{ product.qte }}
              <button @click="addOne(product)">+</button>
            </td>
            <td>{{ product.totalPrice }}</td>
            <td>{{ (product.qte * product.unitPrice).toFixed(2) }}</td>
          </tr>
        </tbody>
      </table>
      <div>{{ totalPrice.toFixed(2) }}</div>
    </div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>

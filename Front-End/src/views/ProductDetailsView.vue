<script setup>

import { onMounted, ref } from 'vue'
import { useRoute, useRouter } from 'vue-router'
// import { products } from '@/dummyData'
import axios from 'axios'

const product = ref('')
const route = useRoute()
const router = useRouter()
let count = ref(0)

function getSingleProduct(){
  axios(
    {
      method:"GET",
      url:`http://127.0.0.1:8000/api/products/${route.params.productId}`
    }
  ).then(res => product.value = res.data)
}

function deleteProduct(){
  axios.delete(
    `http://127.0.0.1:8000/api/products/${route.params.productId}/`
  ).then(router.push('/'))
}

onMounted(()=>{
  getSingleProduct()
})

</script>

<template>
  <div class="container my-5 p-5 border " v-if="product">
    <div class="row g-5 ">

      <div class="col-lg-6 ps-5">
        <img :src="product.image" class="img-fluid rounded-start" :alt="product.name" style="width: 100%; height: 650px;"/>
      </div>

      <div class="col-md-6">
        <div class="card-body">

          <div class="d-flex">
            <h5 class="card-title mt-5 mb-2 me-5 pe-5">{{ product.name }} Details</h5>
            <RouterLink :to="`/${product.id}/update`" class="btn btn-outline-info px-3 mt-5 ms-5">Update the Product</RouterLink>
          </div>


          <p class="card-text">
            <small class="text-muted">brand: {{ product.brand?.name || "no brand" }}</small>
          </p>
          <p class="card-text">
            <small class="text-muted">Price: ${{ product.price }}</small>
          </p>
          <p class="card-text">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Expedita, eligendi soluta dolor libero unde
            blanditiis ducimus aliquid maiores magni sint fuga, velit voluptas minus deleniti architecto recusandae!
            Veniam, velit minus!
          </p>

          <div class="flex my-5 py-5">
            <form>
              <button @click="count++" type="button" class="btn btn-success px-3">+</button>
              <button type="button" class="btn btn-outline-secondary px-3 mx-2">{{ count }}</button>
              <button @click="count--" type="button" class="btn btn-success px-3 me-5">-</button>
              
              <button class="btn btn-success px-3">Add to Cart</button>
            </form>
          </div>
          <div class="d-flex">
            <RouterLink to="/" class="btn btn-secondary px-5 mt-5 me-5">Back to Product Lists</RouterLink>
            <button @click="deleteProduct()" class="btn btn-danger px-5 mt-5 ms-5" >Delete Product</button>
          </div>


        </div>
      </div>

    </div>
  </div>

  <div v-else class="text-center mt-5">
    <h3>Product not found.</h3>
  </div>
</template>

<script setup>
let urlProduct = "http://localhost/vuejs-server/api.php/products";
let urlSearchProduct = "http://localhost/vuejs-server/api.php/search-product";

let urlCategory = "http://localhost/vuejs-server/api.php/categories";
let urlImage = "http://localhost/vuejs-server/uploads/product/";
import anh1 from "@/assets/image/anh1.jpeg";
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { RouterLink } from "vue-router";
//product
const listProduct = ref([]);
const callAPI = async () => {
   try {
      let response = await axios.get(urlProduct);
      listProduct.value = response.data

   } catch (error) {
      console.log(`Loi call API ${error}`);

   }

};
const convertPrice = (number) => {
   return number.toLocaleString("vi-VN");
}

//category
let listCategory = ref([])
const callAPICategory = async () => {
   try {
      let response = await axios.get(urlCategory)
      listCategory.value = response.data
   } catch (error) {
      console.log(`Loi API ${error}`);

   }
}
onMounted(() => {
   callAPI();
   callAPICategory();
})

const productName = ref("")
const handleSearch = async () => {
   if (productName.value != "") {
      try {
         let data = {
            product_name: productName.value
         }
         let response = await axios.post(urlSearchProduct, data)
         if (response.data.status == true) {
            productName.value = ""
            listProduct.value = response.data.products
         }
      } catch (error) {
         console.log(error);
      }
   }else {
      callAPI()
   }
}
</script>
<template>
   <div class="row main-website">
      <div class="col-8">
         <div class="d-flex flex-wrap">
            <!-- san pham -->
            <div class="card card-product" style="" v-for="(product, index) in listProduct" :key="index">
               <img v-if="product.image != null" :src="urlImage + product.image" class="card-img-top"
                  :alt="product.name">
               <img v-else :src="anh1" class="card-img-top" :alt="product.name">
               <div class="card-body">
                  <h5 class="card-title">{{ product.name }}</h5>
                  <p class="text-price">{{ convertPrice(Number(product.price)) }} vnd</p>
                  <p class="card-text">{{ product.description }}</p>
                  <RouterLink :to="`san-pham-chi-tiet/${product.id}`" class="btn btn-primary">Xem chi tiet</RouterLink>
               </div>
            </div>


         </div>
      </div>
      <div class="col-3 offset-1 p-4">
         <div class="card mb-5">
            <div class="card-body d-flex">
               <input type="text" class="form-control me-2" v-model="productName">
               <button class="btn btn-success" @click="handleSearch">Search</button>
            </div>
         </div>
         <div class="card">
            <div class="card-header">
               Danh sach danh muc'
               <div class="list-group">
                  <RouterLink v-for="(category, index) in listCategory" :key="index" :to="`/san-pham/${category.id}`"
                     href="#" class="list-group-item list-group-item-action ">
                     {{ category.name }}
                  </RouterLink>

               </div>
            </div>
         </div>
      </div>
   </div>
</template>
<style lang="scss">
.card-product {
   width: 30%;
   margin: 10px;
   margin-top: 20px;
}

.text-price {
   font-weight: bold;
   color: rgb(157, 11, 11);
   margin: 0;
   padding: 0;
}
</style>
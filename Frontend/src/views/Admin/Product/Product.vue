<script setup>
import { ref, onMounted } from "vue";
import { RouterLink } from "vue-router";
import axios from "axios";
const productList = ref([]);
let urlAPI = "http://localhost/vuejs-server/api.php/products";
let urlImage = "http://localhost/vuejs-server/uploads/product/";
onMounted(() => {
   callAPI();
});

const callAPI = async () => {
   try {
      let response = await axios.get(urlAPI);
      productList.value = response.data;
     
   } catch (error) {
      console.log("CallAPI loi");

   }
}

const handleClick = async (id) => {
   let check = confirm("Ban co muon xoa khong?");
   if (check) {
      try {
         let response = await axios.delete(urlAPI + "/" + id);
         if (response.status === 200) {
            alert("Xoa thanh cong");
            callAPI();
         }
      } catch (error) {
         console.log("CallAPI loi");
      }
   }
}
</script>
<template>
   <div class="p-4" style="min-height:800px">
      <h1>Quan li san pham</h1>
      <RouterLink to="/admin/add-product" class="btn btn-primary">Them moi</RouterLink>
      <table class="table">
         <thead>
            <tr>
               <th>STT</th>
               <th>Name</th>
               <th>Category</th>
               <th>Description</th>
               <th>Price</th>
               <th>Image</th>
               <th>Action</th>
            </tr>
         </thead>
         <tbody>
            <tr v-for="(product, index) in productList">
               <td>{{ index + 1 }}</td>
               <td>{{ product.name }}</td>
               <td>{{ product.categoryName }}</td>
               <td>{{ product.description }}</td>
               <td>{{ product.price }}</td>
               <td><img v-if="product.image != null" :src="urlImage + product.image" alt="" width="50px">
                  <span v-else class="badge text-bg-warning">Khong co anh</span>
               </td>
               <td>
                  <RouterLink :to="`update-product/${product.id}`" class="btn btn-warning">Sua</RouterLink>
                  <button @click="handleClick(product.id)" class="btn btn-danger">Xoa</button>
               </td>
            </tr>
         </tbody>
      </table>
   </div>
</template>
<script setup>
import { ref, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import axios from 'axios';
const route = useRoute();
const router = useRouter();
const id = ref(route.params.id);
import anh1 from "@/assets/image/anh1.jpeg";
let urlProductDetail = "http://localhost/vuejs-server/api.php/product-detail";
let urlAddCart = "http://localhost/vuejs-server/api.php/cart";
let urlImage = "http://localhost/vuejs-server/uploads/product/";
const product = ref({})
const callAPI = async () => {
    try {
        let url = urlProductDetail + "?product_id=" + route.params.id;
        let response = await axios.get(url);
        if (response.data.status) {
            product.value = response.data.products;
        }
    } catch (error) {
        console.log(error);

    }
}
const convertPrice = (number) => {
    return number.toLocaleString("vi-VN");
}
const quantity = ref(1);
onMounted(() => {
    callAPI();
})
const addCart = async () => {
    if (quantity.value >= 1) {
        // let link=`/giohang/${product.value.id}/${quantity.value}`;
        try {
            let url = urlAddCart + 
            "/" + 
            "?product_id=" + 
            product.value.id + 
            "&quantity=" + 
            quantity.value + 
            "&user_id=" + 
            JSON.parse(localStorage.getItem("userLogin")).id;
            console.log(url);
            
            let response = await axios.get(url);
            console.log(response.data);
            
            alert(response.data.message);
            router.push("/giohang");
        } catch (error) {
            console.log(error);

        }
        // router.push("/giohang");
    }
}
</script>
<template>
    <div class="row main-website justify-content-center mt-5">
        <div class="col-8">
            <h4 class="mb-5">Chi tiet san pham</h4>
            <div class="container">
                <div class="row">
                    <div class="col-3">
                        <img v-if="product.image != null" :src="urlImage + product.image" alt="" class="img-product">
                        <img v-else :src="anh1" alt="" class="img-product">
                    </div>
                    <div class="col-9">
                        <p><span class="fw-bold">Ten san pham:</span> {{ product.name }}</p>
                        <p><span class="fw-bold">Danh muc:</span> {{ product.categoryName }}</p>
                        <p><span class="fw-bold">Gia san pham:</span> {{ convertPrice(Number(product.price)) }}</p>
                        <p><span class="fw-bold">Mo ta:</span> {{ product.description }} vnd</p>
                        <p><span class="fw-bold">So luong:</span>
                            <input type="text" class="form-control" v-model="quantity">
                        </p>
                        <button class="btn btn-success" @click="addCart">Them vao gio hang</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style lang="scss">
.img-product {
    width: 100%;
    object-fit: cover;
}
</style>
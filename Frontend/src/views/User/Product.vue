<script setup>
let urlProduct = "http://localhost/vuejs-server/api.php/products";
let urlCategory = "http://localhost/vuejs-server/api.php/categories";
let urlImage = "http://localhost/vuejs-server/uploads/product/";
import anh1 from "@/assets/image/anh1.jpeg";
import { ref, onMounted, watch } from 'vue';
import axios from 'axios';
import { RouterLink } from "vue-router";
import { useRoute } from "vue-router";
//product
const route = useRoute();
const listProduct = ref([]);
const callAPI = async () => {
    try {
        let url = urlProduct + '?idCategory=' + route.params.idCategory;
        let response = await axios.get(url);
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

//watch
watch(() => route.params.idCategory, (newIdCategory) => {
    callAPI();
})
</script>
<template>
    <div class="row main-website">
        <div class="col-8">
            <div class="d-flex flex-wrap" v-if="listProduct.length > 0">
                <!-- san pham -->

                <div class="card card-product" style="" v-for="(product, index) in listProduct" :key="index">
                    <img v-if="product.image != null" :src="urlImage + product.image" class="card-img-top"
                        :alt="product.name">
                    <img v-else :src="anh1" class="card-img-top" :alt="product.name">
                    <div class="card-body">
                        <h5 class="card-title">{{ product.name }}</h5>
                        <p class="text-price">{{ convertPrice(Number(product.price)) }} vnd</p>
                        <p class="card-text">{{ product.description }}</p>
                        <a href="#" class="btn btn-primary">Xem chi tiet</a>
                    </div>
                </div>


            </div>
            <div v-else class="alert alert-danger mt-4" role="alert">
                    khong co san pham nao
                </div>
        </div>
        <div class="col-3 offset-1 p-4">

            <div class="card">
                <div class="card-header">
                    Danh sach danh muc'
                    <div class="list-group">
                        <RouterLink v-for="(category, index) in listCategory" :key="index"
                            :to="`/san-pham/${category.id}`" href="#" class="list-group-item list-group-item-action " :class="{
                                active:category.id == route.params.idCategory
                            }">
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
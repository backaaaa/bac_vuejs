<script setup>
import { useRoute, RouterLink } from 'vue-router';
import { ref, onMounted } from 'vue';
import axios from 'axios';

const route = useRoute()
const orderDetail = ref([])

let urlDetail = "http://localhost/vuejs-server/api.php/order_detail"

const callAPI = async () => {
    try {
        let response = await axios.get(urlDetail + "?order_id=" + route.params.order_id)
        if(response.status == 200) {
            orderDetail.value = response.data
        }
    } catch (error) {
        console.log(error);
    }
}

onMounted(() => {
    callAPI()
})

const convertPrice = (number) => {
    return number.toLocaleString("vi-VN");
}


</script>

<template>
     <div class="row main-website justify-content-center mt-5">
        <div class="col-10">
            <h4>Danh sách sản phẩm</h4>
            <table class="table">
                <thead>
                    <tr>
                        <th>STT</th>
                        <th>Tên sản phẩm</th>
                        <th>Giá sản phẩm</th>
                        <th>Số lượng</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item, index) in orderDetail" :key="index">
                        <td>{{ index + 1 }}</td>
                        <td>
                            <RouterLink target="_blank" :to="`/san-pham-chi-tiet/${item.product_id}`">{{ item.product_name }}</RouterLink>
                        </td>
                        <td>{{ convertPrice(item.price) }} vnđ</td>
                        <td>{{ item.quantity }}</td>
                    </tr>
                </tbody>
            </table>
        </div>
     </div>
</template>
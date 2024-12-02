<script setup>
import axios from 'axios';
import { reactive, ref, onMounted, computed } from 'vue';
import { RouterLink } from 'vue-router';
let url = "http://localhost/vuejs-server/api.php/cart-detail"
let urlCartUpdate = "http://localhost/vuejs-server/api.php/cart-update"

const cart = ref({})
const cartDetail = ref([])

const callAPI = async () => {
    let user_id = JSON.parse(localStorage.getItem('userLogin')).id
    try {
        let response = await axios.get(url + "/?user_id=" + user_id)
        console.log(response.data);

        if (response.data.status) {
            cart.value = response.data.cart
            cartDetail.value = response.data.cart_details
        }
        console.log(cartDetail);

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

const totalPrice = computed(() => {
    let total = 0
    cartDetail.value .forEach(item => {
        total = total + (item.price * item.quantity)
    })
    return total
})

const handleUpdateCart = async (action, product_id) => {
    try {
        let link = urlCartUpdate + "?cart_id=" +cart.value.id + "&action=" + action + "&product_id=" + product_id
        let response = await axios.get(link)
        if(response.status == 200) {
            alert("Cập nhật giỏ hàng thành công!")
            callAPI()
        }
    } catch (error) {
        console.log(error);
    }
}
</script>

<template>
    <div class="row main-website justify-content-center mt-5">
        <div class="col-8">
            <h1>Giỏ hàng</h1>
            <table class="table">
                <thead>
                    <tr>
                        <th>STT</th>
                        <th>Tên sản phẩm</th>
                        <th>Giá sản phẩm</th>
                        <th>Số lượng</th>
                        <th>Thành tiền</th>
                        <th>Action</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-if="cartDetail.length == 0">
                        <td colspan="6">
                            Không có sản phẩm trong giỏ hàng
                            <RouterLink to="/">Tiếp tục mua hàng</RouterLink>
                        </td>
                    </tr>
                    <tr v-else v-for="(item, index) in cartDetail" :key="idnex">
                        <td>{{ index + 1 }}</td>
                        <td>{{ item.product_name }}</td>
                        <td>
                            {{ convertPrice(Number(item.price)) }} vnđ
                        </td>
                        <td>
                            <div class="d-flex">
                                <button class="btn btn-sm btn-info" @click="handleUpdateCart('decrease', item.product_id)">-</button>
                                <span> {{ item.quantity }}</span>
                                <button class="btn btn-sm btn-info" @click="handleUpdateCart('increase', item.product_id)">+</button>
                            </div>
                        </td>
                        <td>{{ convertPrice(Number(item.price) * Number(item.quantity)) }} vnđ</td>
                        <td>
                            <button class="btn btn-danger" @click="handleUpdateCart('delete', item.product_id)">Xóa</button>
                        </td>
                    </tr>
                    <tr v-if="cartDetail.length != 0">
                        <td colspan="4"></td>
                         <td>Tổng tiền: {{ convertPrice(totalPrice) }} vnđ</td>
                         <td>
                            <RouterLink to="/thanh-toan" class="btn btn-warning">Thanh toán</RouterLink>
                         </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>
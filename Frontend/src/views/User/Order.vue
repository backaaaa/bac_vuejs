<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { RouterLink } from 'vue-router';
let urlOrder = "http://localhost/vuejs-server/api.php/show_order"
let urlChangeStatus = "http://localhost/vuejs-server/api.php/change-status"

const order = ref([])

const callAPI = async () => {
    try {
        let user_id = JSON.parse(localStorage.getItem('userLogin')).id
        let response = await axios.get(urlOrder + "?user_id=" + user_id)
        if (response.status == 200) {
            order.value = response.data
            console.log(order.value);
        }
    } catch (error) {
        console.log(error);
    }
}

onMounted(() => {
    callAPI()
})

function convertToDateFormat(input) {
    const dateParts = input.split("-")
    if (dateParts.length !== 3) return "Invalid date format"

    const year = dateParts[0]
    const month = dateParts[1]
    const day = dateParts[2]

    return `${day}/${month}/${year}`
}

const convertPrice = (number) => {
    return number.toLocaleString("vi-VN");
}

const handleCancel = async (order_id) => {
    const check = confirm("Bạn có muốn hủy đơn hàng không?")
    if (check) {
        try {
            let response = await axios.get(urlChangeStatus + "?order_id=" + order_id + "&status_code=3")
            if(response.status == 200) {
                alert("Hủy đơn hàng thành công!")
                callAPI()
            }
        } catch (error) {
            console.log(error);
        }
    }
}   
</script>

<template>
    <div class="row main-website justify-content-center mt-5">
        <div class="col-10">
            <table class="table">
                <thead>
                    <tr>
                        <th>STT</th>
                        <th>Ngày đặt hàng</th>
                        <th>Trạng thái</th>
                        <th>Tổng tiền</th>
                        <th>Action</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item, index) in order" :key="index">
                        <td>{{ index + 1 }}</td>
                        <td>{{ convertToDateFormat(item.order_date) }}</td>
                        <td>
                            <span v-if="item.status == 1" class="badge text-bg-warning">Chưa xác nhận</span>
                            <span v-else-if="item.status == 2" class="badge text-bg-info">Đã xác nhận xác nhận</span>
                            <span v-else-if="item.status == 3" class="badge text-bg-danger">Đã hủy</span>
                            <span v-else-if="item.status == 4" class="badge text-bg-success">Đang giao hàng</span>
                            <span v-else class="badge text-bg-success">Đã giao hàng</span>
                        </td>
                        <td>{{ convertPrice(item.total_price) }} vnđ</td>
                        <td>
                            <RouterLink :to="`/don-hang-chi-tiet/${item.order_id}`" class="btn btn-info">Xem chi tiết</RouterLink>
                            <button v-if="item.status == 1" class="btn btn-danger"
                                @click="handleCancel(item.order_id)">Hủy đơn</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>
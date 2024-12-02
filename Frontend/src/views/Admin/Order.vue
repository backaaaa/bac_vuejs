<script setup>
import axios from 'axios';
import { RouterLink } from 'vue-router';
import { ref, onMounted, reactive } from 'vue';
let urlOrderAll = "http://localhost/vuejs-server/api.php/show_order_admin"
let urlChangeStatus = "http://localhost/vuejs-server/api.php/change-status"

let orderList = ref([])
const statusOrderMap = reactive([])

const callAPI = async () => {
   try {
      let response = await axios.get(urlOrderAll)
      if (response.status == 200) {
         orderList.value = response.data
         orderList.value.forEach(item => {
            statusOrderMap[item.order_id] = item.status
         })
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

const handleStatusChange = async (orderId, newStatus) => {
   try {
      let response = await axios.get(urlChangeStatus + "?order_id=" + orderId + "&status_code=" + newStatus)
      if(response.status == 200) {
         alert("Cập nhật thành công!")
         callAPI()
      }
   } catch (error) {
      console.log(error);
   }
}

</script>
<template>
   <div class="p-4" style="min-height:800px">
      <table class="table">
         <thead>
            <tr>
               <th>STT</th>
               <th>Tên khách hàng</th>
               <th>Email</th>
               <th>Số điện thoại</th>
               <th>Ngày đặt</th>
               <th>Tổng tiền</th>
               <th>Trạng thái</th>
            </tr>
         </thead>
         <tbody>
            <tr v-for="(item, index ) in orderList" :key="index">
               <td>{{ index + 1 }}</td>
               <td>{{ item.customer_name }}</td>
               <td>{{ item.email }}</td>
               <td>{{ item.phone }}</td>
               <td>{{ convertToDateFormat(item.order_date) }}</td>
               <td>{{ convertPrice(item.total_price) }} vnđ</td>
               <td>
                  <select class="form-control" v-model="statusOrderMap[item.order_id]"
                  @change="handleStatusChange(item.order_id, statusOrderMap[item.order_id])">
                     <option value="1">Chưa xác nhận</option>
                     <option value="2">Đã xác nhận</option>
                     <option value="3">Đã hủy</option>
                     <option value="4">Đang giao hàng</option>
                     <option value="5">Đã giao hàng</option>
                  </select>
               </td>
               <td>
                  <RouterLink :to="`/order-detail/${item.order_id}`" class="btn btn-sm btn-info">Xem chi tiết</RouterLink>
               </td>
            </tr>
         </tbody>
      </table>
   </div>
</template>
<script setup>
import { RouterLink } from 'vue-router';
import { reactive, ref } from 'vue';
import { useRouter } from 'vue-router';
const router=useRouter();
let userLogin=reactive({});
if(localStorage.getItem("userLogin") != null){
   userLogin =JSON.parse(localStorage.getItem("userLogin"));
}
</script>
<template>
    <div class="col-3 bg-info position-fixed top-0 start-0" style="height: 100vh;">
        <h3 class=" mt-4 text-center text-light">Admin: {{ userLogin.name }}</h3>
        <div class="list-group mt-4">
            <RouterLink to="/admin" class="list-group-item ">
                Dashboard
            </RouterLink>
            <RouterLink to="/admin/acount" class="list-group-item">
                Quản lý tai khoan
            </RouterLink>
            <RouterLink to="/admin/category" class="list-group-item ">
                Quan li danh muc
            </RouterLink>
            <RouterLink to="/admin/product" class="list-group-item ">
                Quan li san pham
            </RouterLink>
            <RouterLink to="/admin/order" class="list-group-item ">
                Quan li don hang
            </RouterLink>
        </div>
    </div>

</template>
<style scoped lang="css"></style>
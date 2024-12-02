<script setup>
import { useRouter } from 'vue-router';
const props=defineProps({
    titleHeader: {
        type:String,
        required:true,
        default:"Admin",
    },
});
const router=useRouter();
const handleLogout=()=>{
    localStorage.removeItem("userLogin");
    router.push("/login");
}
</script>
<template>
    <div class="bg-primary ps-4 pt-4" style="height: 100px; width: 100%;">
       <div class="d-flex justify-content-between ps-2 pe-3">
        <h3 class="text-light"><Table>{{ props.titleHeader }}</Table></h3>
        <button @click="handleLogout" class="btn btn-info">Dang xuat</button>
       </div>
    </div>
</template>
<style scoped lang="css"></style>
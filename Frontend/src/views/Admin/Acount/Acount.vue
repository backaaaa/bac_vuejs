<script setup>
let url = "http://localhost/vuejs-server/api.php/users"
import { onMounted, ref } from 'vue';
import axios from 'axios';
import { RouterLink } from 'vue-router';
const listAcount = ref([])
onMounted(async () => {
    let response = await axios.get(url);
    listAcount.value = response.data;


});
const handleDelete=async(idAcount)=>{
    const check=confirm("Ban co muon xoa khong?")
    if(check){
        let response=await axios.delete(url+"/"+idAcount)
       if(response){
        alert(response.data.message)
        let response2 = await axios.get(url);
        listAcount.value = response2.data;
       }

       
        
    }
}
</script>
<template>
    <div class="p-4" style="min-height: 800px;">
        <h1>Quan li tai khoan</h1>
        <RouterLink class="btn btn-primary" to="/admin/add-acount">Add Acount</RouterLink>
        <table class="table">
            <thead>
                <tr>
                    <th>STT</th>
                    <th>Name</th>
                    <th>Email</th>
                    <th>Password</th>
                    <th>Role</th>
                    <th>Note</th>
                    <th>Created_at</th>
                    <th>Updated_at</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(users, index) in listAcount" :key="index">
                    <td>{{ index + 1 }}</td>
                    <td>{{ users.name }}</td>
                    <td>{{ users.email }}</td>
                    <td>{{ users.password }}</td>
                    <td>{{ users.role }}</td>
                    <td>{{ users.note }}</td>
                    <td>{{ users.created_at }}</td>
                    <td>{{ users.updated_at }}</td>

                    <td>
                        <RouterLink :to="`/admin/update-acount/${users.id}`" class="btn btn-warning">Sua</RouterLink>
                        <button @click="handleDelete(users.id)" class="btn btn-danger">Xoa</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
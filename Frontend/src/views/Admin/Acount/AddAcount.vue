<script setup>
import { ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
const router = useRouter();
let url = "http://localhost/vuejs-server/api.php/users"
const name = ref("");
const email = ref("");
const password = ref("");
const role = ref("");
const note = ref("");
const handleSubmit = async () => {
    checkValidate();
    if (nameError.value == "" && emailError.value==""&&passwordError.value=="" && roleError.value == ""&& noteError.value== "") {
        let formData = new FormData();
        formData.append("name", name.value);
        formData.append("email", email.value);
        formData.append("password", password.value);
        formData.append("role", role.value);
        formData.append("note", note.value);


        let response = await axios.post(url, formData,
            {
                headers: {
                    "Content-Type": "multipart/form-data",
                },
            }
        );
        if (response) {
            alert(response.data.message);
            router.push('/admin/acount');
        }
    }

};
const nameError = ref("");
const emailError = ref("");
const passwordError=ref("");
const roleError=ref("");
const noteError=ref("");
const checkValidate = () => {
 
    if (name.value == "") {
        nameError.value = "khong duoc de trong ten tai khoan";
        
    }else{
        nameError.value ="";
    }
    if (email.value == "") {
        emailError.value = "Khong duoc de trong email tai khoan ";
        
    }else{
        emailError.value="";
    }
    if (password.value == "") {
        passwordError.value = "Khong duoc de trong password trong tai khoan";
        
    }else{
        passwordError.value="";
    }
    if (role.value == "") {
        roleError.value = "Khong duoc de trong role trong tai khoan";
        
    }else{
        roleError.value="";
    }
    if (note.value == "") {
        noteError.value = "Khong duoc de trong note trong tai khoan";
        
    }else{
        noteError.value="";
    }

}
</script>
<template>
    <div class="p-4" style="min-height: 800px;">
        <h1>trang ten danh muc</h1>
        <form @submit.prevent="handleSubmit">
            <div class="mb-3">
                <label for="name">Name</label>
                <input type="text" id="name" placeholder="Ten danh muc" v-model="name" class="form-control">
                <span v-if="nameError!=''" class="text-danger">{{ nameError }}</span>
            </div>
            <div class="mb-3">
                <label for="description">Email</label>
                <input type="text" id="email" placeholder="Mo ta danh muc" v-model="email"
                    class="form-control">
                <span v-if="emailError !=''" class="text-danger">{{ emailError }}</span>
            </div>
            <div class="mb-3">
                <label for="description">Password</label>
                <input type="text" id="password" placeholder="Mo ta danh muc" v-model="password"
                    class="form-control">
                <span v-if="passwordError !=''" class="text-danger">{{ passwordError }}</span>
            </div>
            <div class="mb-3">
                <label for="description">Role</label>
                <input type="text" id="role" placeholder="Mo ta danh muc" v-model="role"
                    class="form-control">
                <span v-if="roleError !=''" class="text-danger">{{ roleError }}</span>
            </div>
            <div class="mb-3">
                <label for="description">Note</label>
                <input type="text" id="note" placeholder="Mo ta danh muc" v-model="note"
                    class="form-control">
                <span v-if="noteError !=''" class="text-danger">{{ noteError }}</span>
            </div>
            <button class="btn btn-success">Them moi</button>
        </form>
    </div>
</template>
<script setup>
import "bootstrap/dist/css/bootstrap.min.css";
import "bootstrap/dist/js/bootstrap.bundle.min.js"
import { RouterLink,useRouter } from "vue-router";
import { ref } from "vue";
let url = "http://localhost/vuejs-server/api.php/register"
import axios from "axios";
const router=useRouter();
const name = ref("")
const email = ref("")
const password = ref("")

const nameErorr = ref("")
const emailErorr = ref("")
const passwordErorr = ref("")

const handleSubmit = async () => {
    try {
        let data = {
        name: name.value,
        email: email.value,
        password: password.value
    }
    let response=await axios.post(url,data)
    if(response.data.message){
        alert("Dang ki thanh cong")
        router.push('/login')
    }  
    } catch (error) {
        console.log(`Loi callAPI ${error}`);
        
    }
   
}

const handleChange = (field) => {
    switch (field) {
        case 'name': {
            nameErorr.value = name.value == "" ? "Khong duoc de trong name" : "";
            break;
        }
        case 'email': {
            emailErorr.value = email.value == "" ? "Khong duoc de trong email" : "";
            break;
        }
        case 'password': {
            passwordErorr.value = password.value == "" ? "Khong duoc de trong password" : "";
            break;
        }
    }
}
</script>
<template>

    <form @submit.prevent="handleSubmit">
        <h1>Dang ky</h1>
        <div class="mb-4">
            <label for="name">Name</label>
            <input type="text" id="name" placeholder="Name" v-model="name" @keyup="handleChange('name')"
                class="form-control">
            <span v-if="nameErorr != ''" class="text-danger">{{ nameErorr }}</span>
        </div>
        <div class="mb-4">
            <label for="email">Email</label>
            <input type="text" id="email" placeholder="Email" v-model="email" @keyup="handleChange('email')"
                class="form-control">
            <span v-if="emailErorr != ''" class="text-danger">{{ emailErorr }}</span>
        </div>
        <div class="mb-4">
            <label for="password">Password</label>
            <input type="text" id="password" placeholder="Password" v-model="password" @keyup="handleChange('password')"
                class="form-control">
            <span v-if="passwordErorr != ''" class="text-danger">{{ passwordErorr }}</span>
        </div>
        <span class="register">
            Da co tai khoan:
            <RouterLink to="/login">Dang nhap</RouterLink>
        </span>
        <button class="btn btn-success">Dang ky</button>
    </form>
</template>
<style lang="scss">
form {
    width: 60%;
    margin: auto;
    margin-top: 50px;

    .register {
        display: flex;
        margin-top: 30px;
    }
}
</style>
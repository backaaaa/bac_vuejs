<script setup>
import "bootstrap/dist/css/bootstrap.min.css";
import "bootstrap/dist/js/bootstrap.bundle.min.js"
import { useRouter } from "vue-router";
import { ref } from "vue";
import axios from "axios";
import { RouterLink } from "vue-router";

const router = useRouter();
let url = "http://localhost/vuejs-server/api.php/login";
const email = ref("")
const password = ref("")

const emailErorr = ref("")
const passwordErorr = ref("")

const handleSubmit = async () => {
    try {
        let data = {
            email: email.value,
            password: password.value

        };
        let response = await axios.post(url, data);
        let message = response.data;
       
        
        
        if (message.status == true) {
            localStorage.removeItem("userLogin");
            let userLogin={
                id: message.data.id,
                name:message.data.name,
                email:message.data.name,
                role:message.data.role
            };
            localStorage.setItem("userLogin",JSON.stringify(userLogin));
          
            if (message.data.role == "user") {
                router.push("/");
            } else if (message.data.role == "admin") {
                router.push("/admin");
            }
        } else {
            alert("Tai khoan hoac mat khau khong dung");
        }
    } catch (error) {
        console.log(`Loi call API ${error}`);

    }


}

const handleValidate = (field) => {
    switch (field) {
        case 'email': {
            emailErorr.value = email.value == "" ? "khong duoc de trong email" : "";
            break;
        }
        case 'password': {
            passwordErorr.value = password.value == "" ? "khong duoc de trong password" : "";
            break;
        }
    }
}
</script>
<template >
    <form @submit.prevent="handleSubmit">
        <h1>Dang nhap</h1>
        <div class="mb-4">
            <label for="email">Email</label>
            <input type="text" id="email" placeholder="Email" class="form-control" v-model="email"
                @keyup="handleValidate('email')">
            <span v-if="emailErorr != ''" class="text-danger">{{ emailErorr }}</span>
        </div>
        <div class="mb-4">
            <label for="password">Password</label>
            <input type="password"  id="password" placeholder="Password" class="form-control"
                v-model="password" @keyup="handleValidate('password')">
            <span v-if="passwordErorr != ''" class="text-danger">{{ passwordErorr }}</span>
        </div>
        <span class="register">
            Chua co tai khoan:
            <RouterLink to="/register">Dang ky</RouterLink>
        </span>
        <button class="btn btn-primary">Dang nhap</button>
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
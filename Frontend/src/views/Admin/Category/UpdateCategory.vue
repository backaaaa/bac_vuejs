<script setup>
let url = "http://localhost/vuejs-server/api.php/categories";
import { useRoute, useRouter } from 'vue-router';
import { ref, onMounted } from 'vue';
import axios from 'axios';
const route = useRoute();
const router = useRouter();
const name = ref("");
const description = ref("");
onMounted(async () => {
    let response = await axios.get(url + "/" + route.params.id);
    if (response) {
        name.value = response.data.name;
        description.value = response.data.description;
    }


});
const handleSubmit = async () => {
    checkValidate();
    if (nameError.value == "" && descriptionError.value == "") {
        let formData = new FormData();
        formData.append('_method', 'PUT');
        formData.append('name', name.value);
        formData.append('description', description.value);
        let response = await axios.post(
            url + "/" + route.params.id,
            formData,
            {
                headers: {
                    "Content-Type": "multipart/form-data",
                },
            }
        )
        if (response) {
            alert(response.data.message);
            router.push("/admin/category");
        }

    };
};
const nameError = ref("");
const descriptionError = ref("");

const checkValidate = () => {

    if (name.value == "") {
        nameError.value = "khong duoc de trong ten danh muc";

    } else {
        nameError.value = "";
    }
    if (description.value == "") {
        descriptionError.value = "Khong duoc de trong mo ta danh muc";

    } else {
        descriptionError.value = "";
    }

}
</script>
<template>
    <div class="p-4" style="min-height: 800px;">
        <h1>Trang chinh sua</h1>
        <form @submit.prevent="handleSubmit">
            <div class="mb-3">
                <label for="name">Name</label>
                <input type="text" id="name" placeholder="Ten danh muc" v-model="name" class="form-control">
                <span v-if="nameError != ''" class="text-danger">{{ nameError }}</span>
            </div>
            <div class="mb-3">
                <label for="description">Description</label>
                <input type="text" id="description" placeholder="Mo ta danh muc" v-model="description"
                    class="form-control">
                <span v-if="descriptionError != ''" class="text-danger">{{ descriptionError }}</span>
            </div>
            <button class="btn btn-warning">Chinh sua</button>
        </form>
    </div>
</template>
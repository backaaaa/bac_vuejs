<script setup>
import { ref,onMounted } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';
let url = "http://localhost/vuejs-server/api.php/products";
let urlAPICategory="http://localhost/vuejs-server/api.php/categories";
const router = useRouter();
const name = ref("")
const description = ref("")
const price = ref("")
const category_id = ref("")
const image = ref(null)
const nameError = ref("")
const descriptionError = ref("")
const priceError = ref("")
const categoryIdError = ref("")
const imageError = ref("")

let listCategory=ref([]);
const callAPICate=async()=>{
    const response=await axios.get(urlAPICategory);
    listCategory.value=response.data;
}
onMounted(()=>{
    callAPICate();
})
const handleChange = (event) => {
    const file = event.target.files[0];
    const maxFileSize=10 * 1024 * 1024//khong qua 10mb
    if(file.size>maxFileSize){
        imageError.value="kich thuoc file vuot qua dung luong 10mb"
        image.value=null
    }
    image.value = file;
};
const handleSubmit = async () => {
    checkEmpty("name");
    checkEmpty("description");
    checkEmpty("price");
    checkEmpty("category_id");
    if (nameError.value == "" &&
        descriptionError.value == "" &&
        priceError.value == "") {
        try {
            let formData = new FormData();
            formData.append("name", name.value);
            formData.append("description", description.value);
            formData.append("price", price.value);
            formData.append("image", image.value);
            formData.append("category_id", category_id.value);


            let response = await axios.post(url, formData,
                {
                    headers: {
                        "Content-Type": "multipart/form-data",
                    },
                }
            );
            if (response.status === 200) {
                alert("Them san pham thanh cong");
                router.push("/admin/product");
            }
        } catch (error) {
            console.log("CallAPI loi");
        }
    };
};

const checkEmpty = (field) => {
    switch (field) {
        case 'name': {
            if (name.value == "") {
                nameError.value = "Khong duoc de trong ten san pham"
            }
            else {
                nameError.value = ""
            }

            break;
        }
        case 'description': {
            if (description.value == "") {
                descriptionError.value = "Khong duoc de trong description"
            }
            else {
                descriptionError.value = ""
            }

            break;
        }
        case 'price': {
            if (price.value == "") {
                priceError.value = "Khong duoc de trong price"
            }
            else {
                priceError.value = ""
            }

            break;
        }
        case 'category_id': {
            if (category_id.value == "") {
                categoryIdError.value = "Vui long chon category_id"
            }
            else {
                categoryIdError.value = ""
            }

            break;
        }
        default: {
            break;
        }
    }
}



</script>
<template>
    <div class="p-4" style="min-height:800px">
        <h1>Them san pham</h1>
        <form @submit.prevent="handleSubmit">
            <div class="mb-3">
                <label for="name">Name</label>
                <input type="text" id="name" placeholder="Name" class="form-control" v-model="name"
                    @keyup="checkEmpty('name')">
                <p v-if="nameError != ''" class="text-danger">{{ nameError }}</p>
            </div>
            <div class="mb-3">
                <label for="name">Category</label>
                <select id="category" class="form-control" v-model="category_id">
                    <option value="" hidden>---Chon danh muc</option>
                    <option :value="category.id" v-for="(category, index) in listCategory" :key="index">{{ category.name }}</option>
                </select>
                <p v-if="categoryIdError != ''" class="text-danger">{{ categoryIdError }}</p>
            </div>
            <div class="mb-3">
                <label for="description">Description</label>
                <input type="text" id="description" placeholder="Description" class="form-control" v-model="description"
                    @keyup="checkEmpty('description')">
                <p v-if="descriptionError != ''" class="text-danger">{{ descriptionError }}</p>
            </div>
            <div class="mb-3">
                <label for="price">Price</label>
                <input type="text" id="price" placeholder="Price" class="form-control" v-model="price"
                    @keyup="checkEmpty('price')">
                <p v-if="priceError != ''" class="text-danger">{{ priceError }}</p>
            </div>
            <div class="mb-3">
                <label for="image">Image</label>
                <input type="file" id="image" class="form-control" accept="image/" @change="handleChange">
                <p v-if="imageError != ''" class="text-danger">{{ imageError }}</p>
            </div>
            <button class="btn btn-success">Them moi</button>
        </form>
    </div>
</template>
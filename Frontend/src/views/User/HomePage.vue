<script setup>
import "@/assets/client.css";
import "bootstrap/dist/css/bootstrap.min.css";
import "bootstrap/dist/js/bootstrap.bundle.min.js"
import Header from '@/components/User/Header.vue';
import Navbar from '@/components/User/Navbar.vue';
import Footer from '@/components/User/Footer.vue';
import { RouterView } from 'vue-router';
</script>
<template>
   <div class="container-fluid p-0">
      <div class="row">
         <div class="col-12">
            <!-- header -->
            <Header/>
            <!-- header -->
         </div>
      </div>
      <div class="row">
         <div class="col-12">
            <!-- Nav -->
           <Navbar />
         </div>
      </div>
      <RouterView/>
      ]
      <div class="row">
         <div class="col-12">
            <!-- footer -->
           <Footer/>
         </div>
      </div>
</div>

</template>


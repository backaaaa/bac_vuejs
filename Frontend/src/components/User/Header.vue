<script setup>
import anh1 from "@/assets/image/anh1.jpeg";
import anh2 from "@/assets/image/anh2.jpeg";
import anh3 from "@/assets/image/anh3.jpeg";</script>
<template>
     <div id="carouselExample" class="carousel slide">
               <div class="carousel-inner">
                  <div class="carousel-item active">
                     <img :src="anh1" class="d-block w-100 img-header" >
                  </div>
                  <div class="carousel-item">
                     <img :src="anh2" class="d-block w-100 img-header" >
                  </div>
                  <div class="carousel-item">
                     <img :src="anh3" class="d-block w-100 img-header" >
                  </div>
               </div>
               <button class="carousel-control-prev" type="button" data-bs-target="#carouselExample"
                  data-bs-slide="prev">
                  <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                  <span class="visually-hidden">Previous</span>
               </button>
               <button class="carousel-control-next" type="button" data-bs-target="#carouselExample"
                  data-bs-slide="next">
                  <span class="carousel-control-next-icon" aria-hidden="true"></span>
                  <span class="visually-hidden">Next</span>
               </button>
            </div>
</template>
<style scoped lang="scss">
  .img-header {
   height:400px;
   object-fit: cover;

  }
  .main-website {
   min-height:800px;
  }
</style>
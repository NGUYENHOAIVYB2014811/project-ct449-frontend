<template>
    <div>
    
      <div>
        <div v-if="loggedInUser">
         <h2 class="text-primary text-center mt-3 pt-3"> Chào mừng quản lý <b>{{ loggedInUser.username }}</b> đến với trang quản lý mượn sách </h2>
        </div>
      
      </div>
    </div>
   
                <div class="col">
                    <img src="http://khoahocphattrien.vn/Images/Uploaded/Share/2016/11/06/7a73.jpg" alt="" class="w-100 position-relative" style="height: 400px;">
                    <h1 class="position-relative text-center black-lives-matter " style="bottom: 230px">Thư viện sách</h1>
                </div>
  </template>
  
 
  
  <style scoped>
  .page {
    text-align: left;
    max-width: 750px;
  }
  </style>
  <script>
  import { mapGetters, mapActions } from "vuex";
  export default {
    computed: {
      ...mapGetters(["loggedInUser"]),
    },
  
  };
  </script>
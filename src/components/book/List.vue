<template>
    <table class="table">
      <thead>
        <tr class="bg-success text-black">
          <th scope="col">Tên sách</th>
          <th scope="col">Đơn giá</th>
          <th scope="col">Số quyển</th>
          <th scope="col">Năm xuất bản</th>
          <th scope="col">Nhà xuất bản</th>
          <th scope="col">Tác giả</th>
          <th scope="col">Hành động</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(book, index) in Books" :key="book._id"
            :class="{ active: index === activeIndex }" >
          <td>{{ book.TENSACH }}</td>
          <td>{{ book.DONGIA }}</td>
          <td>{{ book.SOQUYEN }}</td>
          <td>{{ book.NAMXUATBAN }}</td>
          <td>{{ book.TenNxb }}</td>
          <td>{{ book.TACGIA }}</td>
          <td>
            <router-link :to="{
              name:'book.edit',
              params:{id:book._id},
            }">
              <button class="mx-1 btn-sm btn-warning"> Sửa</button>
            </router-link>
            <button class="btn btn-sm btn-danger" @click="deletebook(book._id)">Xóa</button>
          </td>
        </tr>
      </tbody>
    </table>
  </template>
  <!-- Trong file bookList.vue -->


  
  <script>
import { useRouter } from 'vue-router';
import NxbService from "@/services/nxb.service";


export default {
    emits: ["delete:book"],
    props: {
      Books: {
        type: Array,
        default: () => []
      }
    },
   
    async created() {
      for (let i = 0; i < this.Books.length; i++) {
        const book = this.Books[i];
        const maNxb = book.MANXB;
      
        const response = await NxbService.get(maNxb);
        
   
        book.TenNxb = response.name;
    
      }
    },
    setup() {
      const router = useRouter();

      const editbook = (id) => {
      
        router.push({ name: 'book.edit', params: { id } });
      };

     

      return { editbook,  };
    },
    methods: {
      deletebook(id) {
          
            this.$emit("deletebook",id);
        
        },
    },
};
</script>

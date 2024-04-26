<template>
  <!-- <div>
    <h2>Đăng nhập</h2>
    <form @submit.prevent="login">
      <div>
        <label for="username">Tên đăng nhập:</label>
        <input type="text" id="username" v-model="nhanvien.username">
      </div>
      <div>
        <label for="password">Mật khẩu:</label>
        <input type="password" id="password" autocomplete="current-password" v-model="nhanvien.password">
      </div>
      <button type="submit">Đăng nhập</button>
    </form>
    <p v-if="error">{{ error }}</p>
  </div> -->


  <!-- Section: Design Block -->
  <section class="">
    <!-- Jumbotron -->
    <div class="px-4 py-5 px-md-5 text-center text-lg-start" style="background-image: url(http://vuthuvien.bvhttdl.gov.vn/wp-content/uploads/2022/04/PL4.jpg);">
      <div class="container">
        <div class="row gx-lg-5 align-items-center">
          <div class="col-lg-6 mb-5 mb-lg-0">
            <h3 class="my-5 display-3 fw-bold ls-tight">
              <span class="black-lives-matter">Chào mừng đến với trang quản lý thư viện</span>
            </h3>

          </div>

          <div class="col-lg-6 mb-5 mb-lg-0">
            <div class="card">
              <div class="card-body py-5 px-md-5" style="background-color: aqua">
                <form @submit.prevent="login">
                    <h4 class="text-center"> Đăng nhập</h4>
                  <div data-mdb-input-init class="form-outline mb-4">
                    <input type="text" id="form3Example3" class="form-control" v-model="nhanvien.username" placeholder="Email" />
                    <!-- <label class="form-label" for="form3Example3">Name</label> -->
                  </div>


                  <div data-mdb-input-init class="form-outline mb-4">
                    <input type="password" id="form3Example4" class="form-control" v-model="nhanvien.password"  placeholder="Password"/>
                    <!-- <label class=" form-label" for="form3Example4">Password</label> -->
                  </div>




                  <button type="submit" data-mdb-button-init data-mdb-ripple-init
                    class="btn btn-primary btn-block mb-4">
                    Đăng nhập
                  </button>
                  <p v-if="error">{{ error }}</p>


                </form>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

  </section>

</template>
<style>
	@import url('https://fonts.googleapis.com/css?family=Red+Hat+Display:900&display=swap');
	.black-lives-matter {
	font-size: 5vw;
	line-height: 8vw;
	margin: 0;
	font-family: 'Red Hat Display', sans-serif;
	font-weight: 900;
	background: linear-gradient(90deg, rgba(2,0,36,1) 0%, rgba(208,2,187,1) 52%, rgba(0,212,255,1) 100%);
	
	background-size: 40%;
	background-position: 50% 50%;
	-webkit-background-clip : text;
	color: rgba(0,0,0,0.08);
	animation: zoomout 10s ease 500ms forwards;
	}

	@keyframes zoomout {
	from {
		background-size: 60%;
	}
	to {
		background-size: 5%;
	}
	}
</style>
<script>
import { mapActions } from 'vuex';
import nhanvienService from "@/services/nhanvien.service"

export default {
  data() {
    return {
      nhanvien: { username: '', password: '' },
      error: '',
    };
  },
  methods: {
    ...mapActions(['setLoggedInUser']),
    async login() {
      try {
        const response = await nhanvienService.login(this.nhanvien);
        if (response.status === "success") {
          console.log('response:', response);

          localStorage.setItem('loggedInUser', JSON.stringify({ username: response.user.HOTENNV, id: response.user._id ,role:"admin"}));
          this.setLoggedInUser({ username: response.user.HOTENNV, id: response.user._id, role: "admin" });
      //  console.log('loggedInUser:',loggedInUser.role);
          
          this.$router.push('/quanly');
        } else {
          this.error = 'Tên đăng nhập hoặc mật khẩu không chính xác.';
        }
      } catch (error) {
        this.error = 'Đăng nhập không thành công. Vui lòng thử lại.';
      }
    }
  },
  created() {
    // Kiểm tra nếu có thông tin đăng nhập trong local storage khi component được tạo
    const loggedInUser = localStorage.getItem('loggedInUser');
    if (loggedInUser) {
      this.$store.dispatch('setLoggedInUser', JSON.parse(loggedInUser));
    }
  }
};
</script>

<template>
   <div class="container">
      <div class="d-flex justify-content-center h-100">
         <div class="card shadow-lg p-3 mb-5 bg-body rounded">
            <div class="card-header text-center">
               <h3>Đăng nhập</h3>
            </div>
            <div class="card-body">
               <form @submit.prevent="login()">
                  <div class="input-group form-group">
                     <div class="input-group-prepend">
                        <span class="input-group-text">
                           <i class="fa-solid fa-envelope"></i>
                        </span>
                     </div>
                     <input
                        type="email"
                        class="form-control"
                        placeholder="Email"
                        @blur="validate()"
                        v-model="user.email"
                        :class="{ 'is-invalid': errors.email }"
                     />
                     <div class="invalid-feedback" v-if="errors.email">
                        {{ errors.email }}
                     </div>
                  </div>
                  <div class="input-group form-group">
                     <div class="input-group-prepend">
                        <span class="input-group-text"
                           ><i class="fas fa-key"></i
                        ></span>
                     </div>
                     <input
                        type="password"
                        class="form-control"
                        placeholder="Mật khẩu"
                        @blur="validate()"
                        v-model="user.password"
                        :class="{ 'is-invalid': errors.password }"
                     />
                     <div class="invalid-feedback" v-if="errors.password">
                        {{ errors.password }}
                     </div>
                  </div>
                  <div class="form-group">
                     <input
                        type="submit"
                        value="Đăng nhập"
                        class="btn login_btn"
                     />
                  </div>
               </form>
            </div>
            <div class="card-footer">
               <div class="d-flex justify-content-center links">
                  <p>
                     Nếu bạn chưa có tài khoản!
                     <router-link to="/register"><b>ĐĂNG KÝ</b></router-link>
                  </p>
               </div>
            </div>
         </div>
      </div>
   </div>
</template>

<script>
import UserService from "../services/user.service";

export default {
   data() {
      return {
         errors: {
            email: "",
            password: "",
         },
         user: {
            email: "",
            password: "",
         },
      };
   },
   methods: {
      validate() {
         let isValid = true;

         this.errors = {
            email: "",
            password: "",
         };

         if (!this.user.email) {
            this.errors.email = "Email là bắt buộc";
            isValid = false;
         }

         if (!this.user.password) {
            this.errors.password = "Mật khẩu là bắt buộc";
            isValid = false;
         }

         return isValid;
      },

      async checkout() {
         // Kiểm tra hợp lệ trước khi thanh toán
         if (!this.validate()) {
            // Nếu dữ liệu không hợp lệ, bạn có thể thực hiện các hành động phù hợp, ví dụ: hiển thị thông báo lỗi
            return;
         }

         // Thực hiện các bước thanh toán ở đây
         // Ví dụ:
         // Gửi dữ liệu đơn hàng và thông tin thanh toán lên máy chủ
         // Xóa các sản phẩm trong giỏ hàng sau khi đã thanh toán thành công
         // Chuyển người dùng đến trang xác nhận đơn hàng hoặc trang thành công

         // Sau khi thanh toán thành công, bạn có thể làm như sau để xóa giỏ hàng:
         this.products = [];
         localStorage.removeItem("localProductCart");

         // Hoặc nếu bạn muốn đưa người dùng đến trang xác nhận đơn hàng, bạn có thể chuyển hướng bằng cách sử dụng router Vue:
         // this.$router.push('/xac-nhan-don-hang');
      },

      async login() {
         if (this.validate()) {
            try {
               const userLogin = await UserService.login(this.user);
               const localUserLogin = JSON.stringify(userLogin);
               localStorage.setItem("localUserLogin", localUserLogin);
               localStorage.setItem("userName", userLogin.name);
               if (userLogin.role === "user") {
                  this.$router.push({ name: "home" });
               } else if (userLogin.role === "admin") {
                  this.$router.push({ name: "admin" });
               } else {
                  alert("Xin lỗi! Bạn đã nhập sai email hoặc mật khẩu!");
               }
            } catch (error) {
               alert("Đã xảy ra lỗi trong quá trình đăng nhập!");
            }
         }
      },
   },
};
</script>

<style scoped>
.container {
   padding-top: 80px;
   padding-bottom: 180px;
   margin-top: 10px;
   
}
.card {
   height: 400px;
   width: 400px;
   /* border-radius: 10px; */
   /* border: 1px solid #E38B29; */
}
.social_icon span {
   font-size: 60px;
   margin-left: 10px;
   color: #169008;
}
.social_icon span:hover {
   color: white;
   cursor: pointer;
}
.card-header h3 {
   color: #169008;
}
.card-header{
   background-color: white;
   border-radius: 10px;
}
.social_icon {
   position: absolute;
   right: 20px;
   top: -45px;
}
.input-group-prepend span { 
   width: 50px;
   background-color: white;
   color: #169008;
   border: 0 !important;
}
input:focus {
   outline: 0 0 0 0 !important;
   box-shadow: 0 0 0 0 !important;
}
.login_btn {
   color: #169008;
   border-radius: 20px;
   border: 1px solid #169008;
   width: 50%;
   margin-left: 25%;
   /* margin-right: 25%; */
}
.login_btn:hover {
   color: white;
   background-color: #169008;
}
.card-footer{
   background-color: white;

}
.links {
   color: #169008;
}
.links a {
   margin-left: 4px;
   text-decoration: none;
   color:#169008;
}
</style>

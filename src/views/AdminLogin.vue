;<template>
  <auth-container>
    <h2>Admin Login</h2>
    <form @submit.prevent="login" >
      <div class="form-control">
        <label for="email">Email:</label>
        <input type="email" name="email" placeholder="abc@mail.com" autocomplete="off" v-model="email" required>
      </div>
      <div class="form-control">
        <label for="password">Password:</label>
        <input type="password" name="password" placeholder="Password"  v-model="password" required>
      </div>
      <button>
        Submit
      </button>
      
    </form>
  </auth-container>
</template>

<script>
import AuthContainer from "../components/UI/AuthContainer.vue"
import api from "../utils/api.js"
import Toast from "../utils/Toast.js"

export default {
  components:{
    AuthContainer
  },
  data(){
    return{
      email:"",
      password: ""
    }
  },
  methods:{
    async login(){
      const res = await api.post("/store/login",{
        email: this.email,
        password:this.password
      })
      if(res.data.success){
        localStorage.setItem("token",res.data.accessToken )
        localStorage.setItem("userType","ADMIN")
        // Reload to open app as ADMIN
        window.location.reload()
        const toast = new Toast(res.data.message)
        toast.show()
      }else{
        const toast = new Toast(res.data.message,"", "danger")
        toast.show()
      }
    }
  }
}
</script>

<style scoped>
p{
  font-size: .9rem;
}
</style>
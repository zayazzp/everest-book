;<template>
  <auth-container>
    <h2>Login</h2>
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
    <p>New to Everest Book? <router-link to="/signup">Join Now</router-link></p>
    <p>Admin Login <router-link to="/admin-login">Here</router-link></p>
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
      const res = await api.post("/user/login",{
        email: this.email,
        password:this.password
      })
      if(res.data.success){
        localStorage.setItem("token",res.data.accessToken )
        localStorage.setItem("uid",res.data.data._id )
        localStorage.setItem("userType","USER")
        window.location.reload()
        const toast = new Toast(res.data.message)
        toast.show()
      }
      else{
        const toast = new Toast(res.data.message,"", "danger" )
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
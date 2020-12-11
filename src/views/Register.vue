<template>
  <div class="container">
    <div class="card register">
      <div class="card-body">
        <h2 class="card-title text-center">Register</h2>
        <form v-on:submit= "register" @submit.prevent="register" class="text-center">
          <div class="form-group">
            <input
              type="text"
              class="form-control"
              placeholder="Enter your Identity number:"
              name="id"
              required
              v-model="REGISTERUSER.id_number"
            /><br>

            <input
              type="text"
              class="form-control"
              placeholder="Enter your phone number:"
              name="phone"
              required
              v-model="REGISTERUSER.phone_number"
            /><br>
            
            <input
              type="text"
              class="form-control"
              placeholder="Enter your name and surname:"
              name="fullnames"
              required
              v-model="REGISTERUSER.full_names"
            /><br>

            <input
              type="text"
              class="form-control"
              placeholder="Enter your address:"
              name="address"
              required
              v-model="REGISTERUSER.address"
            /><br>
    
            <label for="role"> I am a technician</label>
            <input type="checkbox" class="form-control" name="role" value="is_technician" v-model="REGISTERUSER.is_technician"><br>
  
            <input
              type="password"
              class="form-control"
              placeholder="Enter your password:"
              name="password"
              required
              v-model="REGISTERUSER.password"
            /><br>

            <input
              type="password"
              class="form-control"
              placeholder="Confirm passsword:"
              name="passwordconfirm"
              required
              v-model="REGISTERUSER.password_confirm"
            /><br>
            

            <p v-if="errorText" class="text-danger">{{ errorText }}</p>
          </div>
          <button type="button" @click="register(REGISTERUSER)" class="btn btn-primary">Register</button>
        </form>
      </div>
    </div> 
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "register",
  data() {
    return {
      REGISTERUSER: {
        id_number: '',
        phone_number: '',
        full_names: '',
        address: '',
        is_technician: false,
        password: '',
        password_confirm: ''

      },
      errorText: null
    };
  },
  methods: {
    register(item) {

      var number =  item.phone_number;
      var id_number = item.id_number;
      var is_technician = item.is_technician;

      axios.get(`http://localhost:3000/users/checkNumber/${number}/${id_number}`)

      .then((results) => {

           console.log("data found: ", results);

           let userExists = results.data.numbercheck.data[0].fn_register_number_exists;

         
         if(item.phone_number.length < 10){

          alert('Your phone number must not have less than 10 characters')

        }else if(item.password != item.password_confirm){

          alert('Password does not match');

        }else if(item.password.length <= 7){

          alert('Password should be atleast 8 characters long');

        }else if(userExists == true){

          alert('Phone or ID number already registered with the system')

        }else{

            axios.get()
            let newUser = {
                id_number: item.id_number,
                phone_number: item.phone_number,
                full_names: item.full_names,
                address: item.address,
                is_technician: item.is_technician,
                password: item.password,
                password_confirm: item.password_confirm
            }

            axios.post('http://localhost:3000/users/register', newUser)
            .then(() =>{

              if(is_technician == true){

                alert("You are now successfully registered as a technician."),
                this.$router.push({ name: "List"})

            }else{

              alert("You are now successfully registered as a client."),
                this.$router.push({ name: "ClientAppointments"})

            }

           }).catch(error => {
             console.log(error);
           })
            // e.preventDefault();

       }
        // e.preventDefault();
      })
    }
  }
};
</script>

<style scoped>
.register {
  max-width: 450px;
  margin-top: 50px;
  display: block;
  margin-left: auto;
  margin-right: auto;
}
</style> 
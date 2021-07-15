<template>
  <q-page class="flex justify-start">
        <div class="row" >
  <!--         <div>
    <q-btn color="primary" @click="open = true" label="Open" />
    <q-modal v-model="open">
      <q-btn color="primary" @click="open = false" label="Close" />
    </q-modal>
  </div>
   -->    		<q-card square style="width:1024px;height:485px;">  
       		     <q-card-section>
                  <q-form ref="myForm" class="q-px-lg q-pt-xl">
                    <q-input square clearable v-model="email" id="email" ref="email" type="email" label="Email" :rules="[val => validateEmail(val) || 'invalid email address, Please enter valid email address']">
                      <template v-slot:prepend>
                        <q-icon name="email" />
                      </template>
                    </q-input>
                    <q-input square clearable v-model="password" ref="password" id="password" type="password" label="Password" :rules="[ val => val.length >= 6 || 'Minimum 6 characters']">
                      <template v-slot:prepend>
                        <q-icon name="lock" />
                      </template>
                    </q-input> 
                   <!--  <div>
                        <q-btn label="Sign in" type="submit" color="purple-4" icon="send"/>
                    </div> -->
                  </q-form>
                </q-card-section>
                <div style="background-color: red;">                
                   <h6 class="error" ref="erreur" style="text-align: center;" id="error"></h6>
                 </div>

                 <q-card-actions class="q-px-lg">
                  <q-btn unelevated type="submit" icon="send"  round dense flat size="lg" color="purple-4" class="full-width text-white" label="Sign In" @click="sendForm()"/>
                </q-card-actions>
                
            </q-card>

        </div>
  </q-page>
</template>

<script>
import { defineComponent,ref } from 'vue';
import axios from 'axios'
import { jQuery } from 'jquery';
import { LocalStorage, SessionStorage } from 'quasar';

let value = LocalStorage.getItem("userToken");
let value_session = SessionStorage.getItem("userToken");


if (value && value.length>0 && value_session && value_session.length>0) {
  alert(value);

  window.location.href="#/home";
}

export default defineComponent({
  name: 'PageIndex',
  data () {
    return {
      email: '',
      password: '',
      open: false

    }
  },
  methods: {
    isValidEmail () {
      const emailPattern = /^(?=[a-zA-Z0-9@._%+-]{6,254}$)[a-zA-Z0-9._%+-]{1,64}@(?:[a-zA-Z0-9-]{1,63}\.){1,8}[a-zA-Z]{2,63}$/;
      return emailPattern.test(this.email) || 'Invalid email';
    },
     validateEmail (email) {
      // Source : https://stackoverflow.com/questions/46155/how-to-validate-an-email-address-in-javascript
      const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
      return re.test(String(email).toLowerCase())
    },
    showModal: function () {
      this.opened = true
    },
    
  sendForm(){

    let disp_err = document.getElementById('error');
    let email_input = document.getElementById("email");
    let password_input = document.getElementById("password");

    axios({
      method: 'post',
      url: 'http://localhost:3000/login',
      data: {
        email: this.email,
        password: this.password
      }
    }).then(function (response) {
      const status = response.data.status;


      if (status=="1") {
        disp_err.innerHTML=response.data.message;
      }
      else if (status=="2") {

        LocalStorage.set("userToken",response.data.token);

        SessionStorage.set("userToken",response.data.token);
        disp_err.innerHTML=""
        window.location.href="#/home";
      }
      else{
        disp_err.innerHTML="Erreur de validation";
      }

    })
    .catch(function (error) {
      console.log(error);
    });
  },

}


})

</script>





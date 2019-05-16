<template>
  <div>
    <div v-if="!user">
       <h1>Sign Uo</h1>
      <input type="text" v-model="login" placeholder="Login"><br>
      <input type="password" v-model="password" name="" id="" placeholder="password"><br>
      <input type="email" name="" id="" placeholder="email"><br>
      <button @click="signup">SignUp</button>
    </div>
   
    <div v-else>
      <h2>Confirm SignUp</h2>
      <input type="text" v-model="code" placeholder="Code"><br>
      <button @click="confirm">Confirm</button>
    </div>
  </div>
</template>


<script> 

import { Auth } from 'aws-amplify';

export default {
  data() {
    return {
      login:'',
      email: '',
      password: '',
      code: '',
      user: ''
    } 
  },
  
  methods: {

    signup() {
      Auth.signUp({
      username: this.login,
      password: this.password,
      attributes: {
        email: this.email,
      },
      validationData: []
    }).then(res => {
      this.user = res.user
    }).catch(err => console.log(err))
    },
    
    confirm() {
      Auth.confirmSignUp(this.login, this.code, {
        
        forceAliasCreation: true
      }).then(data => {
        this.$router.push("/")
      })
        .catch(err => console.log(err));
    },
  }
}
</script>


<style>
input {
  margin: 10px;
  padding: 16px;

}
</style>

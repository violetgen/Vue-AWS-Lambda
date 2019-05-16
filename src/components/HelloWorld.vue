<template>
  <div class="hello">
    <!-- <amplify-authenticator></amplify-authenticator> -->
    <div v-if="!signedIn">
      <input type="text" v-model="login" placeholder="login"><br>
      <input type="password" v-model="password" placeholder="password"><br>
      <button @click="signIn">Sign in</button>
    </div>
    
    <div v-if="signedIn">
      <!-- <amplify-sign-out></amplify-sign-out> -->
      <button @click="signOut">Sign Out</button>
    </div>
  </div>
</template>

<script>

import Amplify, { Auth } from 'aws-amplify';
import { AmplifyEventBus } from 'aws-amplify-vue'

export default {
  name: 'HelloWorld',
  props: {
  },
  data(){
    return {
      login: '',
      password: ''
    }
  },

  computed: {
    signedIn() {
      return this.$store.state.signedIn
    }
  },

  created() {
    this.findUser();

    AmplifyEventBus.$on("authState", info => {
      if(info === "signedIn") {
        this.findUser()
      } else {
        this.$store.state.signedIn = false
      }
    });
  },

  methods:{

    signOut(){
      Auth.signOut()
          .then(data => {
            //assigned the boolean gotten to th authenticated user:
            this.$store.state.signedIn = !!data;
          })
          .catch(err => console.log(err));
    },

    signIn() {
      Auth.signIn(this.login, this.password)
          .then(user => {
            this.$store.state.signedIn = !!user; //return the signed in user as a boolean
            this.$store.state.user = user;
          })
          .catch(err => console.log(err));
    },

    async findUser() {
      try {
        const user = await Auth.currentAuthenticatedUser();
        this.$store.state.signedIn = true
        this.$store.state.user = user;
        console.log('this is the user: ', user);
      } catch(err) {
        this.$store.state.signedIn = false
        this.$store.state.user = null;

      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input {
  padding: 16px;
  margin: 10px;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

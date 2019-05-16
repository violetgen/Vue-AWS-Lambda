<template>
  <div>
    <h1>THis is the server</h1>
    <h2>{{ info.data }}</h2>
  </div>
</template>


<script>

import axios from 'axios';

export default {
  data(){
    return {
      info: ''
    }
  },

  mounted() {
    if(this.$store.state.user) {
      const jwt = this.$store.state.user
                  .getSignInUserSession()
                  .getIdToken()
                  .getJwtToken();
      const config = {
        headers: {
          authorization: jwt
        }
      }

      axios.get('https://ev0sss6084.execute-api.us-east-1.amazonaws.com/v1/m', config)
            .then(val => 
              this.info = val
            )
            .catch(err => console.log(err));
    }
  }
}
</script>

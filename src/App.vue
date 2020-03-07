<template>
<div id="app">
  <div v-if="login" id="login">
    <h1>Police Safety Control</h1>
    <input id="pin" type="password" placeholder="Enter pin">
    <button @click="signin" id="btn">Login</button>
  </div>
  <div id="main" v-else>
    <ol :key = "index" v-for="(location, index) in locations">
      <li>{{location}}</li>
    </ol>
  </div>
</div>
</template>

<script>
import database from './firebaseInit'

export default {
  data() {
    return {
      login: true,
      locations: []
    }
  },
  methods: {
    signin() {
      this.login = !this.login
      database.collection('entries').get().then(data => {
        data.docs.map(doc => {
          this.locations.push(doc.data().location)
        })
        data.docs.map(res => {
          database.collection('entries').doc(res.id).onSnapshot(doc => {
            alert('Help is needed!')
          })
        })
      })
      database.collection('entries')
    }
  }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
}

#app {
    height: 100vh;
    margin: 0 auto;
    display: flex;
    justify-content: center;
}

#login {
    display: flex;
    flex-direction: column;
    align-items: center;
}

h1 {
    margin-top: 20px;
    font-size: 5em;
}

#pin {
    width: 250px;
    height: 30px;
    border-radius: 5px;
    margin-top: 50px;
}

#pin:focus {
    outline-style: none;
    box-shadow: 0 0 1pt 1pt rgb(26, 96, 224);
}

#btn {
    width: 100px;
    font-size: 1em;
    margin-top: 15px;
    padding: 5px 20px;
    border-radius: 5px;
    background-color: rgb(79, 131, 226);
    color: white;
}

#btn:focus {
    outline-style: none;
    box-shadow: 0 0 1pt 1pt rgb(79, 131, 226);
}
</style>

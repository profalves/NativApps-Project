<template>
  <div class="row justify-center ">
    
    <q-card inline class="login center">
      <q-card-media class="layout-padding">
        <img src="http://fedesoft.org/wp-content/uploads/2015/12/fondo_blanco-1-1.png" 
             width="10%" height="10%" />
      </q-card-media>
      <q-card-title>
        Login
      </q-card-title>
      <q-card-main>
        <q-input v-model="user" 
                 float-label="Usuário" 
                 align="center" />
        <q-input v-model="pass" 
                 float-label="Senha" 
                 type="password" 
                 align="center" 
                 @keyup.enter="login()"/>
      </q-card-main>
      <q-card-separator />
      <div class="layout-padding center">
        <q-btn color="primary" @click="login()">Entrar</q-btn> 
      </div>
    </q-card>
  </div>
</template>
  

<script>
/* eslint-disable */
import {
  openURL,
  QInput,
  QLayout,
  QToolbar,
  QToolbarTitle,
  QBtn,
  QIcon,
  QList,
  QListHeader,
  QItem,
  QItemSide,
  QItemMain,
  QCard,
  QCardTitle,
  QCardMedia,
  QCardActions,
  QCardSeparator,
  QCardMain,
  Loading,
  Toast
} from 'quasar'
import API from './api.js'
import axios from 'axios'
export default {
  name: 'login',
  components: {
    QLayout,
    QInput,
    QToolbar,
    QToolbarTitle,
    QBtn,
    QIcon,
    QList,
    QListHeader,
    QItem,
    QItemSide,
    QItemMain,
    QCard,
    QCardTitle,
    QCardMedia,
    QCardActions,
    QCardSeparator,
    QCardMain,
  },
  data () {
    return {
      API,
      user: '',
      pass: '',
    }
  },
  methods: {
    launch (url) {
      openURL(url)
    },
    login(){
      Loading.show()
      axios.get(this.API + this.user + '/' + this.pass)
      .then((res) => {
        Loading.hide()
        //console.log(res.data)
        this.$router.push('/')
        localStorage.setItem('logado?', true)
      })
      .catch((e) => {
        Loading.hide()
        console.log(e.response)
        Toast.create('Usuário ou senha incorretos')
      })
      
    }
  },
  created(){
    localStorage.setItem('logado?', false)
  }
}
</script>

<style scoped>
  body{
    background-color: #fff;
  }
  .white{
    color: #fff;
  }
  .login{
    width: 500px; 
    background-color: #fff;
    margin-top: 25px;
  }
  .center{
    text-align: center;
  }
</style>

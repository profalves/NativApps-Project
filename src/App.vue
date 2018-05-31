<template>
  <!-- Don't drop "q-app" class -->
  <div id="q-app">
    <q-layout
      ref="layout"
      view="lHh Lpr fff"
      :left-class="{'bg-dark': true}"
    >
      <q-toolbar slot="header" class="glossy" color="dark" v-if="$route.path !== '/login'">
        <q-btn
          flat
          @click="$refs.layout.toggleLeft()"
        >
          <q-icon name="menu" />
        </q-btn>

        <q-toolbar-title>
          NativApps
          <div slot="subtitle">Sistema de cursos</div>
        </q-toolbar-title>
      </q-toolbar>

      <div slot="left" style="color: #fff;" v-if="$route.path !== '/login'">
        <q-list no-border link inset-delimiter>
          <q-list-header class="white">Menu</q-list-header>
          <q-side-link to="/hello">
            <q-item>
              <q-item-side icon="fa-flag-checkered" class="white" />
              <q-item-main label="Início" />
            </q-item>
          </q-side-link>
          <q-side-link to="/alunos">
            <q-item>
              <q-item-side icon="school" class="white" />
              <q-item-main label="Alunos" sublabel="Cadastros de Alunos" />
            </q-item>
          </q-side-link>
          <q-side-link to="/professores">
            <q-item>
              <q-item-side icon="face" class="white" />
              <q-item-main label="Professores" sublabel="Cadastros de Professores" />
            </q-item>
          </q-side-link>
          <q-side-link to="/cursos">
            <q-item>
              <q-item-side icon="account_balance" class="white" />
              <q-item-main label="Cursos" sublabel="Cadastros de Cursos" />
            </q-item>
          </q-side-link>
          <q-item @click="launch('https://github.com/profalves/NativApps-Project')">
            <q-item-side icon="fa-github" class="white" />
            <q-item-main label="Github" sublabel="Repositório do Projeto" />
          </q-item>
          <q-side-link to="/login">
            <q-item>
              <q-item-side icon="exit_to_app" class="white" />
              <q-item-main label="Sair" /> 
            </q-item>
          </q-side-link>
        </q-list>
      </div>
      
      <transition name="fade" mode="out-in">
        <router-view class="layout-padding" />    
      </transition>
      
  </q-layout>
  </div>
</template>

<script>
/* eslint-disable */
import {
  openURL,
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
  QSideLink,
} from 'quasar'

export default {
  name: 'index',
  components: {
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
    QSideLink
  },
  data () {
    return {
    }
  },
  methods: {
    launch (url) {
      openURL(url)
    }
  },
  beforeUpdate(){
    if(localStorage.getItem('logado?') === 'false'){
      this.$router.push('login')
      localStorage.getItem('logado?')
    }
  },
  created(){
    if(localStorage.getItem('logado?') === 'false'){
      this.$router.push('login')
      localStorage.getItem('logado?')
    }
  }
}
</script>

<style>
  body{
    background-color: aliceblue;
  }
  .white{
    color: #fff;
  }
  /*transitions router*/
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s ease;
  }
  .fade-enter, .fade-leave-active {
    opacity: 0
  }
  .child-view {
    position: absolute;
    transition: all .5s cubic-bezier(.55,0,.1,1);
  }
  .slide-left-enter, .slide-right-leave-active {
    opacity: 0;
    -webkit-transform: translate(30px, 0);
    transform: translate(30px, 0);
  }
  .slide-left-leave-active, .slide-right-enter {
    opacity: 0;
    -webkit-transform: translate(-30px, 0);
    transform: translate(-30px, 0);
  }
</style>

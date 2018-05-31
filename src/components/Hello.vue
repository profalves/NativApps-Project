<template>
  <div class="row justify-center ">
    <img src="http://fedesoft.org/wp-content/uploads/2015/12/fondo_blanco-1-1.png" width="60%" height="60%" />
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
  QItemMain
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
    QItemMain
  },
  data () {
    return {
    }
  },
  methods: {
    launch (url) {
      openURL(url)
    }
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
</style>

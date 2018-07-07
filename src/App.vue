<template>
<div id="app">
<list-maker/>
</div>
</template>

<script>

import listMaker from './components/listMaker'
import Vue from 'vue'
import Buefy from 'buefy'
import 'buefy/lib/buefy.css'

Vue.use(Buefy)

export default {
  components: {
    listMaker
  }
}
</script>
<style>
</style>

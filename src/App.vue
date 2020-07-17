<template>
  <div class="main-container">
    <app-header></app-header>
    <div class="content-container">
        <app-sidebar></app-sidebar>
        <app-content></app-content>
    </div>
  </div>
</template>

<script>
import Header from './components/Header'
import Sidebar from './components/Sidebar'
import Content from './components/Content'

export default {
  name: 'App',
  components: {
    'app-header': Header,
    'app-sidebar': Sidebar,
    'app-content': Content,
  },
  data() {
    return {
    };
  }
}
</script>

<style>

</style>

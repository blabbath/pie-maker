<template>
  <div class="main-container">
    <app-header></app-header>
    <div class="content-container">
          <div class="clr-col-4">
            <app-input></app-input>
          </div>
          <div class="clr-col-8">
            <app-content></app-content>
          </div>
    </div>
  </div>
</template>

<script>
import Header from './components/Header'
import Input from './components/Input'
import Content from './components/Content'

export default {
  name: 'App',
  components: {
    'app-header': Header,
    'app-input': Input,
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

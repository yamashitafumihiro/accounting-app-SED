<template>
   <div class="hello-world">
    <NavigationBar />
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
import NavigationBar from './NavigationBar.vue'

export default {
  name: 'HelloWorld',
  components: {
    NavigationBar
  },
  data() {
    return {
      msg: '割り勘アプリへようこそ!'
    }
  }
};
</script>

<style>
.hello-world {
  padding: 20px;
}
</style>

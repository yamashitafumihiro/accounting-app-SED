<template>
  <div>
    <NavigationBar />
    <h1>ホームページ</h1>
    <p>ここはホームページです。</p>
  </div>
</template>

<script>
import NavigationBar from '@/components/NavigationBar.vue';

export default {
  name: 'HelloWorld',
  components: {
    NavigationBar
  }
};
</script>

<style>
/* ここにスタイルを追加 */
</style>

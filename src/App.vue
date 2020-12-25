<template>
  <div id="app">
    <train />
  </div>
</template>

<script>
import Train from '@/components/Train'

export default {
  name: 'App',
  components: {
    Train
  }
}
</script>

<style>
 html, body {
    background-color: black;
    color: rgb(45, 223, 45);
 }
</style>

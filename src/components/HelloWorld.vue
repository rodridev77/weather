<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button class="bnt btn-success">Aqui</button>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

<template>
  <div id="container">

      <h3>Decimal</h3>
      <h3>Hex</h3>
      <h3>Binary</h3>
      <p>{{value.decimal}}</p>
      <p>{{value.hex}}</p>
      <p>{{value.binary}}</p>

  </div>
</template>

<script>
    export default {
        name: "history",
        props: ['value']
    }
</script>

<style scoped>

    #container{
        display: grid;
        grid-template-columns: 1fr 1fr 1fr;
        border: #1E305B 2px solid;
        border-radius: 5px;
        align-items: center;
        justify-items: center;
    }

</style>
<script>
export default {
  props: {
    changeAmount: {
      type: Function,
      required: true
    },
    convert: {
      type: Function,
      required: true
    }
  }
}
</script>

<template>
  <div>
    <input min="0" @input="changeAmount($event.target.value)" type="number" placeholder="Input number">
  </div>
</template>
 
<style scoped>
input {
  outline: none;
  border-radius: 3px;
  border: 0;
  padding: 10px 15px;
  background: #fafafa;
  color: #333;
  width: 500px;
  position: relative;
  top: -50px;
  font-size: 1.2em;
}
</style>
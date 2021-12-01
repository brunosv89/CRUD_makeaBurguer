<template>
  <div class="msg-container">
    <p>{{ msg }}</p>
  </div>
</template>

<script>
export default {
  name: "Mensagem",
  props: {
    msg: String,
  },
};
</script>

<style scoped>
.msg-container {
  color: #38618d;
  background-color: #cce5ff;
  border: 1px solid #b8daff;
  width: 80%;
  font-weight: bold;
  margin: 0 auto;
  text-align: center;
  padding: 5px;
  height: 30px;
  border-radius: 5px;
}
</style>

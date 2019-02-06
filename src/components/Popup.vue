<template>
  <div class="modal-container">
    <div class="modal-pointer"/>
    <div class="modal-content">
      <button v-on:click="onEdit">Edit</button>
      <button v-on:click="onDelete">Delete</button>
    </div>
  </div>
</template>


<script>
export default {
  name: "Popup",
  props: {
    onEdit: Function,
    onDelete: Function
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.modal-pointer {
  width: 0;
  height: 0;
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  border-bottom: 5px solid white;
}

.modal-container {
  width: 50px;
  margin: 0px auto;
  padding: 20px 30px;
  position: absolute;
  background-color: #fff;
  border-radius: 4px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}
</style>

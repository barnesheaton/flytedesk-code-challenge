<template>
  <div class="popup-container">
    <div class="popup-content">
      <button class="button" v-on:click="onEdit">Edit</button>
      <button class="button" v-on:click="onDelete">Delete</button>
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
.button {
  text-align: left;
  border: none;
  margin-bottom: 16px;
}
.popup-content {
  display: flex;
  flex-direction: column;
  padding: 16px 16px 0 16px;
  background-color: #fff;
  border: solid thin #cdd4e3;
  border-radius: 4px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
}
.popup-container {
  margin-top: 20px;
  position: absolute;
  right: -22px;
}

.popup-container:after,
.popup-container:before {
  content: "";
  display: block;
  position: absolute;
  right: 15px;
  bottom: 100%;
  width: 0;
  height: 0;
  border-style: solid;
  transform: rotate(-90deg);
}
.popup-container:after {
  top: -19px;
  border-color: transparent transparent transparent white;
  border-width: 10px;
}
.popup-container:before {
  top: -21px;
  right: 14px;
  border-color: transparent transparent transparent #cdd4e3;
  border-width: 11px;
}
</style>

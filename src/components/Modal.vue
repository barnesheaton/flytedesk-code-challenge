<template>
  <div class="modal-backdrop">
    <div class="modal">
      <div v-if="!!this.school">
        <p class="header">Edit School</p>
      </div>
      <div v-else>
        <p class="header">Add New School</p>
      </div>
      <div class="input-conatiner">
        <p class="label">School Name</p>
        <input v-model="name" placeholder="School Name" value="name">
        <p class="label">City</p>
        <input v-model="city" placeholder="City">
        <p class="label">State</p>
        <input v-model="state" placeholder="State">
        <p class="label">Zip Code</p>
        <input v-model="zip" placeholder="00000">
      </div>
      <div class="buttons">
        <button class="button white" v-on:click="close">Cancel</button>
        <button class="button" v-on:click="onSave">Save</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      name: "",
      city: "",
      state: "",
      zip: ""
    };
  },
  methods: {
    close() {
      this.$emit("close");
    },
    onSave: function() {
      this.$emit("onSave", {
        name: this.name,
        city: this.city,
        state: this.state,
        zip: this.zip
      });
      this.name = "";
      this.city = "";
      this.state = "";
      this.zip = "";
    }
  },
  name: "Modal",
  props: {
    school: Object | false
  },
  watch: {
    school(value) {
      this.name = value.name;
      this.city = value.city;
      this.state = value.state;
      this.zip = value.zip;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.button {
  flex: 1;
  background: #5faee1;
  border: none;
  color: #ffffff;
  padding: 16px 32px;
}
.buttons {
  display: flex;
}
.header {
  font-weight: 700;
  padding: 8px;
  text-align: center;
  background-color: #cdd4e3;
}
.input-conatiner {
  flex-direction: column;
  display: flex;
  padding: 16px 20px 30px 20px;
  border-bottom: solid thin #cdd4e3;
}
.label {
  font-weight: 900;
  margin: 16px 0 4px 0;
}
.modal {
  border-radius: 6px;
  width: 500px;
  background: #ffffff;
  box-shadow: 2px 2px 20px 1px;
  overflow-x: auto;
  display: flex;
  flex-direction: column;
}
.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}
.white {
  background: white;
  color: black;
}
::placeholder {
  /* Chrome, Firefox, Opera, Safari 10.1+ */
  color: #cdd4e3;
  opacity: 1; /* Firefox */
}
input {
  padding-left: 16px;
  height: 40px;
  border: thin solid #cdd4e3;
  border-radius: 4px;
}
p {
  margin: 0px;
  padding: 0px;
  line-height: 14px;
}
</style>

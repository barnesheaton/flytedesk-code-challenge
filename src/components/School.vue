<template>
  <div class="school" v-bind:class="{ schoolSelected: isSelected() }">
    <div class="checkbox">
      <label class="checkboxLabel">
        <input type="checkbox" :checked="selected" @change="onToggle">
        <span></span>
      </label>
    </div>
    <div class="item">
      <p>{{schoolData.name}}</p>
    </div>
    <div class="item">
      <p>{{schoolData.city}}</p>
    </div>
    <div class="item">
      <p>{{schoolData.state}}</p>
    </div>
    <div class="item">
      <p>{{schoolData.zip}}</p>
    </div>
    <div class="action" v-on:click="togglePopup">
      <font-awesome-icon icon="ellipsis-v" :style="{ color: '#5faee1' }"/>
    </div>
    <Popup v-show="isModalVisible" v-bind:onDelete="onDelete" v-bind:onEdit="onEdit"/>
  </div>
</template>

<script>
import Popup from "./Popup.vue";

export default {
  components: {
    Popup
  },
  data: function() {
    return {
      isModalVisible: false
    };
  },
  methods: {
    togglePopup: function() {
      this.isModalVisible = !this.isModalVisible;
    },
    onToggle: function() {
      this.$emit("onToggle", this.schoolData.id);
    },
    onDelete: function() {
      this.$emit("onDelete", this.schoolData.id);
    },
    onEdit: function() {
      this.$emit("onEdit", this.schoolData.id);
    },
    isSelected: function() {
      return this.selected;
    },
    isFirstIndex: function() {
      return !this.index;
    }
  },
  name: "School",
  props: {
    index: Number,
    schoolData: {
      id: Number,
      name: String,
      city: String,
      state: String,
      zip: String
    },
    selected: Boolean
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.action {
  border-right: solid #cdd4e3;
  border-right-width: thin;
  align-items: center;
  display: flex;
  flex-basis: 100px;
  justify-content: center;
}
.checkbox {
  border: solid #cdd4e3;
  border-width: 0 thin 0 thin;
  padding: 16px;
  display: flex;
  flex: 0;
  align-items: center;
}
.checkboxLabel input {
  display: none;
}
.checkboxLabel span {
  height: 12px;
  width: 12px;
  border-radius: 2px;
  border: 1px solid grey;
  display: inline-block;
  position: relative;
}
.item {
  align-items: center;
  flex: 1;
  display: flex;
  text-align: left;
  border-right: solid #cdd4e3;
  border-right-width: thin;
  padding-left: 10px;
}
.school {
  display: flex;
  border-bottom: solid #cdd4e3;
  border-bottom-width: thin;
}
.schoolSelected {
  background: #eef0f5;
}
[type="checkbox"]:checked + span:before {
  content: "";
  position: absolute;
  width: 6px;
  height: 3px;
  background: transparent;
  top: 3px;
  left: 2px;
  border: 2px solid #ffffff;
  border-top: none;
  border-right: none;
  transform: rotate(-45deg);
}
[type="checkbox"]:checked + span {
  border: 1px solid #5faee1;
  background: #5faee1;
}
</style>

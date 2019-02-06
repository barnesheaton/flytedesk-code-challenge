<template>
  <div>
    <div
      class="school"
      v-bind:class="{ 'school-selected': isSelected(), 'school-last': isLastIndex() }"
    >
      <div class="checkbox">
        <label class="checkbox-label">
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
        <span class="popup">
          <popup v-bind:onDelete="onDelete" v-bind:onEdit="onEdit"/>
        </span>
      </div>
    </div>
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
    isSelected: function() {
      return this.selected;
    },
    isLastIndex: function() {
      return this.isLastSchool;
    },
    isFirstIndex: function() {
      return !this.index;
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

    togglePopup: function() {
      this.isModalVisible = !this.isModalVisible;
    }
  },
  name: "School",
  props: {
    isLastSchool: Boolean,
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
  align-items: center;
  display: flex;
  flex-basis: 100px;
  justify-content: center;
}
.action .popup {
  visibility: hidden;
  position: relative;
  z-index: 1;
}
.action:hover .popup {
  visibility: visible;
}
.checkbox {
  border-right: solid thin #cdd4e3;
  padding: 16px;
  display: flex;
  flex: 0;
  align-items: center;
}
.checkbox-label input {
  display: none;
}
.checkbox-label span {
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
  border: solid #cdd4e3;
  border-width: 0 thin thin thin;
}
.school-selected {
  background: #eef0f5;
}
.school-last {
  border-bottom-left-radius: 6px;
  border-bottom-right-radius: 6px;
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

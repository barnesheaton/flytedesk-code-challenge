<template>
  <div class="container">
    <div class="header">
      <div>
        <h2 class="header-text">School Data</h2>
        <p class="header-subtext">Manage and export data for schools below</p>
      </div>
      <button class="button-text" v-on:click="openModal">Add New School</button>
    </div>
    <div class="toolbar">
      <p class="item-selected">{{selectedSchools.length}} Selected</p>
      <button class="button-icon" v-on:click="deleteSelected">
        <font-awesome-icon icon="trash-alt" :style="{ color: '#788193' }" size="2x"/>
      </button>
      <button class="button-icon" v-on:click="exportCSV">
        <font-awesome-icon icon="file-download" :style="{ color: '#788193' }" size="2x"/>
      </button>
    </div>
    <div class="table-header">
      <div class="checkbox">
        <label class="checkbox-label">
          <input type="checkbox" v-model="checked" @change="toggleAll">
          <span></span>
        </label>
      </div>
      <div class="item">
        <p>School Name</p>
      </div>
      <div class="item">
        <p>City</p>
      </div>
      <div class="item">
        <p>State</p>
      </div>
      <div class="item">
        <p>Zip Code</p>
      </div>
      <div class="action">
        <p>Action</p>
      </div>
    </div>
    <div class="schools" style="overflow-y: scroll; height:400px;">
      <School
        v-bind:key="school.id"
        v-for="(school, index) in schools"
        v-bind:schoolData="school"
        v-bind:selected="selectedSchools.includes(school.id)"
        v-bind:isLastSchool="index + 1 === schools.length"
        v-on:onDelete="onDelete"
        v-on:onEdit="onEdit(school)"
        v-on:onToggle="onToggle"
      />
      <a href id="downloadlink" ref="downloadlink" hidden="true" download="export.csv">download</a>
    </div>
    <Modal
      v-show="isModalVisible"
      v-on:close="closeModal"
      v-on:onSave="onSave"
      v-bind:school="editSchool"
    />
  </div>
</template>

<script>
import School from "../components/School.vue";
import Modal from "../components/Modal.vue";
import colleges from "../assets/colleges.js";

export default {
  components: {
    Modal,
    School
  },
  data: function() {
    return {
      checked: false,
      editSchool: false,
      isModalVisible: false,
      schools: colleges.map((college, index) => ({
        ...college,
        id: index.toString()
      })),
      selectedSchools: []
    };
  },
  methods: {
    deleteSelected: function() {
      this.schools = this.schools.filter(
        school => !this.selectedSchools.includes(school.id)
      );
      this.selectedSchools = [];
    },
    exportCSV: function() {
      const data = this.schools
        .filter(school => this.selectedSchools.includes(school.id))
        .reduce((accumulator, school) => {
          return (
            accumulator +
            `${school.name.replace(/,/g, "at")}, ${school.city}, ${
              school.state
            }, ${school.zip}\n`
          );
        }, "");

      const encodedData = encodeURI("data:text/csv;charset=utf-8," + data);
      const link = this.$refs.downloadlink;
      link.setAttribute("href", encodedData);
      link.setAttribute("download", "export.csv");
      link.click();
    },
    onEdit: function(school) {
      this.editSchool = school;
      this.openModal();
    },
    onDelete: function(id) {
      this.schools = this.schools.filter(school => school.id !== id);
    },
    openModal: function(id) {
      this.isModalVisible = true;
    },
    closeModal: function(id) {
      this.editSchool = false;
      this.isModalVisible = false;
    },
    onSave: function(data) {
      if (this.editSchool) {
        this.schools[this.editSchool.id] = { ...data, id: this.editSchool.id };
      } else {
        this.schools.unshift({ ...data, id: this.schools.length });
      }
      this.closeModal();
    },
    onToggle: function(id) {
      if (this.selectedSchools.includes(id)) {
        this.selectedSchools = this.selectedSchools.filter(i => i !== id);
      } else {
        this.selectedSchools.push(id);
      }
    },
    toggleAll: function() {
      if (this.checked) {
        this.schools.map(school => {
          this.selectedSchools.push(school.id);
        });
      } else {
        this.selectedSchools = [];
      }
    }
  },
  name: "LandingView"
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.action {
  border-right: solid #cdd4e3;
  border-right-width: thin;
  text-align: center;
  width: 100px;
}
.button-icon {
  margin-left: 10px;
  width: 40px;
  height: 40px;
  border: solid #cdd4e3;
  border-width: thin;
  border-radius: 4px;
}
.button-text {
  background: #53bb77;
  border: none;
  border-radius: 4px;
  color: #ffffff;
  padding: 16px 32px;
}
.checkbox {
  border: solid #cdd4e3;
  border-width: 0 thin 0 thin;
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
.container {
  padding: 50px 40px;
}

.header {
  align-items: flex-start;
  display: flex;
  justify-content: space-between;
}
.header-text {
  font-weight: bolder;
  font-size: 24px;
  line-height: 24px;
  margin-bottom: 10px;
}
.header-subtext {
  margin-top: 0px;
  font-weight: normal;
  line-height: 14px;
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
.item-selected {
  font-weight: 400;
  color: #5faee1;
}
.table-header {
  overflow: hidden;
  display: flex;
  border: solid #cdd4e3;
  border-width: thin 0 thin;
  border-top-right-radius: 6px;
  border-top-left-radius: 6px;
  -webkit-box-shadow: 0 6px 4px -4px black;
  -moz-box-shadow: 0 6px 4px -4px black;
  box-shadow: 0 6px 4px -4px rgba(0, 0, 0, 0.2);
}
.toolbar {
  display: flex;
  justify-content: flex-end;
  margin: 20px 0 20px 0;
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
p {
  font-weight: bold;
}
</style>

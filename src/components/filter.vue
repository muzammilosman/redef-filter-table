<template>
  <div class="md-layout">
    <div class="md-layout-item md-size-100">
      <h2>FILTERS</h2>
      <md-button class="md-raised md-primary" @click="addFilter"> + ADD FILTER</md-button>
    </div>
    <div class="md-layout md-layout-item">
      <slot v-for="(condition, i) in conditions">
        <div class="md-layout md-layout-item md-size-100" :key="i">
          <div class="md-layout-item md-size-20" v-if="i != 0">
            <md-field>
              <label>Boolean</label>
              <md-select v-model="condition.linkLogic">
                <md-option value="&&">AND</md-option>
                <md-option value="||">OR</md-option>
              </md-select>
            </md-field>
          </div>
          <div class="md-layout-item md-size-20">
            <md-field>
              <label>Select Field</label>
              <md-select v-model="condition.id">
                <md-option
                  v-for="(item, i) in fields"
                  :value="item.value"
                  :key="i"
                  >{{ item.text }}</md-option
                >
              </md-select>
            </md-field>
          </div>
          <div class="md-layout-item md-size-20">
            <md-field>
              <label>Expression</label>
              <md-select
                v-model="condition.operator"
                v-if="condition.id != 'verified'"
              >
                <md-option
                  v-for="item in expressions"
                  :value="item.value"
                  :key="item.value"
                  >{{ item.text }}</md-option
                >
              </md-select>
              <md-select v-model="condition.operator" v-else>
                <md-option value="eq">EQUALS</md-option>
              </md-select>
            </md-field>
          </div>
          <div class="md-layout-item md-size-20">
            <md-field>
              <label>Value</label>
              <md-input
                v-if="condition.operator == 'contains'"
                v-model="condition.value"
                type="text"
              ></md-input>
              <md-select
                v-if="condition.id == 'verified'"
                v-model="condition.value"
              >
                <md-option :value="true">YES</md-option>
                <md-option :value="false">NO</md-option>
              </md-select>
              <md-input
                v-if="
                  condition.operator != 'contains' && condition.id != 'verified'
                "
                v-model="condition.value"
                type="number"
              ></md-input>
            </md-field>
          </div>

          <div class="md-layout-item item-size-20">
            <md-button class="md-accent md-raised" @click="deleteFilter(i)"
              >DELETE</md-button
            >
          </div>
        </div>
      </slot>
    </div>
    <div class="md-layout-item md-size-100">
        <user-table :conditions="conditions" />
    </div> 
  </div>
</template>

<script>
import UserTable from "./table.vue";

export default {
  name: "TableFilter",
  components: {
    UserTable
  },

  data() {
    return {
      fields: [
        { value: "name", text: "Name" },
        { value: "screen_name", text: "Screen Name" },
        { value: "followers_count", text: "Followers Count" },
        { value: "following_count", text: "Following Count" },
        { value: "location", text: "Location" },
        { value: "verified", text: "Verified" }
      ],
      expressions: [
        { text: ">=", value: "gte" },
        { text: "<=", value: "lte" },
        { text: "EQUALS", value: "eq" },
        { text: "CONTAINS", value: "contains" }
      ],
      conditions: []
    };
  },

  methods: {
    addFilter() {
      this.conditions.length != 0
        ? this.conditions.push({
            linkId: this.conditions.length - 1
          })
        : this.conditions.push({});
    },

    deleteFilter(index) {
      this.conditions.splice(index, 1);
    }
  }
};
</script>

<style></style>

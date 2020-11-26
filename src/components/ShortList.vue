<template>
  <div class="subcontainer">
    <div class="header secondary-header">
      Shorts
    </div>
    <div>
      <div v-if='success' class="success-message">
        {{ success }}
      </div>
      <div v-if='error' class="error-message">
        {{ error }}
      </div>

      <div class="shorts-container">  
        <div v-if="shorts.length">
          <ShortListItem
            v-for="short in shorts"
            v-bind:key="short.id"
            v-bind:short="short"
          />
        </div>
        <div v-else>
          <p style="text-align: center;">There are no shorts to display. Create one today!</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ShortListItem from "./ShortListItem";

import { eventBus } from "../main";

export default {
  name: "ShortList",

  components: { ShortListItem },

  data() {
    return {
      error: "",
      success: "",
      shorts: []
    };
  },

  created: function() {
    eventBus.$on("create-short-success", this.createShortHandler);
    eventBus.$on("update-short-success", this.updateShortHandler);
    eventBus.$on("delete-short-success", this.deleteShortHandler);
    eventBus.$on("update-short-error",this.updateShortErrorHandler);
    eventBus.$on("delete-short-error", this.deleteShortErrorHandler);
  },

  beforeDestroy: function () {
    eventBus.$off("create-short-success", this.createShortHandler);
    eventBus.$off("update-short-success", this.updateShortHandler);
    eventBus.$off("delete-short-success", this.deleteShortHandler);
    eventBus.$off("update-short-error",this.updateShortErrorHandler);
    eventBus.$off("delete-short-error", this.deleteShortErrorHandler);
  },

  mounted: function() {
    this.loadShorts();
  },

  methods: {
    loadShorts: function() {
      axios.get("/api/shorts").then(response => {
        this.shorts = response.data;
      });
    },
    clearMessages: function() {
      setInterval(() => {
        this.success = "";
        this.error = "";
      }, 5000);
    },
    createShortHandler: function(res) {
      this.shorts.push(res.data);
    },
    updateShortHandler: function(res) {
      this.success = `Short name ${res.data.shortName} now resolves to ${
        res.data.url
      }`;
      this.clearMessages();
      this.loadShorts();
    },
    deleteShortHandler: function(res) {
      this.success = `Short name ${res.shortName} has been deleted`;
      this.clearMessages();
      this.loadShorts();
    },
    updateShortErrorHandler: function(res) {
      this.error = `Error updating short ${res.data.shortName}`;
      this.clearMessages();
      this.loadShorts();
    },
    deleteShortErrorHandler: function(res) {
      this.error = `Error deleting short ${res.shortName}`;
      this.clearMessages();
      this.loadShorts();
    }
  }
};
</script>

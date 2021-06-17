<template>
  <div class="about">
    <label>Write a number:</label><br />
    <input type="number" v-model="searchQuery" /><br />

    <p :hidden="!autoSavingMsg" :class="{ hidden: this.invisibility }">
      <span>Auto-saving...</span>
    </p>
    <p :hidden="!savedMsg" :class="{ hidden: this.invisibility }">
      <span>Saved!!!</span>
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      autoSavingMsg: false,
      savedMsg: false,
      searchQuery: null,
      invisibility: false,
    };
  },
  watch: {
    searchQuery(val) {
      if (val === "") {
        this.invisibility = true;
        return;
      }
      this.savedMsg = false;
      if (!this.awaitingSearch) {
        setTimeout(() => {
          this.autoSavingMsg = true;
          setTimeout(() => {
            this.awaitingSearch = false;
            this.autoSavingMsg = false;
            this.savedMsg = true;
          }, 1000); // 1 second delay simulating API call and response
        }, 2000); // 2 seconds delay (waiting the user to press another number)
      }
      this.invisibility = false;
      this.autoSavingMsg = false;
      this.savedMsg = false;
    },
  },
};
</script>

<style>
.hidden {
  visibility: hidden;
}
</style>

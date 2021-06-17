<template>
  <div class="about">
    <h2>Challenge</h2>
    <p style="text-align: justify">
      We have a system where we need to implement auto-save when a user enters
      numeric quantities for an order and it should also indicate to the user
      when their data is saved. <br />
      It should not save after every keystroke since that would be too many
      calls to our backend. It should only save after the user hasn’t typed any
      number for 2 seconds. <br />
      For simulating the call to our server, it should simulate a 1 second
      delay. <br />
      Create a page that will have an edit field where the user will enter the
      number. After 2 seconds if the user hasn’t pressed any keys it should
      display on the page “Auto-saving”. After the 1 additional second where it
      simulates calling our server it should display “Saved”.
    </p>
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

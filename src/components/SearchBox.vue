<template>
  <div class="search-box">
    <input
      type="text"
      class="search-bar"
      placeholder="Search any city/town"
      :value="inputValue"
      @input="handleInput"
      @keydown.enter="onEnter"
    />
    <div v-if="showAutocomplete" class="autocomplete">
      <ul class="autocomplete-list">
        <li v-for="suggestion in suggestions" :key="suggestion">
          <button class="buttonStyle" @click="selectSuggestion(suggestion)">
            {{ suggestion }}
          </button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "SearchBox",
  props: {
    api_key: String,
    url_base: String,
  },
  data() {
    return {
      suggestions: [],
      showAutocomplete: false,
      inputValue: "", 
    };
  },
  methods: {
    async handleInput(event) {
      this.inputValue = event.target.value;

      const newQuery = this.inputValue;
      if (newQuery.length >= 2) {
        try {
          const response = await fetch(
            `${this.url_base}/search.json?key=${this.api_key}&q=${newQuery}`
          );
          const data = await response.json();
          this.suggestions = data.map(
            (item) => `${item.name}, ${item.country}`
          );
          this.showAutocomplete = true;
        } catch (error) {
          console.error("Error fetching suggestions:", error);
          this.suggestions = [];
          this.showAutocomplete = false;
        }
      } else {
        this.suggestions = [];
        this.showAutocomplete = false;
      }
    },
    selectSuggestion(selectedQuery) {
      this.$emit("selected-suggestion", selectedQuery);
      this.showAutocomplete = false;
      this.inputValue = "";
    },
    onEnter(event) {
      if (event.key === "Enter") {
        const query = this.inputValue;
        this.$emit("enter-pressed", query);
        this.showAutocomplete = false;
        this.inputValue = "";
      }
    },
  },
};
</script>
  
  <style>
.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.15);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.autocomplete ul.autocomplete-list {
  width: 100%;
  padding: 15px;
  list-style: none;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.15);
}

.buttonStyle {
  width: 100%;
  padding: 15px;
  text-align: left;

  color: #000000;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
}
</style>
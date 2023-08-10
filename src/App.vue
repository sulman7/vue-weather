<template>
  <div id="app" :class="backgroundClass">
    <main>
      <WeatherDetails 
      :query="query" 
      :weather="weather"
      @set-background="setBackground"
      />
    </main>
  </div>
</template>

<script>
import WeatherDetails from "./components/WeatherDetails.vue";

export default {
  name: "App",
  components: {
    WeatherDetails,
  },
  data() {
    return {
      whatBackground: "",
    };
  },
  computed: {
    backgroundClass() {
      return this.whatBackground;
    },
  },
  methods: {
    setBackground(howWarm) {
      this.whatBackground = howWarm;
    },
  },
};
</script>
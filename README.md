# Vue Weather App
This Vue.js weather app is a simple yet powerful tool to retrieve and display weather information for various locations. It utilizes the WeatherAPI to fetch real-time weather data and provides a user-friendly interface for users to interact with.

# Features
* Multiple Components: The app is structured using multiple Vue components to keep the codebase organized and modular.
* Emmiting and Listening with Events: The app effectively uses Vue's event system to communicate between parent and child components. Events like set-background are emitted and listened to, allowing seamless interaction and data exchange.
* Fetching Third-Party APIs: The app integrates the WeatherAPI to fetch current weather data based on user input. It makes asynchronous requests using the fetch API and handles responses using async/await.
* Reading JSON Data and Displaying: The app extracts relevant data from the fetched JSON response, including temperature, location, and weather conditions, and displays them in a user-friendly format.

# Components
* App.vue: This is the main component that serves as the entry point of the application. It uses the WeatherDetails component to display weather information and dynamically applies different background images based on weather conditions.
* WeatherDetails.vue: This component handles the display of weather details. It integrates the SearchBox component for user input and makes API calls to fetch weather data. It also includes methods to format the date and apply different CSS classes based on weather conditions.
* SearchBox.vue: This component provides a search input field that dynamically suggests locations based on user input. It communicates with the parent component using events like selected-suggestion and enter-pressed.

# Installation and Usage
1. Clone the repository: git clone https://github.com/sulman7/weather-app.git
2. Navigate to the project directory: cd weather-app
3. Install dependencies: npm install
4. Start the development server: npm run serve
5. Access the app in your browser at: http://localhost:8080

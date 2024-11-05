# weather-website
# HTML CODE

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather App</title>
    <link rel="shortcut icon" href="/cloudy.png" type="image/x-icon" />
    <link rel="stylesheet" href="/style.css">
    <script src="/script.js" defer></script>
    <script src="https://kit.fontawesome.com/1a48c47b52.js" crossorigin="anonymous" defer></script>
    <script src="https://cdn.jsdelivr.net/npm/luxon@2.0.2/build/global/luxon.min.js" defer></script>
</head>

<body>
    <div class="container">
        <div class="weather__header">
            <form class="weather__search"  method="get">
                <input type="text" placeholder="Search for a city" class="weather__searchform">
                <i class="fa-solid fa-magnifying-glass"></i>
            </form>
            <div class="weather__units">
                <span class="unit__celsisus">&deg;C</span>
                <span class="unit__fharenheit">&deg;F</span>
            </div>
        </div>
        <div class="weather__body">
            <h1 class="weather__city"></h1>
            <div class="weather__datetime">
                <p></p>
            </div>
            <div class="weather__forecast">
                <p>Cloudy</p>
            </div>
            <div class="weather__icon">
                <img src="https://th.bing.com/th/id/OIP.eAldPEWygo1WkjWbutXVZAAAAA?pid=ImgDet&w=161&h=161&c=7" alt="">
            </div>
            <p class="weather__temperature">
                18&deg
            </p>
            <div class="weather__minmax">
                <p>Min: 12&deg;</p>
                <p>Max: 16&deg;</p>
            </div>
        </div>
        <div class="weather__info">
            <div class="weather__card">
                <i class="fa-solid fa-temperature-three-quarters"></i>
                <div>
                    <p>Real Feel</p>
                    <p class="weather__realfeel">18&deg;</p>
                </div>
            </div>
            <div class="weather__card">
                <i class="fa-solid fa-droplet"></i>
                <div>
                    <p>Humidity</p>
                    <p class="weather__humidity">18&deg;</p>
                </div>
            </div>
            <div class="weather__card">
                <i class="fa-solid fa-wind"></i>
                <div>
                    <p>Wind</p>
                    <p class="weather__wind">18&deg;</p>
                </div>
            </div>
            <div class="weather__card">
                <i class="fa-solid fa-stopwatch"></i>
                <div>
                    <p>Pressure</p>
                    <p class="weather__pressure">18&deg;</p>
                </div>
            </div>
        </div>
    </div>
</body>

</html>


# JAVA SCRIPT CODE

let currentCity = "Hyderabad";
let units = "metric";

let city = document.querySelector(".weather__city");
let datetime = document.querySelector(".weather__datetime");
let weather__forecast = document.querySelector(".weather__forecast");
let weather__temperature = document.querySelector(".weather__temperature");
let weather__icon = document.querySelector(".weather__icon");
let weather__minmax = document.querySelector(".weather__minmax");
let weather__realfeel = document.querySelector(".weather__realfeel");
let weather__humidity = document.querySelector(".weather__humidity");
let weather__wind = document.querySelector(".weather__wind");
let weather__pressure = document.querySelector(".weather__pressure");
let weather__search = document.querySelector(".weather__search");

weather__search.addEventListener("submit", (e) => {
  let search = document.querySelector(".weather__searchform");
  e.preventDefault();
  currentCity = search.value;
  getWeather();
  search.value = "";
});

document.querySelector(".unit__celsisus").addEventListener("click", () => {
  if (units !== "metric") {
    units = "metric";

    getWeather();
  }
});

document.querySelector(".unit__fharenheit").addEventListener("click", () => {
  if (units !== "imperial") {
    units = "imperial";

    getWeather();
  }
});

function convertCountryCode(country) {
  let regionName = new Intl.DisplayNames(["en"], { type: "region" });
  return regionName.of(country);
}

function convertTimestamp(timestamp, timezone) {
  const convertTimeZone = timezone / 3600;

  const date = new Date(timestamp * 1000);

  const options = {
    weekday: "long",
    day: "numeric",
    month: "long",
    year: "numeric",
    hour: "numeric",
    minute: "numeric",
    timeZOne: `Etc/GMT${convertTimeZone >= 0 ? "-" : "+"}${Math.abs(
      convertTimeZone
    )}`,
    hour12: true,
  };
  return date.toLocaleString(options);
}
function getWeather() {
  const API_KEY = "1d9f78e30b2044005ce9938e00c57fb2";
  fetch(
    `https://api.openweathermap.org/data/2.5/weather?q=${currentCity}&appid=${API_KEY}&units=${units}`
  )
    .then((res) => res.json())
    .then((data) => {
      city.innerHTML = `${data.name}, ${convertCountryCode(data.sys.country)}`;
      datetime.innerHTML = convertTimestamp(data.dt, data.timezone);
      weather__forecast.innerHTML = `<p>${data.weather[0].main}`;
      weather__temperature.innerHTML = `${data.main.temp.toFixed()}&deg`;
      weather__icon.innerHTML = `<img src="https://openweathermap.org/img/wn/${data.weather[0].icon}@4x.png" alt="">
      `;
      weather__minmax.innerHTML = `<p>Min: ${data.main.temp_min.toFixed()}&deg</p>
      <p>Max: ${data.main.temp_max.toFixed()}&deg</p>`;
      weather__realfeel.innerHTML = `${data.main.feels_like.toFixed()}&deg`;
      weather__humidity.innerHTML = `${data.main.humidity.toFixed()}%`;
      weather__wind.innerHTML = `${data.wind.speed}${
        units === "imperial" ? "mph" : "m/s"
      }`;
      weather__pressure.innerHTML = `${data.main.pressure}hPA`;
    });
}

document.body.addEventListener("load", getWeather());


# CSS CODE

@import url("https://fonts.googleapis.com/css2?family=Poppins&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Poppins", sans-serif;
}

.container {
  background: rgb(14, 13, 13);
  color: #ffffff;
  padding: 2rem;
  width: 40%;
  margin: 4rem auto;
  border-radius: 10px;
  box-shadow: -5px 5px 5px #0000002c;
}

.weather__header {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

input {
  border: none;
  background-color: #292727;
  outline: none;
  padding: 0.5rem 2.5rem;
  border-radius: 20px;
  color: #ffffff;
  font: inherit;
}

input::placeholder {
  color: #ffffff;
}

input:hover {
  background-color: #312d2d;
}

.weather__search {
  position: relative;
}

.weather__search i {
  position: absolute;
  left: 10px;
  top: 10px;
  font-size: 20px;
  color: white;
}

.weather__units {
  font-size: 1.5rem;
}

.weather__units span {
  cursor: pointer;
}

.weather__units span:first-child {
  margin-right: 0.5rem;
}

.weather__body {
  text-align: center;
  margin-top: 3rem;
}


.weather__forecast {
  margin-top: 2rem;
  font-size: 18px;
  background: black;
  display: inline-block;
  padding: 0.25rem 0.75rem;
  border-radius: 20px;
}

.weather__icon img {
  width: 100px;
}

.weather__temperature {
  font-size: 2rem;
}

.weather__minmax {
  display: flex;
  justify-content: center;
}

.weather__minmax p {
  margin: 1rem;
}

.weather__info {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 1rem;
  margin-top: 2rem;
}

.weather__card {
  display: flex;
  align-items: center;
  background: black;
  padding: 1rem;
  border-radius: 10px;
}

.weather__card i {
  font-size: 1.5rem;
  margin-right: 1rem;
}

@media (max-width: 1060px) {
  .container {
    width: 6
    0%;
  }
}

@media (max-width: 936px) {
  .container {
    width: 90%;
  }

  .weather__header {
    flex-direction: column;
  }

  .weather__units {
    margin-top: 1rem;
  }
}

@media (max-width: 400px) {
  .weather__icon {
    grid-template-columns: none;
  }
}


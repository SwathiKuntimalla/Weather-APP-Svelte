<script>
	let city = "";
	let weatherData = null;
	let isError = false;


	const apikey = "aa7685502a8106474df34adddafc9ffa";

	// // for getting the data from of weather
	async function getWeatherData() {
		try {
			const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apikey}`;
			const response = await fetch(apiUrl);
			const data = await response.json();

			if (!response.ok) {
				isError = true;
			} else {
				isError = false;
			}

			weatherData = data;
			console.log(weatherData, "weatherData");
		} catch (error) {
			console.log(error);
			isError = true;
			weatherData = { message: error };
		}
	}
	function getWeatherEmoji(weatherId){
		switch(true){
			case (weatherId >= 200 && weatherId < 300):
            return "⛈️";
        case (weatherId >= 300 && weatherId < 400):
            return "🌧️";
        case (weatherId >= 500 && weatherId < 600):
            return "🌦️";
        case (weatherId >= 600 && weatherId < 700):
            return "☃️";
        case (weatherId >= 700 && weatherId < 800):
            return "🌨️";
        case (weatherId === 800):
            return "☀️";
        case (weatherId >= 801 && weatherId < 810):
            return "🌥️";
        default:
            return "❓"

		}
	}
	
</script>

<body>
	<form on:submit|preventDefault={getWeatherData} class="displayform">
		<input
			type="text"
			placeholder="Enter the city name"
			class="cityInput"
			bind:value={city}
			required
		/>
		<button type="submit" value="Get Weather" class="button">
			Get Weather
		</button>
	</form>
	{#if isError === false && weatherData}
		<div class="card">
			<h1>{weatherData?.name}</h1>
			<p class="tempDisplay">{(weatherData.main?.temp-273.15).toFixed(1)}°C</p>
			<p class="humidityDisplay">{weatherData.main?.humidity}%</p>
			<p class="weatherEmoji">{getWeatherEmoji(weatherData?.weather[0]?.id)}</p>
			<p>{weatherData?.weather[0]?.description}</p>
		</div>
	{:else if weatherData}
		<div class="card">
			<p>{weatherData?.message}</p>
		</div>
	{/if}
</body>

<style> 
	
	.displayform {
		text-align: center;
		margin-top: 20px;
	}
	.cityInput {
		font-weight: 200;
		border-radius: 10px;
		border-color: hwb(42 27% 60%);
		border-width: 2px;
		width: 20%;
	}
	.button {
		background-color: hsla(273, 84%, 55%, 0.669);
		font-weight: 500;
		border-radius: 10px;
		text-align: center;
		color: aliceblue;
		margin-left: 10px;
		cursor: pointer;
	}
	button:hover {
		background-color: hsla(273, 28%, 40%, 0.669);
	}

	.card {
		background: linear-gradient(
			180deg,
			hsl(129, 38%, 68%),
			hsla(40, 100%, 75%, 0.253)
		);
		padding: 70px;
		box-shadow: 2px 2px 5px hsla(0, 15%, 15%, 0.4);
		display: flex;
		flex-direction: column;
		align-items: center;
		border-radius: 15px;
		width: 30%;
		margin: auto;
	}
	body {
		background: hwb(187 76% 8%);
	}
	.displayform {
		margin: 25px;
	}
	.cityDisplay {
		margin-top: 0;
		margin-bottom: 1px;
		font-weight: bold;
		font-size: 2.3rem;
		color: rgb(81, 38, 8);
		padding-top: 0;
	}
	.tempDisplay {
		font-size: 1.5rem;
	}
	.humidityDisplay {
		font-size: 1.2rem;
	}
	.descDisplay {
		font-style: italic;
		font-weight: bold;
		font-size: 1.5rem;
	}
	.weatherEmoji {
		margin: 0;
		font-size: 2rem;
	}
	.errorDisplay {
		margin: 0;
		font-size: 20px;
		font-style: oblique;
	}
</style>

<!DOCTYPE html>
<html>
<head>
  <title>今日の天気</title>
</head>
<body>
  <h1>主要都市の今日の天気</h1>
  <div id="weather-info"></div>

  <script>
    // OpenWeatherMap APIキー
    const apiKey = 'YOUR_API_KEY';

    // 主要都市の情報（都市名と対応する都市ID）
    const cities = [
      { name: '東京', id: 1850147 },
      { name: 'ニューヨーク', id: 5128581 },
      { name: 'ロンドン', id: 2643743 },
      { name: 'パリ', id: 2988507 },
    ];

    // 都市の天気情報を取得する関数
    async function getWeather(cityId) {
      try {
        const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?id=${cityId}&appid=${apiKey}`);
        const data = await response.json();

        const cityName = data.name;
        const weatherDescription = data.weather[0].description;
        const temperature = (data.main.temp - 273.15).toFixed(1); // ケルビンから摂氏に変換

        return `${cityName}: ${weatherDescription}, ${temperature}°C`;
      } catch (error) {
        console.error('天気情報の取得に失敗しました', error);
        return '天気情報を取得できませんでした';
      }
    }

    // 主要都市の天気情報を表示
    async function displayWeatherInfo() {
      const weatherInfoElement = document.getElementById('weather-info');

      for (const city of cities) {
        const weather = await getWeather(city.id);
        const weatherElement = document.createElement('p');
        weatherElement.textContent = weather;
        weatherInfoElement.appendChild(weatherElement);
      }
    }

    displayWeatherInfo();
  </script>
</body>
</html>

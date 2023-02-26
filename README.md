
<!DOCTYPE HTML><html>
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" href="data:,">
  <title>ESP(LoRa + Server)</title>
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
  <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.7.2/css/all.css" integrity="sha384-fnmOCqbTlWIlj8LyTjo7mOUStjsKC4pOpQbqyi7RrhN7udi9RwhKkMHpvLbHG9Sr" crossorigin="anonymous">
  <style>

  body {
    margin: 0;
    font-family: Arial, Helvetica, sans-serif;
    text-align: center;
  }
  header {
    margin: 0;
    padding-top: 5vh;
    padding-bottom: 5vh;
    overflow: hidden;
    background-image: url(mainimage);
    background-size: cover;
    color: white;
  }
  h2 {
    font-size: 2.0rem;
  }
  p { font-size: 1.2rem; }
  .units { font-size: 1.2rem; }
  .readings { font-size: 2.0rem; }
  </style>
</head> 
<body>
  <!-- <header> -->
    <h2>LoRa GATEWAY</h2>
    <p><strong> <br/><span id="timestamp">%TIMESTAMP%</span></strong></p>
    <!-- <p>LoRa RSSI:<span id="rssi1">%RSSI%</span></p> -->
  <!-- </header> -->
  <!-- <main> -->
  <p>
    <i class="fas fa-thermometer-half" style="color:#e62802;">
    </i> Temperature: <span id="temperature" class="readings">%TEMPERATURE%</span>
    <sup>&deg;C</sup>
  </p>
  <p>
    <i class="fas fa-tint" style="color:#04b6c9;">
    </i> Humidity: <span id="humidity" class="readings">%HUMIDITY%</span>
    <sup>&#37;</sup>
  </p>
  <p>
    <i class="fa fa-leaf" style="color:#00b52d;">
    </i> Moisture: <span id="moisture" class="readings">%MOISTURE%</span>
    <sup>&#37;</sup>
  </p>
  <p>
	<i class="fa-solid fa-bolt" style="color:#00b;">
    </i> PIR Count <span id="svalue" class="readings">%SVALUE%</span>
    <sup>&#37;</sup>
  </p>
  <!-- </main> -->
<script>
$.getJSON("bph_prediction.json", function(model) {
			var parsedModel = JSON.parse(model);
			function predict(){
        alert("function called")
				var temperature = parseFloat($("#temperature").val());
				var humidity = parseFloat($("#humidity").val());
				var moisture = parseFloat($("#moisture").val());
				var ir_value = parseFloat($("#svalue").val());
        alert(temperature,humidity,moisture,ir_value)
				var prediction = parsedModel.predict([temperature,humidity,moisture,ir_value]);
				$("#output").text("Prediction: " + prediction);
			}
});

setInterval(updateValues, 1, "temperature");
setInterval(updateValues, 1, "humidity");
setInterval(updateValues, 1, "moisture");
setInterval(updateValues, 1, "rssi");
setInterval(updateValues, 1, "timestamp");
setInterval(updateValues, 1, "svalue");

function updateValues(value) {
  var xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
      document.getElementById(value).innerHTML = this.responseText;
    }
  };
  xhttp.open("GET", "/" + value, true);
  xhttp.send();
}
</script>
</body>
</html>

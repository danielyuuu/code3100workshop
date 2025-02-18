#Task 1
Use this prompt to generate files for a Live Bus tracker App.
---------
Build a Flask-based web dashboard that displays live bus locations using the Transport NSW API and Mapbox. The dashboard should visualize the live bus locations as a heatmap on the map, with live data being fetched and updated every 4 seconds.

The project structure is as follows:
Project Structure:
/app

/static/js/script.js (This file will handle Mapbox heatmap visualization and dynamic data fetching.)

/templates/index.html (This file will be the HTML template for the frontend, using Mapbox for the heatmap display.)

locations.geojson (This will be an empty GeoJSON file for storing live bus locations, which will be updated by the Flask backend.)

app.py (This will be the Flask backend that fetches data from the Transport NSW API and saves the bus locations.)

Details:

Flask Backend (app.py):

Use the gtfs-realtime-bindings package to parse protocol buffer (protobuf) data from the Transport NSW API.
Specifically, import gtfs_realtime_pb2 from google.transit

The Transport NSW API endpoint is: https://api.transport.nsw.gov.au/v1/gtfs/vehiclepos/buses.
Use the API Key: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJqdGkiOiJ0TkV2czEyU0tVTFdjV1JhWXhkYVR6YXEzVzBqb0YzUWFRbENCZEdyd0lnIiwiaWF0IjoxNzI3NjE2NjI3fQ.QyQJxrXm4HDf_UOZmcpi06KTcSr0bQtIuQqF6kIzPs8.

Fetch live bus locations from the Transport NSW API every 4 seconds, parsing latitude and longitude using gtfs_realtime_pb2.
Save the bus location data in GeoJSON format to locations.geojson.
Continuously fetch bus locations using a while True loop in a separate background thread using threading.Thread.
Serve the GeoJSON file via a Flask route /locations.
Serve the frontend via the route /, which renders the Mapbox heatmap.

Frontend (HTML and JS):

Use Mapbox to display live bus locations as a heatmap.
Fetch the GeoJSON data from the /locations endpoint every 4 seconds and update the heatmap dynamically.
Use the custom Mapbox style mapbox://styles/mapbox/dark-v10.
Adjust the heatmap radius based on the map's zoom level.
The Mapbox token to use is: pk.eyJ1IjoiZGFuaWVseXUiLCJhIjoiY20xdTZmdzlwMDlvaTJrb3FoNG10d2ZrcSJ9.gKodiYemF0wrGbtixm3L1g.

Files to Generate:

app.py: The Flask backend, which fetches bus locations, saves them to locations.geojson, and serves the file to the frontend. Be sure to use from google.transit import gtfs_realtime_pb2 for parsing the protobuf data from the API.
index.html: The HTML template that integrates Mapbox and displays the heatmap of live bus locations.
script.js: JavaScript that handles fetching the GeoJSON data every 4 seconds and updating the Mapbox heatmap dynamically.
locations.geojson: An initially empty GeoJSON file that will store live bus location data.
Pip Packages: Please provide a list of all necessary pip install packages, including:

Flask
gtfs-realtime-bindings (for parsing protocol buffer data)
Any additional packages necessary for the app to run

Generate the full contents of the app.py, index.html, script.js, and locations.geojson files based on these details, and list the required pip packages for the project.

------
#Task 2
Take the code below and develop a backend script (python) to save the parameters to a local config file (any type).
------
```html
<!DOCTYPE html>

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern Soft UI - Sliders & Parameters</title>
    <style>
        /* Global Styling */
        body {
            font-family: 'Arial', sans-serif;
            background: #f9f9f9;
            color: #333;
            text-align: center;
            margin: 0;
            padding: 40px;
        }
        h2 {
            background: linear-gradient(45deg, #ff9a9e, #fad0c4);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            font-size: 2rem;
            margin-bottom: 20px;
        }

        /* Input Container */
        .container {
            background: linear-gradient(135deg, #ffffff, #f3f3f3);
            padding: 20px;
            margin: 15px auto;
            width: 60%;
            border-radius: 12px;
            box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.1);
            border: 1px solid #ddd;
        }

        label {
            font-size: 1.1rem;
            font-weight: bold;
            color: #666;
            display: block;
            margin-bottom: 8px;
        }

        /* Inputs Styling */
        input, select {
            width: 80%;
            padding: 12px;
            font-size: 1rem;
            border: 2px solid #ff9a9e;
            background-color: #fff;
            color: #333;
            border-radius: 8px;
            outline: none;
            transition: 0.3s ease;
            box-shadow: inset 3px 3px 6px rgba(0, 0, 0, 0.05);
        }

        input:hover, select:hover {
            border-color: #fa709a;
            box-shadow: 0 0 10px rgba(255, 154, 158, 0.3);
        }

        input[type="checkbox"] {
            width: 20px;
            height: 20px;
            cursor: pointer;
            accent-color: #fa709a;
        }

        /* Slider Styling */
        input[type="range"] {
            -webkit-appearance: none;
            width: 80%;
            height: 8px;
            border-radius: 5px;
            background: linear-gradient(90deg, #ff9a9e, #fad0c4);
            outline: none;
            transition: background 0.3s;
        }

        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: #fff;
            border: 2px solid #fa709a;
            cursor: pointer;
        }

        /* Button Styling */
        .button-container {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 20px;
        }

        button {
            background: linear-gradient(45deg, #ff9a9e, #fad0c4);
            color: #fff;
            font-size: 1rem;
            padding: 12px 18px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.3s ease;
            box-shadow: 3px 3px 10px rgba(255, 154, 158, 0.3);
        }

        button:hover {
            background: linear-gradient(45deg, #fa709a, #ff9a9e);
            box-shadow: 0 0 15px rgba(255, 154, 158, 0.5);
        }

    </style>
</head>
<body>

<h2>Soft UI - Interactive Sliders & Parameters</h2>

<!-- Range Slider -->
<div class="container">
    <label for="rangeSlider">Range Slider (0-100):</label>
    <input type="range" id="rangeSlider" min="0" max="100" value="50" oninput="updateValue('rangeValue', this.value)">
    <span id="rangeValue">50</span>
</div>

<!-- Dropdown Selector -->
<div class="container">
    <label for="dropdownSelector">Dropdown Selector:</label>
    <select id="dropdownSelector" onchange="updateValue('dropdownValue', this.value)">
        <option value="Option 1">Option 1</option>
        <option value="Option 2">Option 2</option>
        <option value="Option 3">Option 3</option>
    </select>
    <span id="dropdownValue">Option 1</span>
</div>

<!-- Checkbox Toggle -->
<div class="container">
    <label for="toggleCheckbox">Checkbox Toggle:</label>
    <input type="checkbox" id="toggleCheckbox" onchange="updateValue('checkboxValue', this.checked ? 'Checked' : 'Unchecked')">
    <span id="checkboxValue">Unchecked</span>
</div>

<!-- Text Input -->
<div class="container">
    <label for="textInput">Text Input:</label>
    <input type="text" id="textInput" placeholder="Enter text..." oninput="updateValue('textValue', this.value)">
    <span id="textValue"></span>
</div>

<!-- Number Input -->
<div class="container">
    <label for="numberInput">Number Input (0-50):</label>
    <input type="number" id="numberInput" min="0" max="50" value="25" oninput="updateValue('numberValue', this.value)">
    <span id="numberValue">25</span>
</div>

<!-- Centered Buttons -->
<div class="button-container">
    <button onclick="resetFields()">Reset</button>
    <button onclick="submitForm()">Submit</button>
</div>

<script>
    function updateValue(id, value) {
        document.getElementById(id).innerText = value;
    }

    function resetFields() {
        document.getElementById('rangeSlider').value = 50;
        document.getElementById('dropdownSelector').value = "Option 1";
        document.getElementById('toggleCheckbox').checked = false;
        document.getElementById('textInput').value = "";
        document.getElementById('numberInput').value = 25;

        updateValue('rangeValue', 50);
        updateValue('dropdownValue', "Option 1");
        updateValue('checkboxValue', "Unchecked");
        updateValue('textValue', "");
        updateValue('numberValue', 25);
    }

    function submitForm() {
        alert("Form Submitted! 🎉");
    }
</script>

</body>
</html>
```
------
# Task 3
Develop a dynamic visualisation page to view the 3D file (stl) below)
------
Shoei Yoh, Oguni Bus Station - https://github.com/danielyuuu/code3100workshop/blob/main/bus-station_cleaned_90radius_rev02.stl



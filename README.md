#Task 1
Use this prompt to generate files for a Live Bus tracker App.

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


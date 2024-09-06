# Smart-Travel-Planner-Route-Optimizer/

import os
import requests
from flask import Flask, request, jsonify, render_template

app = Flask(__name__)

# Store the API keys
TRIPADVISOR_API_KEY = "D9D9D66A1105428DA7B115B31A01546C"
GOOGLE_MAPS_API_KEY = "AIzaSyAffeEVNd8vpXVYoT1FAdwInQdcGPkfFJ0"

# Function to get recommendations from TripAdvisor
def get_recommendations(destination_type, budget, time, travel_preferences):
    api_url = 'https://api.tripadvisor.com/recommendations'  # Replace with actual TripAdvisor endpoint
    params = {
        'key': TRIPADVISOR_API_KEY,
        'destination_type': destination_type,
        'budget': budget,
        'time': time,
        'travel_preferences': travel_preferences
    }
    response = requests.get(api_url, params=params)
    return response.json()

# Function to get route from Google Maps
def get_route(start_location, end_location, waypoints, mode='driving'):
    api_url = 'https://maps.googleapis.com/maps/api/directions/json'
    params = {
        'origin': start_location,
        'destination': end_location,
        'waypoints': '|'.join(waypoints),
        'mode': mode,
        'key': GOOGLE_MAPS_API_KEY
    }
    response = requests.get(api_url, params=params)
    return response.json()

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/recommend', methods=['GET'])
def recommend():
    destination_type = request.args.get('destination_type')
    budget = request.args.get('budget')
    time = request.args.get('time')
    travel_preferences = request.args.get('travel_preferences')

    recommendations = get_recommendations(destination_type, budget, time, travel_preferences)
    return jsonify(recommendations)

@app.route('/route', methods=['GET'])
def route():
    start_location = request.args.get('start_location')
    end_location = request.args.get('end_location')
    waypoints = request.args.getlist('waypoints')
    mode = request.args.get('mode', 'driving')

    route_info = get_route(start_location, end_location, waypoints, mode)
    return jsonify(route_info)

if __name__ == '__main__':
    app.run(debug=True)

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Travel Planner</title>
</head>
<body>
    <h1>Smart Travel Planner</h1>
    <form action="/recommend" method="get">
        <label for="destination_type">Destination Type:</label>
        <input type="text" id="destination_type" name="destination_type" placeholder="beach, action, pool"><br><br>
        <label for="budget">Budget:</label>
        <input type="number" id="budget" name="budget"><br><br>
        <label for="time">Time (days):</label>
        <input type="number" id="time" name="time"><br><br>
        <label for="travel_preferences">Travel Preferences:</label>
        <input type="text" id="travel_preferences" name="travel_preferences" placeholder="car, scooter, flying"><br><br>
        <input type="submit" value="Get Recommendations">
    </form>

    <h1>Optimize Route</h1>
    <form action="/route" method="get">
        <label for="start_location">Start Location:</label>
        <input type="text" id="start_location" name="start_location" placeholder="New York, NY"><br><br>
        <label for="end_location">End Location:</label>
        <input type="text" id="end_location" name="end_location" placeholder="San Francisco, CA"><br><br>
        <label for="waypoints">Waypoints (Separate by commas):</label>
        <input type="text" id="waypoints" name="waypoints" placeholder="Chicago, IL, Denver, CO"><br><br>
        <label for="mode">Travel Mode (driving, walking, bicycling):</label>
        <input type="text" id="mode" name="mode" value="driving"><br><br>
        <input type="submit" value="Get Route">
    </form>
</body>
</html>


# Smart Travel Planner and Route Optimizer

## Summary
This project is an AI-based travel planner that recommends destinations based on user preferences such as budget, time, and travel preferences. It also provides optimized routes between selected destinations.

## Features
- **Destination Recommendations**: Based on budget, vacation type, and time.
- **Route Optimization**: Provides a travel route with waypoints and mode of travel.

## Setup
1. Clone this repository.
2. Install dependencies using: `pip install -r requirements.txt`
3. Run the Flask app: `python app.py`
4. Access the app in your browser at `http://127.0.0.1:5000`

## License
MIT License

Flask==2.0.1

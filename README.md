# Smart-Travel-Planner-Route-Optimizer/

import os
import requests
from flask import Flask, request, jsonify, render_template

app = Flask(__name__)

# Fetch API keys from environment variables
TRIPADVISOR_API_KEY = os.getenv('TRIPADVISOR_API_KEY')
GOOGLE_MAPS_API_KEY = os.getenv('GOOGLE_MAPS_API_KEY')

def get_recommendations(destination_type, budget, time, travel_preferences):
    api_url = 'https://api.tripadvisor.com/recommendations'
    params = {
        'key': TRIPADVISOR_API_KEY,
        'destination_type': destination_type,
        'budget': budget,
        'time': time,
        'travel_preferences': travel_preferences
    }
    response = requests.get(api_url, params=params)
    return response.json()

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

# Flask routes
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

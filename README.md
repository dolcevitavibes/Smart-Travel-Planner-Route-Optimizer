# Smart-Travel-Planner-Route-Optimizer
# An AI-based system for recommending travel destinations and optimizing routes for efficient travel planning.

# RECOMMENDATION LOGIC

import requests

def get_recommendations(destination_type, budget, time, travel_preferences):
    # Example API URL (replace with actual API endpoint and parameters)
    api_url = 'https://api.example.com/recommendations'
    params = {
        'destination_type': destination_type,
        'budget': budget,
        'time': time,
        'travel_preferences': travel_preferences
    }
    response = requests.get(api_url, params=params)
    recommendations = response.json()
    return recommendations

# Example usage
destinations = get_recommendations('beach', 1000, 7, 'car')
print(destinations)

# ROUTE OPTIMIZATION

import requests

def get_route(start_location, end_location, waypoints, mode='driving'):
    api_key = 'YOUR_GOOGLE_MAPS_API_KEY'
    api_url = 'https://maps.googleapis.com/maps/api/directions/json'
    params = {
        'origin': start_location,
        'destination': end_location,
        'waypoints': '|'.join(waypoints),
        'mode': mode,
        'key': api_key
    }
    response = requests.get(api_url, params=params)
    route = response.json()
    return route

# Example usage
route = get_route('New York, NY', 'San Francisco, CA', ['Chicago, IL', 'Denver, CO'])
print(route)

# INTERFACE WITH FLASK

from flask import Flask, request, jsonify

app = Flask(__name__)

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
    route = get_route(start_location, end_location, waypoints)
    return jsonify(route)

if __name__ == '__main__':
    app.run(debug=True)

# HMTL FORM FOR USER INPUT

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Travel Planner</title>
</head>
<body>
    <h1>Travel Planner</h1>
    <form action="/recommend" method="get">
        <label for="destination_type">Destination Type:</label>
        <input type="text" id="destination_type" name="destination_type"><br><br>
        <label for="budget">Budget:</label>
        <input type="number" id="budget" name="budget"><br><br>
        <label for="time">Time (days):</label>
        <input type="number" id="time" name="time"><br><br>
        <label for="travel_preferences">Travel Preferences:</label>
        <input type="text" id="travel_preferences" name="travel_preferences"><br><br>
        <input type="submit" value="Get Recommendations">
    </form>
</body>
</html>

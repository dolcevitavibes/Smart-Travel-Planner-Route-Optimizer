Smart-Travel-Planner-Route-Optimizer/
│
├── app.py                # Flask application
├── mock_data.py          # File to hold mock data
├── templates/
│   └── index.html        # HTML form for user input
├── README.md             # Project description
├── requirements.txt      # List of dependencies (Flask, etc.)

# app.py

from flask import Flask, request, jsonify, render_template
from mock_data import get_mock_recommendations, get_mock_route

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/recommend', methods=['GET'])
def recommend():
    destination_type = request.args.get('destination_type')
    budget = int(request.args.get('budget'))
    time = int(request.args.get('time'))
    travel_preferences = request.args.get('travel_preferences')
    
    recommendations = get_mock_recommendations(destination_type, budget, time, travel_preferences)
    return jsonify(recommendations)

@app.route('/route', methods=['GET'])
def route():
    start_location = request.args.get('start_location')
    end_location = request.args.get('end_location')
    waypoints = request.args.getlist('waypoints')
    mode = request.args.get('mode', 'driving')
    
    route = get_mock_route(start_location, end_location, waypoints, mode)
    return jsonify(route)

if __name__ == '__main__':
    app.run(debug=True)

# mock_data.py

def get_mock_recommendations(destination_type, budget, time, travel_preferences):
    # Placeholder mock data
    recommendations = [
        {'name': 'Beach Resort', 'location': 'Malibu, CA', 'rating': 4.7, 'budget': 1200, 'days': 5, 'type': 'beach'},
        {'name': 'Mountain Adventure', 'location': 'Aspen, CO', 'rating': 4.5, 'budget': 800, 'days': 7, 'type': 'action'},
        {'name': 'City Break', 'location': 'New York, NY', 'rating': 4.8, 'budget': 1000, 'days': 3, 'type': 'mix'}
    ]
    
    # Return filtered recommendations based on input criteria
    return [rec for rec in recommendations if rec['type'] == destination_type and rec['budget'] <= budget]

def get_mock_route(start_location, end_location, waypoints, mode='driving'):
    # Placeholder mock route
    route = {
        'start': start_location,
        'end': end_location,
        'waypoints': waypoints,
        'distance': '300 miles',
        'estimated_time': '5 hours',
        'mode': mode
    }
    return route

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Travel Planner</title>
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

    <h2>Get Travel Route</h2>
    <form action="/route" method="get">
        <label for="start_location">Start Location:</label>
        <input type="text" id="start_location" name="start_location"><br><br>
        <label for="end_location">End Location:</label>
        <input type="text" id="end_location" name="end_location"><br><br>
        <label for="waypoints">Waypoints (comma separated):</label>
        <input type="text" id="waypoints" name="waypoints"><br><br>
        <label for="mode">Mode of Travel:</label>
        <input type="text" id="mode" name="mode" value="driving"><br><br>
        <input type="submit" value="Get Route">
    </form>
</body>
</html>

# Smart Travel Planner and Route Optimizer

## Summary
This project is an AI-based travel planner that recommends destinations based on user preferences such as budget, time, and travel preferences. It also provides optimized routes between selected destinations using mock data.

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

Overview:
This repository implements a simple home‑price estimator for Bangalore (Bengaluru) real‑estate.
A linear regression model trained on historic listings predicts the price (in Lakhs ₹) from four inputs:

Feature	Description
total_sqft:	Area of the house in square feet
bhk:	Number of bedrooms
bath:	Number of bathrooms
location: Neighborhood (e.g., Electronic City, Rajaji Nagar)

The model, data columns, and a list of supported locations are stored as pickled / JSON artifacts under server/artifacts.
A lightweight Flask API (server/server.py) exposes two endpoints:

/predict_home_price – POST request returns the estimated price.
/get_location_names – GET request returns the list of locations for the UI dropdown.
The client (client/app.html, client/app.js, client/app.css) is a pure HTML page that:

Dynamically loads the location list from the server.
Lets the user input the four features.
Sends an AJAX request to the prediction endpoint and displays the result with a smooth animation.
All code is written in pure Python, JavaScript, HTML & CSS – no heavy front‑end frameworks, making the project easy to run on any machine.
The client (client/app.html, client/app.js, client/app.css) is a pure HTML page that:

Dynamically loads the location list from the server.
Lets the user input the four features.
Sends an AJAX request to the prediction endpoint and displays the result with a smooth animation.
All code is written in pure Python, JavaScript, HTML & CSS – no heavy front‑end frameworks, making the project easy to run on any machine.



Features:
Linear‑Regression model trained on a curated Bangalore dataset.
RESTful Flask API with CORS headers for easy cross‑origin use.
Dynamic UI – location dropdown populated from the server, micro‑animations for button clicks and result display.
Modular utilities (server/util.py) that load artifacts once at startup and cache them in memory.
Zero‑dependency front‑end – only jQuery (CDN) is used for concise AJAX handling.


Running the Application:

1. Start the Flask server
   python server/server.py
   The server loads the model and location list once at startup (util.load_saved_artifacts()).

2. Open the client
   Open client/app.html in any modern browser (Chrome, Edge, Firefox).
  No additional web server is required because the page talks to the Flask API via CORS‑enabled AJAX calls.

3. Use the UI
   Choose a location from the dropdown (populated automatically).
  Enter area (sqft), BHK, and bathrooms.
  Click “Estimate Price”.
The estimated price (in Lakhs ₹) appears with a subtle fade‑in effect.



Model Details:
Algorithm – Scikit‑learn LinearRegression.
Features –
  total_sqft (continuous)
  bath (continuous)
  bhk (continuous)
  One‑hot encoded location columns (derived from columns.json).
Training – Performed in model/Project_Price_Prediction.ipynb.
  The notebook loads a CSV of Bangalore listings, performs basic cleaning, encodes categorical locations, and fits the regression.
  The resulting model is serialized with pickle.dump to banglore_home_prices_model.pickle.
Prediction flow (util.get_estimated_price)
  Build a zero‑filled NumPy vector of length len(__data_columns).
  Populate the first three entries with sqft, bath, bhk.
  Set the one‑hot location index (if the location exists).
  Call __model.predict([x]) and round to two decimals.

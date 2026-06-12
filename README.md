# Copilot Implementation Instructions (STRICT)

You are generating code for a **Python Machine Learning inference system**.

Follow the instructions **strictly**.

Do NOT add extra frameworks, files, or directories.

Only generate code for the files listed below.

Do NOT create additional files.

Do NOT introduce random technologies like:

Flask
Django
React
Docker
NodeJS
Tensorflow
PyTorch

Only use:

Python
FastAPI
scikit-learn
numpy
pandas
joblib

---

# PROJECT PURPOSE

This project predicts **MCU maximum operating frequency (Fmax)** using a trained machine learning model.

The trained model already exists.

Model file:

models/rf_fmax_model.pkl

This is a **scikit-learn RandomForestRegressor** exported from Google Colab.

The model must NOT be retrained.

It must only be **loaded and used for inference**.

---

# INPUT FEATURES

The system receives **30 input features** in this exact order:

SMON1
SMON2
SMON3
...
SMON27

Env_voltage
Env_frequency
Env_temperature

Total features:

30

Target output:

Fmax

---

# PROJECT STRUCTURE (DO NOT CHANGE)

Copilot must generate code **ONLY for these files**.

MCU_Fmax_Predictor/

models/
rf_fmax_model.pkl

src/
preprocessing.py
predictor.py
uncertainty.py

api/
app.py

web/
index.html
script.js
style.css

requirements.txt

No other files should be created.

---

# FILE RESPONSIBILITIES

Each file has a specific purpose.

Follow these responsibilities exactly.

---

## src/preprocessing.py

This file must contain:

Function:

prepare_input(features)

Responsibilities:

• validate that exactly 30 features are provided
• convert features to numpy array
• reshape input to (1,30)

Return processed input ready for model prediction.

---

## src/predictor.py

This file must:

1. load the trained model using joblib
2. store the loaded model globally
3. implement function:

predict_fmax(features)

Steps:

1. call preprocessing.prepare_input()
2. run model.predict()
3. return predicted value

Model path:

models/rf_fmax_model.pkl

---

## src/uncertainty.py

This file calculates prediction uncertainty.

Use Random Forest tree predictions.

Steps:

1. iterate through model.estimators_
2. get prediction from each tree
3. compute mean prediction
4. compute standard deviation

Return:

mean_prediction
uncertainty
confidence interval

Confidence interval:

mean ± std

---

## api/app.py

Build a **FastAPI backend**.

The API must expose endpoint:

POST /predict

Input:

JSON with 30 features.

Example input:

{
"SMON1": 410,
"SMON2": 398,
...
"SMON27": 421,
"Env_voltage": 231,
"Env_frequency": 50.1,
"Env_temperature": 32
}

Steps:

1. extract features from request
2. call predictor.predict_fmax()
3. call uncertainty module
4. return response JSON

Response format:

{
"predicted_fmax": value,
"uncertainty": value,
"confidence_interval": [lower, upper]
}

---

## web/index.html

Create a simple interface with:

30 input fields.

Fields:

SMON1–SMON27
Env_voltage
Env_frequency
Env_temperature

Add button:

Predict

When clicked:

Call FastAPI endpoint `/predict`.

Display results:

Predicted Fmax
Uncertainty
Confidence interval

---

## web/script.js

Responsibilities:

1. collect input values
2. create JSON request
3. send POST request to FastAPI
4. receive response
5. update UI

---

## web/style.css

Basic styling only.

No external CSS frameworks.

---

## requirements.txt

Only include:

fastapi
uvicorn
numpy
pandas
scikit-learn
joblib
python-multipart

Do not add additional packages.

---

# IMPORTANT RULES

Copilot must follow these constraints:

1. Only generate code for the files listed above.
2. Do not introduce new frameworks.
3. Do not create new directories.
4. Do not retrain the model.
5. Use the existing `rf_fmax_model.pkl` file.
6. Keep code simple and readable.

---

# GOAL

Produce a **fully working ML inference system** that:

loads the trained RandomForest model
accepts 30 input features
predicts Fmax
returns uncertainty
provides a simple web interface
pip install -r requirements.txt
uvicorn api.app:app --reload
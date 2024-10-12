# Pregnancy Health Chatbot API

This repository contains the full development of a pregnancy health chatbot system that utilizes IndoBERT for intent classification, integrates generative AI (Ollama) for response generation, and is deployed on Google Cloud App Engine. The system includes an API built with Flask and a mobile application for end-users.

## Project Structure

- **`indobert-classification.ipynb`**: A Jupyter notebook for training and fine-tuning the IndoBERT model for intent classification on pregnancy-related data.
- **`flask-api/`**: Contains the Flask API, which handles user input classification and response generation.
- **`app-debug.apk`**: An Android application file that serves as the mobile client for interacting with the API.

## Features

1. **Intent Classification Using IndoBERT**: The chatbot uses IndoBERT to classify input into specific tags, such as "nutrition," "exercise," or "emergency" related to pregnancy.
2. **Response Generation via Ollama**: The system generates informative and context-aware responses for pregnancy-related queries using Ollama, a generative AI model.
3. **RESTful API**: A Flask-based API that handles incoming user queries, performs intent classification, and generates responses.
4. **Mobile App (APK)**: The mobile app provides a user-friendly interface for accessing the chatbot on Android devices.
5. **Google Cloud App Engine Deployment**: The API is deployed and scaled on Google Cloud App Engine.

## Technical Workflow
1. **Training the Model**: The indobert-classification.ipynb notebook is used to fine-tune the IndoBERT model on pregnancy-related data.
2. **Building the API**: The Flask API integrates this model for intent classification and uses Ollama to generate appropriate responses based on the predicted intent.
3. **Testing**: The API is tested locally using Postman, ensuring it correctly classifies user queries and generates accurate responses.
4. **Deployment**: The API is deployed to Google Cloud App Engine, making it scalable and accessible through a public URL.
5. **Mobile Application**: The Android app allows users to interact with the chatbot via a user-friendly interface.

## Installation and Setup

### Prerequisites
- Python 3.9+
- Google Cloud account
- Postman (for local testing)
- Android device (for APK installation)

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/pregnancy-chatbot.git
cd pregnancy-chatbot
```

### 2. Setting Up the Environment
Navigate to the `flask-api` directory:
```bash
cd flask-api
```
Create a virtual environment and install dependencies:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
```
Download the necessary NLTK data (required for preprocessing):
```bash
import nltk
nltk.download('punkt') # Or punk_tab
```
### 3. IndoBERT Classification
Before running the API, ensure that the IndoBERT model is fine-tuned. Run the notebook `indobert-classification.ipynb` to train and fine-tune the model on the dataset.
Save the trained model inside the `flask-api` directory.

### 4. Set Up Ollama API
Configure Ollama by adding API key to an .env file in the `flask-api` directory:
```bash
LLM_API_KEY=your_api_key_here
```

### 5. Running the Flask API Locally
To start the Flask API locally using terminal:
```bash
python app.py
```
The API will be available at  `http://127.0.0.1:5000`

### 6. Testing the API with Postman
You can test the API using Postman by sending a POST request with the following format:
1. Set the request type to POST.
2. Set the URL to http://127.0.0.1:5000/predict.
3. In the Body section, choose raw and set the type to JSON.
4. Use the following structure for the request body:
```bash
{
  "text": "Apa makanan yang baik untuk ibu hamil?"
}
```
5. The API will return a response with the classified tag and the generated reply:
```bash
{
    "tag": "nutrisi",
    "response": "Makanan bergizi seimbang, termasuk sayuran, buah, dan protein, sangat penting bagi ibu hamil."
}
```

### 7. Deploying to Google Cloud App Engine
#### Step 1: Set Up Google Cloud Project
1. Go to Google Cloud Console.
2. Create a new project or use an existing one.
3. Enable the App Engine API.

#### Step 2: Set Up App Engine Environment
Install the Google Cloud SDK and authenticate:
```bash
gcloud auth login
gcloud config set project your-project-id
```
#### Step 3: Prepare app.yaml for App Engine Deployment
In the `flask-api` directory, create an `app.yaml` file with the following content:
```bash
runtime: python39  
entrypoint: gunicorn -b :$PORT app:app

instance_class: F4

handlers:
- url: /.*
  script: app.py

network:
  session_affinity: true 

env: standard
```

#### Step 4: Deploy to App Engine
Deploy the Flask API to Google Cloud App Engine using the following command:
```bash
gcloud app deploy
```
After deployment, you will receive a URL where the API is hosted. Test it using Postman, similar to local testing, but with the new App Engine URL.

### 8. Running the Mobile App
1. Download the `app-debug.apk` file to your Android device.
2. Enable Install from Unknown Sources in your device's settings.
3. Install the APK on your Android device and launch the application.
4. The app will send requests to the hosted Flask API and provide responses to user queries.



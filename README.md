# Create a docker container serving a scikit-learn model using flask
- [x] Create a new directory and navigate to it.

- [x] Create a file called "app.py" and add the following code:
```
from flask import Flask, request
import pickle
import numpy as np

app = Flask(__name__)

@app.route("/predict", methods=["POST"])
def predict():
    data = request.get_json()
    model = pickle.load(open("model.pkl", "rb"))
    prediction = model.predict(np.array(data["input"]).reshape(1, -1))
    return str(prediction[0])

if __name__ == "__main__":
    app.run()
```
    
This code creates a Flask app that listens for POST requests to the "/predict" endpoint. When a request is received, it loads the scikit-learn model from a pickle file, makes a prediction using the input data from the request, and returns the prediction as a string.
- [x] Create a file called "Dockerfile" and add the following code:
```
FROM python:3.7

COPY . /app
WORKDIR /app

RUN pip install -r requirements.txt

CMD ["python", "app.py"]
```
This Dockerfile specifies the base image as python:3.7, copies all files in the current directory to the /app directory in the container, and installs the required packages from the requirements.txt file. Finally, it runs the app.py script when the container is started.

- [x] Create a file called "requirements.txt" and add the following code:
```
flask
scikit-learn
```
This file lists the required packages for the application, including Flask and scikit-learn.

- [x] Place the pickled scikit-learn model in the current directory and name it "model.pkl".

- [x] Build the docker image by running the following command:
```
docker build -t scikit-learn-model .
```
- [x] Run the docker container by running the following command:
```
docker run -p 5000:5000 scikit-learn-model
```
This will start the Flask app and make it available at http://localhost:5000. You can now make POST requests to the "/predict" endpoint with input data and receive a prediction from the scikit-learn model.


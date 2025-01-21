# web-for-teachable-Machine-Detection-with-Firebase-dog-classification-
This project demonstrates how to deploy a Teachable Machine image classification model on a webpage. It uses Firebase Firestore to log detections and displays a detection counter on the webpage. A reset button is also included to reset the counter.

---

‏## *Features*

‏1. *Webcam Integration*: Uses a webcam to classify images in real-time using the Teachable Machine model.
‏2. *Firebase Integration*: Logs detection counts and timestamps to a Firestore database.
‏3. *Detection Counter*: Displays the total number of detections on the webpage.
‏4. *Reset Button*: Provides functionality to reset the detection counter to zero.

---

‏## *How It Works*

‏1. *Setup*:
‏   - The webpage initializes the Teachable Machine model and connects to the webcam.
‏   - The Firebase Firestore is configured to log detection events.

‏2. *Detection*:
‏   - The model classifies the webcam feed in real-time.
‏   - If a class's prediction probability exceeds the specified threshold (default: 70%), the detection counter is incremented, and the detection is logged in Firestore.

‏3. *Counter Display*:
‏   - The current detection count is dynamically updated on the webpage.

‏4. *Reset Counter*:
‏   - Clicking the reset button sets the detection counter to zero.

---

‏## *Setup Instructions*

‏### Prerequisites

‏- A Firebase project with Firestore enabled.
‏- A Teachable Machine project URL.
‏- A modern web browser that supports JavaScript and WebRTC.

---

‏### Steps

‏1. *Clone the Repository*:
‏   Clone or download the repository to your local machine.

‏2. *Set Up Firebase*:
‏   - Create a Firebase project and enable Firestore.
‏   - Replace the Firebase configuration object in the code with your project's configuration details.

‏   javascript
‏   const firebaseConfig = {
‏       apiKey: "YOUR_API_KEY",
‏       authDomain: "YOUR_AUTH_DOMAIN",
‏       projectId: "YOUR_PROJECT_ID",
‏       storageBucket: "YOUR_STORAGE_BUCKET",
‏       messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
‏       appId: "YOUR_APP_ID",
‏       measurementId: "YOUR_MEASUREMENT_ID"
   };
   

‏3. *Update the Teachable Machine URL*:
‏   Replace the URL in the URL variable with your Teachable Machine project's URL.

‏   javascript
‏   const URL = "https://teachablemachine.withgoogle.com/models/YOUR_MODEL_ID/";
   

‏4. *Run the Project*:
‏   Open the index.html file in your browser. Allow the webpage to access your webcam when prompted.

---

‏## *File Structure*

‏plaintext
.
‏├── index.html   # Main webpage file
‏└── README.md    # Project documentation


---

‏## *Code Overview*

‏### Firebase Initialization

‏The Firebase configuration is initialized to connect to the Firestore database.

‏javascript
‏import { initializeApp } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-app.js";
‏import { getFirestore } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-firestore.js";

‏const app = initializeApp(firebaseConfig);
‏const db = getFirestore(app);


---

‏### Webcam and Model Initialization

‏The webcam is set up, and the Teachable Machine model is loaded.

‏javascript
‏async function init() {
‏    const modelURL = URL + "model.json";
‏    const metadataURL = URL + "metadata.json";
‏    model = await tmImage.load(modelURL, metadataURL);
‏    webcam = new tmImage.Webcam(200, 200, true);
‏    await webcam.setup();
‏    await webcam.play();
‏    window.requestAnimationFrame(loop);
}


---

‏### Detection and Logging

‏The model predicts the webcam feed, updates the counter, and logs detections to Firestore.

‏javascript
‏async function predict() {
‏    const prediction = await model.predict(webcam.canvas);
‏    for (let i = 0; i < maxPredictions; i++) {
‏        if (prediction[i].probability > 0.7) {
‏            detectionCount++;
‏            document.getElementById("counter").innerText = detectionCount;
‏            await saveDetectionToFirestore(detectionCount);
        }
    }
}


---

‏### Reset Counter

‏The counter is reset to zero when the reset button is clicked.

‏javascript
‏function resetCounter() {
‏    detectionCount = 0;
‏    document.getElementById("counter").innerText = detectionCount;
}


---

‏## *Demo*

‏1. Click *Start* to initialize the webcam and begin detection.
‏2. The detection counter updates dynamically on successful predictions.
‏3. Click *Reset Counter* to reset the detection count.

---

‏## *Dependencies*

‏- TensorFlow.js
‏- Teachable Machine Image Library
‏- Firebase SDK

---

‏## This is the complete code

‏<!DOCTYPE html>
‏<html lang="en">
‏<head>
‏    <meta charset="UTF-8">
‏    <meta name="viewport" content="width=device-width, initial-scale=1.0">
‏    <title>Teachable Machine with Firebase</title>

‏    <!-- Firebase SDK -->
‏    <script type="module">
‏        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-app.js";
‏        import { getFirestore } from "https://www.gstatic.com/firebasejs/11.1.0/firebase-firestore.js";

‏        // Your web app's Firebase configuration
‏        const firebaseConfig = {
‏            apiKey: "AIzaSyCObISasqXnsPxJSVLTc5t8ptQv8XPBUvc",
‏            authDomain: "dogs-classifier.firebaseapp.com",
‏            projectId: "dogs-classifier",
‏            storageBucket: "dogs-classifier.firebasestorage.app",
‏            messagingSenderId: "354325637367",
‏            appId: "1:354325637367:web:24b3e93e31ef1e9f60cbf2",
‏            measurementId: "G-T2WWB123EN"
        };

‏        // Initialize Firebase
‏        const app = initializeApp(firebaseConfig);
‏        const db = getFirestore(app); // Initialize Firestore
‏    </script>

‏    <!-- TensorFlow.js and Teachable Machine -->
‏    <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@latest/dist/tf.min.js"></script>
‏    <script src="https://cdn.jsdelivr.net/npm/@teachablemachine/image@latest/dist/teachablemachine-image.min.js"></script>

‏    <style>
‏        body {
‏            margin: 0;
‏            padding: 0;
‏            height: 100vh;
‏            display: flex;
‏            justify-content: center;
‏            align-items: center;
‏            flex-direction: column;
‏            background: linear-gradient(to bottom, #1E3C72, #2A69AC); /* Gradient blue background */
‏            font-family: Arial, sans-serif;
‏            text-align: center;
‏            color: white; /* White text color */
        }

‏        .text-block {
‏            background-color: rgba(0, 0, 0, 0.7); /* Dewy black background */
‏            color: white; /* White text color */
‏            padding: 15px;
‏            border-radius: 10px;
‏            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.5);
‏            margin: 5px 0;
        }

‏        #webcam-container, #label-container, #counter-container {
‏            margin: 10px 0;
        }

‏        button {
‏            padding: 10px 20px;
‏            background-color: rgba(255, 255, 255, 0.8);
‏            border: none;
‏            border-radius: 5px;
‏            cursor: pointer;
‏            font-size: 1em;
‏            margin: 5px;
‏            color: #333; /* Button text color */
        }

‏        button:hover {
‏            background-color: rgba(255, 255, 255, 1);
        }
‏    </style>
‏</head>
‏<body>
‏    <div class="text-block">Teachable Machine Image Model</div>
‏    <button type="button" onclick="init()" class="text-block">Start</button>
‏    <div id="webcam-container" class="text-block"></div>
‏    <div id="label-container" class="text-block"></div>
‏    <div id="counter-container" class="text-block">Total Detections: <span id="counter">0</span></div>
‏    <button type="button" onclick="resetCounter()" class="text-block">Reset Counter</button>

‏    <script type="text/javascript">
‏        const URL = "https://teachablemachine.withgoogle.com/models/YgmJKBsUY/";
‏        let model, webcam, labelContainer, maxPredictions;
‏        let detectionCount = 0; // Initialize detection counter

‏        // Load the image model and setup the webcam
‏        async function init() {
‏            const modelURL = URL + "model.json";
‏            const metadataURL = URL + "metadata.json";

‏            // Load the model and metadata
‏            model = await tmImage.load(modelURL, metadataURL);
‏            maxPredictions = model.getTotalClasses();

‏            // Setup the webcam
‏            const flip = true; // whether to flip the webcam
‏            webcam = new tmImage.Webcam(200, 200, flip); // width, height, flip
‏            await webcam.setup(); // request access to the webcam
‏            await webcam.play();
‏            window.requestAnimationFrame(loop);

‏            // Append elements to the DOM
‏            document.getElementById("webcam-container").appendChild(webcam.canvas);
‏            labelContainer = document.getElementById("label-container");
‏            for (let i = 0; i < maxPredictions; i++) {
‏                labelContainer.appendChild(document.createElement("div"));
            }
        }

‏        async function loop() {
‏            webcam.update(); // update the webcam frame
‏            await predict();
‏            window.requestAnimationFrame(loop);
        }

‏        // Run the webcam image through the image model
‏        async function predict() {
‏            const prediction = await model.predict(webcam.canvas);
‏            for (let i = 0; i < maxPredictions; i++) {
‏                const classPrediction = prediction[i].className + ": " + prediction[i].probability.toFixed(2);
‏                labelContainer.childNodes[i].innerHTML = classPrediction;

‏                // Check if the prediction exceeds a certain threshold
‏                if (prediction[i].probability > 0.7) { // Adjust threshold as needed
‏                    detectionCount++;
‏                    document.getElementById("counter").innerText = detectionCount;
‏                    await saveDetectionToFirestore(detectionCount);
                }
            }
        }

‏        // Save detection count to Firestore
‏        async function saveDetectionToFirestore(count) {
‏            try {
‏                await db.collection("detections").add({
‏                    count: count,
‏                    timestamp: new Date()
                });
‏                console.log("Detection saved to Firestore:", count);
‏            } catch (error) {
‏                console.error("Error saving detection to Firestore:", error);
            }
        }

‏        // Reset counter function
‏        function resetCounter() {
‏            detectionCount = 0;
‏            document.getElementById("counter").innerText = detectionCount;
‏            console.log("Counter reset to zero");
        }
‏    </script>
‏</body>
‏</html>

##Website link
file:///C:/Users/ReeR/Desktop/Rocky1.html

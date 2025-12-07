# Hand Proximity Detection – Real-Time OpenCV POC

This project is a real-time Hand Tracking + Virtual Boundary Proximity Alert System, built without MediaPipe, OpenPose, or any pose-detection APIs.
It demonstrates classical computer vision techniques to detect the hand, measure distance to a virtual object, and classify interaction states as:
	•	SAFE
	•	WARNING
	•	DANGER (with on-screen “DANGER DANGER”)

This prototype is created as part of the Arvyax Internship Assignment.

⸻

📌 Features

1. Real-Time Hand Tracking (Without ML Models)

Hand tracking is performed purely using:
	•	Skin-color segmentation (HSV color space)
	•	Contour detection
	•	Contour centroid tracking
	•	Noise reduction (Gaussian blur, erosion, dilation)

2. Virtual Object on Screen

The user selects a Region of Interest (ROI) manually using a mouse.
This ROI becomes the virtual object whose center is used to calculate distance.

3. Distance-Based State Logic

The system dynamically assigns one of three states:
	•	SAFE → Hand is far (>120px)
	•	WARNING → Hand approaching (between 60px and 120px)
	•	DANGER → Hand touching or extremely close (<60px)

4. Visual Feedback Overlay

The screen shows:
	•	State (SAFE / WARNING / DANGER)
	•	Distance in pixels
	•	Virtual object highlighted in state-color
	•	Hand contour
	•	Hand center point
	•	“DANGER DANGER” flashing on the screen in Danger range

5. CPU-Friendly & Lightweight
	•	Runs at 10–20 FPS on standard CPU
	•	Uses only OpenCV + NumPy


📂 Project Structure

Hand_Proximity_POC/
│── hand_proximity.py        # Main code (hand tracking + state logic)
│── Using_LLM_Model.ipynb    # (Optional) ML-based version
│── Without_using_ML.ipynb   # Classical CV version
│── sample_output.png        # Demo image (optional)
│── README.md                # Project documentation


📦 Requirements
Install dependencies:
pip install opencv-python numpy


▶️ Run the Project
python hand_proximity.py


Steps during execution:
	1.	Webcam starts
	2.	A window pops up → Select the virtual object using your mouse
	3.	Hand tracking starts automatically
	4.	Approach the object to trigger:
	•	Warning
	•	Danger
	•	“DANGER DANGER” alert


🧠 Technical Approach (Short Summary)

The system uses classical computer vision for hand tracking:

Skin Segmentation:

Hand pixels are extracted using HSV color range:
lower = np.array([0, 30, 60])
upper = np.array([20, 150, 255])

Contour Extraction:
Largest contour is considered the hand.

Centroid Calculation:
Hand center = Moments of contour.

Distance Calculation:
Between hand center and virtual object center
dist = np.linalg.norm(hand_center - obj_center)


State Machine
Simple 3-level rule-based system:
dist > 120 → SAFE  
60 < dist ≤ 120 → WARNING  
dist ≤ 60 → DANGER  

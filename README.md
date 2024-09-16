# Hand Tracking with OpenCV and MediaPipe

## Project Overview
This project implements real-time hand tracking using OpenCV and MediaPipe. It captures video input from the webcam, processes each frame to detect hand landmarks, and overlays hand connections and landmark points on the video feed. The frame rate (FPS) is also displayed in real-time. The project can be extended for gesture recognition, sign language interpretation, or other hand-based interaction applications.

## Key Features
- **Real-time hand tracking** using a webcam feed.
- **Detection and visualization** of hand landmarks (e.g., fingertips, knuckles).
- **Calculation and display of FPS** for performance tracking.
- **Easy-to-extend code** for gesture recognition and further hand interaction applications.

## Prerequisites
Before running the code, ensure you have the following installed:

- **Python 3.x**
- **OpenCV**: `pip install opencv-python`
- **MediaPipe**: `pip install mediapipe`

## How it Works

- **Webcam Capture**: The code captures video from your webcam using OpenCV (`cv2.VideoCapture(0)`).
- **Hand Detection**: MediaPipe's Hand solution is used to detect hand landmarks in real-time from the video frames.
- **Landmark Processing**: Each detected hand's landmarks (such as the fingertips, wrist, etc.) are processed to get their pixel coordinates.
- **Visualization**:
    - Circles are drawn around each hand landmark to highlight them.
    - The connections between the hand landmarks are drawn to visualize the hand structure.
- **FPS Display**: Frames per second (FPS) is calculated and displayed on the screen for performance monitoring.

## Code

```python
import cv2
import mediapipe as mp
import time

# Initialize the webcam capture
cap = cv2.VideoCapture(0)

# Setup MediaPipe Hand tracking solution
mpHands = mp.solutions.hands
hands = mpHands.Hands()
mpDraw = mp.solutions.drawing_utils

# Variables for calculating FPS
pTime = 0
cTime = 0

while True:
    # Read frame from the webcam
    success, img = cap.read()

    # Convert the frame to RGB, as MediaPipe requires it
    imgRGB = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

    # Process the RGB frame to detect hands
    results = hands.process(imgRGB)

    # If hands are detected, iterate through landmarks
    if results.multi_hand_landmarks:
        for handLms in results.multi_hand_landmarks:
            for id, lm in enumerate(handLms.landmark):
                # Get height, width, and channel of the frame
                h, w, c = img.shape
                # Convert normalized coordinates to pixel values
                cx, cy = int(lm.x * w), int(lm.y * h)
                print(id, cx, cy)
                # Draw a circle on the index finger tip (landmark id 4)
                cv2.circle(img, (cx, cy), 15, (255, 0, 255), cv2.FILLED)

            # Draw hand landmarks and connections on the frame
            mpDraw.draw_landmarks(img, handLms, mpHands.HAND_CONNECTIONS)

    # Calculate and display FPS
    cTime = time.time()
    fps = 1 / (cTime - pTime)
    pTime = cTime
    cv2.putText(img, str(int(fps)), (10, 70), cv2.FONT_HERSHEY_PLAIN, 3,
                (255, 0, 255), 3)

    # Display the output frame
    cv2.imshow("Image", img)
    cv2.waitKey(1)
```

## Explanation of Key Sections
- **Webcam Input**: The line `cap = cv2.VideoCapture(0)` opens the default webcam for capturing video.
- **MediaPipe Initialization**: The `mpHands.Hands()` object is used to detect hand landmarks, and `mpDraw.draw_landmarks()` visualizes them.
- **Landmark Coordinates**: Each landmark has normalized coordinates, which are scaled to pixel values using the image’s width and height.
- **FPS Calculation**: Using `time.time()`, the FPS is calculated to monitor the performance of the real-time tracking.

## Extensions
This project can be extended to:

- **Gesture Recognition**: Build gesture-based interfaces or games.
- **Sign Language Interpretation**: Extend the model for recognizing specific hand gestures for sign language.
- **Human-Computer Interaction (HCI)**: Use hand tracking as a natural input method for devices.

## Potential Use Cases
- **Gesture Recognition**: Create gesture-controlled systems or games.
- **Sign Language Interpretation**: Build tools that recognize and interpret hand gestures in sign language.
- **Virtual Interaction**: Develop applications that enable interaction with virtual environments using hand gestures.
## References
- OpenCV Documentation
- MediaPipe Hands Documentation



# Output :
![Image 16-09-2024 20_24_33](https://github.com/user-attachments/assets/72dfe1bb-00d7-4229-9961-e19f4407c960)
![Image 16-09-2024 20_25_26](https://github.com/user-attachments/assets/eacb8d2d-ac4f-4cde-9928-51bdf18b15a3)
![Image 16-09-2024 20_25_28](https://github.com/user-attachments/assets/c3d23cbe-8dcb-4442-98d7-b96501cd6cbd)
![Image 16-09-2024 20_25_33](https://github.com/user-attachments/assets/6515b62b-62e3-4fe3-840c-38a04426010e)
![Image 16-09-2024 20_25_37](https://github.com/user-attachments/assets/ccbee4e6-7c45-48cf-8d26-99698a4264cc)

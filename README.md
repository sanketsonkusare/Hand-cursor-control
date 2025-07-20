**Hand Cursor Control and Click/Right-Click with MediaPipe**

This Python script demonstrates real-time hand cursor control with click and right-click functionality using MediaPipe and PyAutoGUI libraries.

**Features:**

- **Cursor Control:** Moves the mouse cursor based on the index finger tip's position on the screen.
- **Click:** Performs a left click when the index finger tip and thumb tip come close together (adjustable distance threshold).
- **Right-Click:** Performs a right click when the index finger tip and middle finger tip come close together (adjustable distance threshold).
- **Screen Calibration:** Automatically scales the finger tip coordinates to match your screen resolution.

**Requirements:**

- Python 3.x (with OpenCV, MediaPipe, and PyAutoGUI libraries installed)
- Webcam

**Installation:**

1. Ensure you have Python 3.x installed on your system.
2. Open a terminal or command prompt and navigate to your project directory.
3. Install the required libraries using pip:

   ```bash
   pip install opencv-python mediapipe pyautogui
   ```

**Usage:**

1. Run the script:

   ```bash
   python mouse.py
   ```

2. A window titled "MediaPipe Hands" will display your webcam feed with hand landmarks.
3. Move your hand in front of the webcam to control the cursor and perform clicks/right-clicks based on finger gestures.

**Explanation:**

1. **Import Libraries:**
   - `cv2`: OpenCV for image processing.
   - `mediapipe`: MediaPipe for hand landmark detection.
   - `pyautogui`: PyAutoGUI for mouse control.

2. **Get Screen Resolution:**
   - `screen_width`, `screen_height`: Capture the screen dimensions.

3. **Open Webcam:**
   - `cap`: Create a VideoCapture object to access the webcam.

4. **Initialize Hand Detection:**
   - `mp_drawing`: Utility functions for drawing hand landmarks.
   - `mp_hands`: Hands model for landmark detection with configurable confidence thresholds.

5. **Main Loop:**
   - `while cap.isOpened()`: Continuously process webcam frames until the script is exited.
   - `success, image = cap.read()`: Read the next frame from the webcam.
   - `If not successful: continue`: Skip to the next iteration if frame reading fails.

6. **Image Preprocessing:**
   - `image = cv2.flip(image, 1)`: Mirror the image horizontally to match the user's perspective.
   - `image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)`: Convert the image from BGR (OpenCV's default) to RGB for MediaPipe compatibility.

7. **Hand Landmark Detection:**
   - `results = hands.process(image)`: Detect hand landmarks in the image.

8. **Image Postprocessing:**
   - `image = cv2.cvtColor(image, cv2.COLOR_RGB2BGR)`: Convert the image back to BGR for OpenCV display.

9. **Draw Landmarks (Optional):**
   - `if results.multi_hand_landmarks`: If hands are detected:
     - `for hand_landmarks in results.multi_hand_landmarks`: Iterate over each detected hand.
       - `mp_drawing.draw_landmarks(image, hand_landmarks, mp_hands.HAND_CONNECTIONS)`: Draw the detected hand landmarks on the image.

10. **Cursor Control:**
   - `x`, `y`: Calculate the index finger tip's screen coordinates, scaled to match the screen resolution using a scaling factor (adjust if needed).
   - `pyautogui.moveTo(x, y)`: Move the mouse cursor to the calculated coordinates.

11. **Click/Right-Click Detection:**
   - `index_finger_tip`, `middle_finger_tip`: Get the coordinates of the index and middle finger tips.
   - `distance = ...`: Calculate the Euclidean distance between the two fingertips.
   - `if distance < 0.03:`: If the distance is less than a threshold (adjust if needed), perform a right click.
   - Perform similar logic for click detection using the index fingertip and thumb tip.

12. **Display and Exit:**
   - `cv2.imshow('MediaPipe Hands', image)`: Display the processed image with hand landmarks (if enabled).
   - `cv2.waitKey(5) & 0xFF == 27`: Check for keyboard input (Esc key to quit).
   - `cap.release()`:

# Saving People in Pool

## Overview
The **Saving People in Pool** project is designed to enhance safety in swimming pools by using AI to detect and respond to potential drowning situations. By integrating pose estimation with object detection, this project identifies when someone might be in distress and issues a warning alert.

## Features
- **Real-Time Pool Monitoring:** Continuously monitors the pool area using a webcam feed.
- **Pose Estimation:** Utilizes MediaPipe to detect and analyze body landmarks, specifically focusing on arm positions to identify potential distress.
- **YOLOv5 Integration:** Implements the YOLOv5 model to detect and track individuals in the pool.
- **Automated Alerts:** Issues a warning on the screen when it detects someone in a potentially dangerous position, such as having their arms raised above their head in a way that could indicate distress.

## Dependencies
This project requires the following libraries:
- OpenCV
- MediaPipe
- NumPy
- PyTorch

Install the dependencies with the following command:
```bash
pip install opencv-python mediapipe numpy torch
```

## How It Works
### Pose Estimation
The project uses MediaPipe's Pose solution to detect key body landmarks. It calculates angles at the elbow joints to determine if a person's arm is raised in a way that might indicate they need help.

### Object Detection with YOLOv5
The YOLOv5 model is used to detect and track people in the pool. It helps in identifying the position of individuals and ensuring that the pose estimation is focused on the right targets.

### Alert System
The system continuously analyzes the detected poses. If it detects that someone's arm is raised above their head with a significant angle at the elbow, and this condition persists for several frames, it triggers a visual alert on the screen.

### Code Explanation
1. **Initialization:**
   Set up MediaPipe Pose and YOLOv5 model for pose estimation and object detection.
   ```python
   mp_pose = mp.solutions.pose
   poses = mp_pose.Pose(min_tracking_confidence=0.5, min_detection_confidence=0.5)
   model = torch.hub.load("ultralytics/yolov5", "yolov5m", pretrained=True)
   ```

2. **Angle Calculation:**
   Define a function to calculate the angle between three points, essential for detecting arm positions.
   ```python
   def angles(a, b, c):
       ...
       return angle
   ```

3. **Real-Time Detection:**
   Capture video from the webcam, process it to detect poses and objects, and determine if someone is in distress.
   ```python
   _, image = video.read()
   image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
   results = model.process(image)
   result = poses.process(image_rgb)
   ```

4. **Alert System:**
   If the conditions for distress are met (e.g., arms raised with a significant elbow angle), the system triggers a warning.
   ```python
   if flag >= frame_check:
       cv2.putText(image, "Warning!!! Someone need Help", (20, 75), cv2.FONT_ITALIC, 1, (0, 255, 0))
   ```

5. **Display Output:**
   Show the video feed with overlaid pose landmarks and detection boxes, including warnings when necessary.
   ```python
   cv2.imshow("image", image)
   ```


## Future Enhancements
- **Enhanced Detection:** Improve the system to handle complex scenarios with multiple people and varying water conditions.
- **Integration with Alarm Systems:** Connect the alert system to external alarms or emergency notifications for faster response.
- **Machine Learning Training:** Fine-tune the model to reduce false positives and increase accuracy in detecting distress situations.

## Conclusion
This project is a practical application of AI in enhancing safety in swimming pools. By combining pose estimation with object detection, it provides a real-time solution to monitor and potentially save lives in aquatic environments. Contributions and improvements are welcome as we aim to make pools safer for everyone.

---

# 🍎 Sliced Fruit Detector (SliceTrack AI)

A vision system based on the **YOLOv11** architecture that detects fruits in real-time and precisely counts points for slicing them. The project uses advanced object detection to simulate the mechanics of a "Fruit Ninja" style game in the real world.

## 📂 Project Files Description

* **`main.py`** – The main script controlling the application. It handles capturing video from the camera, initializing the display window, and managing sliders (Trackbars) for dynamic, on-the-fly calibration of `Confidence` and `IOU` parameters.
* **`detector.py`** – A module containing the `FruitNinjaDetector` class. It processes raw video frames, manages logical states, and implements a custom point-counting algorithm.
* **`model_train.py`** – A dedicated script for the machine learning process. It allows loading base weights and training the model on custom data.
* **`data.yaml`** – YOLO configuration file. Defines paths to datasets and maps classes: `not cutting` (ID: 0) and `cutting` (ID: 1).

## 📊 Custom Dataset

Unlike ready-made solutions available online, **I prepared the entire training dataset myself**. 
* The photos were taken in real-world conditions, reflecting the actual positioning of hands and knives.
* Labeling accounts for the critical moments of contact between the blade and the fruit, eliminating the problem of detection bounding boxes disappearing during interaction.

### Model Performance (Confusion Matrix)

The confusion matrix below confirms the model's high effectiveness in distinguishing the state of a whole fruit from the moment it is cut:

<img width="3000" height="2250" alt="image" src="https://github.com/user-attachments/assets/8e56fd70-0ab4-4044-aae1-9dda40f1004d" />

## 🛠️ Project Evolution and Development

### Phase 1: Baseline Detection
Early tests focused on the stability of class recognition by the YOLOv11 model without implemented point-counting logic.

<img width="796" height="598" alt="Zrzut ekranu 2026-03-21 230217" src="https://github.com/user-attachments/assets/3bf5fb25-a1c1-40d9-8ba9-1f4b43f7cb7a" />
 <img width="792" height="601" alt="Zrzut ekranu 2026-03-21 230526" src="https://github.com/user-attachments/assets/58ea90d6-1f32-4dd0-a2a5-2a6b0ae90fb1" />


### Phase 2: Cutting Recognition Logic Implementation
A key element of the project was creating a robust, error-resistant counter. The system relies on state change verification – a point is awarded only upon detecting a transition from a whole object (`not cutting`) to a cut one (`cutting`).

**Code snippet implementing this logic:**
```python
# Check for the presence of a whole fruit (Class 0)
if 0 in classes: 
    self.pom1 = 1
    
# Award a point after detecting a confident cut (Class 1, confidence > 60%)
if max_cutting_conf > 0.60 and self.pom1 == 1:
    self.counter += 1
    self.pom1 = 0
```

### Phase 3: System Integrated with UI
The final version of the project features a scaled-down point counter in the upper right corner of the screen and optimized interface readability.
<img width="795" height="603" alt="Zrzut ekranu 2026-03-22 133233" src="https://github.com/user-attachments/assets/1b0c9733-23d0-4443-96d5-ecda7b7b895d" />
<img width="796" height="597" alt="Zrzut ekranu 2026-03-22 132614" src="https://github.com/user-attachments/assets/4af45394-2d09-4bbb-bd8b-90495e568a90" />

## 🧰 Technologies & Libraries Used

This project leverages the following tools and frameworks to ensure real-time performance and accurate detection:

* **Python 3.x**: The core programming language used for the entire logic.
* **Ultralytics (YOLOv11)**: The state-of-the-art framework used for training the custom dataset and running the object detection model.
* **OpenCV (`cv2`)**: Handles capturing the video stream from the camera, frame processing, and drawing the graphical user interface (bounding boxes, text, trackbars).
* **PyTorch & CUDA**: Enables GPU acceleration (NVIDIA RTX series) to ensure smooth, lag-free inference during gameplay.



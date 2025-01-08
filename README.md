# AR-BattleVision
This Python script performs real-time video processing to detect objects, assess posture, detect camouflage, and classify subjects as "Friend" or "FOE". It calculates a hostility score based on weapon detection, posture analysis, and camouflage detection. The output video displays bounding boxes around detected objects, highlighting "Friend" or "FOE" subjects, along with their hostility score.

Key components of the script:
- **Object Detection**: Uses an SSD MobileNet V2 model to identify objects like weapons.
- **Posture Detection**: Uses MoveNet to analyze key body points and assess posture.
- **Camouflage Detection**: Uses a pretrained model to detect camouflage patterns.
- **Hostility Scoring**: Calculates a hostility score based on weapon detection, posture, and camouflage presence.
- **Friend or Foe Classification**: QR code detection is used to identify friendly targets.

---

## Prerequisites

1. **Python 3.6+** (tested with Python 3.8)
2. **TensorFlow 2.x**
3. **OpenCV**
4. **NumPy**
5. **TensorFlow Hub**

### Install Required Libraries

```bash
pip install tensorflow opencv-python numpy tensorflow-hub
```

---

## Script Structure

### 1. **Loading Pretrained Models**

The script loads three essential models for object detection, posture detection, and camouflage detection:

- **SSD MobileNet V2 (Object Detection)**: Detects objects (e.g., weapons) in the video.
- **MoveNet Thunder (Posture Detection)**: Analyzes human body postures, specifically detecting keypoints such as wrists, nose, etc.
- **Camouflage Detection Model**: Detects camouflage patterns in frames, using a custom-trained model.

These models are downloaded from TensorFlow Hub and saved locally in the `pretrained_models` directory to avoid repeated downloads.

### 2. **Hostility Scoring Parameters**

The following weights are assigned to different aspects of hostility detection:
- **Weapon Weight**: 0.5
- **Posture Weight**: 0.3
- **Camouflage Weight**: 0.3
- **Hostility Threshold**: 0.7 (Subjects with scores above this threshold are considered hostile)

### 3. **Detection Functions**

- **`detect_objects(image, model)`**: Detects objects in an image frame using the SSD MobileNet model.
- **`detect_posture(image, model)`**: Detects human keypoints using the MoveNet model.
- **`detect_camouflage(image)`**: Detects camouflage using the trained camouflage model.
- **`apply_camouflage_mask(frame, mask)`**: Applies the camouflage mask to the frame and highlights it.
- **`classify_friend_or_foe(frame)`**: Decodes QR codes in the frame to determine if a subject is "Friend" or "FOE" based on predefined codes.
- **`calculate_hostility_score(detections, keypoints, camouflage_mask)`**: Computes the hostility score based on weapon detection, posture analysis, and camouflage presence.

### 4. **Video Processing**

The script processes each frame of an input video (`input_video`) and applies the following steps:
- **Object Detection**: Identifies objects and scores detections above a threshold.
- **Posture Detection**: Extracts human body keypoints and checks for aggressive posture.
- **Camouflage Detection**: Identifies whether the subject is camouflaged.
- **Friend or Foe Classification**: Uses QR codes to classify subjects as "Friend" or "FOE".
- **Hostility Scoring**: Combines the above factors to calculate a hostility score.
- **Frame Updates**: Draws bounding boxes and labels on the frame, with color-coding based on classification and hostility score.

The processed frames are saved to an output video (`processed_video.mp4`).

---

## Workflow

1. **Model Loading**:
   - The script checks if the pretrained models are already downloaded. If not, it downloads them from TensorFlow Hub and saves them locally.

2. **Frame Processing**:
   - The video is processed frame by frame. For each frame:
     - **Object Detection**: Detects objects using SSD MobileNet V2.
     - **Posture Detection**: Extracts keypoints using MoveNet.
     - **Camouflage Detection**: Identifies camouflage using the trained model.
     - **Friend or Foe Classification**: Identifies friendly targets using QR codes.
     - **Hostility Scoring**: Combines the results from the above functions to calculate a hostility score.
     - **Visualization**: Bounding boxes are drawn on the frame with appropriate labels and color-coding.
   
3. **Saving Processed Video**:
   - The processed video is saved with bounding boxes, hostility scores, and other visual enhancements.

4. **Output**:
   - The final video (`processed_video.mp4`) shows the result of the analysis.

---

## Parameters

- **Model Directory**: Models are saved in the `pretrained_models` directory to avoid re-downloading.
- **Hostility Weights**: These parameters control the significance of weapon, posture, and camouflage detection in the hostility score calculation.
- **Input Video**: Path to the input video to be processed (default: `input_video`).
- **Output Video**: The processed video will be saved as `processed_video.mp4`.

---

## Output

- **Processed Video**: Each frame of the video will contain:
  - Bounding boxes for detected objects.
  - Labels indicating "Friend" or "FOE".
  - Hostility scores displayed for each detection.

---

## Customization

- **QR Codes**: Update the `FRIENDLY_QR_CODES` dictionary to include any additional QR codes you want to classify as "Friend".
- **Hostility Calculation**: Adjust the weights in the `HOSTILITY_WEIGHT` parameters to change how the hostility score is calculated.

---

## Conclusion

This script combines advanced computer vision techniques to detect, classify, and score subjects in a video based on various factors such as posture, camouflage, and object detection. It is highly customizable, allowing for modifications in model choice, detection methods, and hostility scoring. This can be useful in a variety of applications, including security, military simulations, and video analysis.

# Vision-Based Robotic Arm for Object Sorting

A fully working prototype of an Arduino-powered robotic arm that uses computer vision (OpenCV) to detect and sort objects by **shape** and **color** in real time.

---

## 📌 Project Overview

This system automates the sorting of objects on a conveyor belt using a camera-vision pipeline paired with a multi-DOF robotic arm. The arm identifies each object's shape and color, then picks and places it into the correct bin — all without human intervention.

---

## 🎥 Demo

> _Add a GIF or video link here_

---

## ⚙️ System Components

| Component | Details |
|-----------|---------|
| **Microcontroller** | Arduino (Uno / Mega) |
| **Vision Library** | OpenCV (Python) |
| **Camera** | USB Webcam / Pi Camera |
| **Robotic Arm** | Multi-DOF servo-based arm |
| **Gripper** | Servo-controlled end effector |
| **Communication** | Serial (USB) — PC to Arduino |

---

## 🧠 How It Works

```
Camera Feed → OpenCV Processing → Object Classification → Serial Command → Arduino → Arm Movement
```

1. **Image Capture** — Camera streams live feed of objects on the conveyor
2. **Color Segmentation** — HSV thresholding isolates object color (red / green / blue / etc.)
3. **Shape Detection** — Contour analysis + Hough Transform identifies shape (circle / square / triangle)
4. **Decision Making** — Python script maps shape+color to a target bin
5. **Serial Command** — Command sent to Arduino via `pyserial`
6. **Arm Execution** — Arduino drives servos to pick and place the object

---

## 📁 Project Structure

```
vision-robotic-arm/
├── src/
│   ├── vision/
│   │   ├── color_segmentation.py   # HSV-based color detection
│   │   ├── shape_detection.py      # Contour & Hough Transform
│   │   └── main_pipeline.py        # Full vision + serial pipeline
│   └── arduino/
│       └── arm_controller.ino      # Servo control & serial command parser
├── config/
│   ├── color_thresholds.yaml       # HSV min/max values per color
│   └── arm_positions.yaml          # Pre-defined bin positions (servo angles)
├── docs/
│   └── system_diagram.png
├── requirements.txt
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- Arduino IDE
- OpenCV: `pip install opencv-python`
- PySerial: `pip install pyserial`

### Installation

```bash
# Clone the repository
git clone https://github.com/your-username/vision-robotic-arm.git
cd vision-robotic-arm

# Install Python dependencies
pip install -r requirements.txt
```

### Upload Arduino Code

1. Open `src/arduino/arm_controller.ino` in Arduino IDE
2. Select your board and COM port
3. Upload the sketch

### Run the Vision Pipeline

```bash
# Update the serial port in main_pipeline.py (e.g., COM3 or /dev/ttyUSB0)
python src/vision/main_pipeline.py
```

---

## 🎨 Supported Object Classes

| Color | Shape | Target Bin |
|-------|-------|------------|
| Red | Circle | Bin 1 |
| Blue | Square | Bin 2 |
| Green | Triangle | Bin 3 |

> _Extend `color_thresholds.yaml` and `arm_positions.yaml` to add more classes._

---

## 📷 Image Processing Pipeline

```
Raw Frame
   │
   ▼
HSV Conversion → Color Mask → Contour Detection → Shape Classification
                                                         │
                                              ┌──────────┴──────────┐
                                           Circle              Square / Triangle
                                              │                      │
                                         Bin 1 cmd             Bin 2 / 3 cmd
```

**Key techniques used:**
- `cv2.cvtColor` — BGR to HSV conversion
- `cv2.inRange` — Color thresholding
- `cv2.findContours` — Shape boundary extraction
- `cv2.approxPolyDP` — Polygon approximation for shape ID
- `cv2.HoughCircles` — Circle detection

---

## 🔧 Configuration

Edit `config/color_thresholds.yaml` to tune HSV ranges for your lighting:

```yaml
red:
  lower: [0, 120, 70]
  upper: [10, 255, 255]
blue:
  lower: [100, 150, 50]
  upper: [130, 255, 255]
```

Edit `config/arm_positions.yaml` to set servo angles per bin:

```yaml
bin_1: [90, 45, 30, 80]   # [base, shoulder, elbow, wrist] in degrees
bin_2: [45, 50, 35, 80]
bin_3: [135, 45, 30, 80]
```

---

## 🔌 Hardware Wiring

| Arduino Pin | Connected To |
|-------------|-------------|
| D3 | Base Servo Signal |
| D5 | Shoulder Servo Signal |
| D6 | Elbow Servo Signal |
| D9 | Wrist Servo Signal |
| D11 | Gripper Servo Signal |
| 5V / GND | External servo power supply |

> ⚠️ Use an external 5V power supply for servos — do NOT power them from the Arduino's onboard 5V pin.

---

## 🛠️ Future Improvements

- [ ] Add CNN-based classifier for more complex object shapes
- [ ] Integrate ROS2 for modular sensor-arm control pipeline
- [ ] Replace rule-based sorting with ML model (SVM / YOLO)
- [ ] Add conveyor belt speed feedback for dynamic pick timing
- [ ] Web dashboard for live camera feed and sorting stats

---

## 📄 License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.

---

## 👤 Author

**Himanshu Rokade**  
Robotics Engineer | ROS2 | Computer Vision  
[GitHub](https://github.com/Himanshu-Rokade) · [LinkedIn](https://linkedin.com/in/your-profile)

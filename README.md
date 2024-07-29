# Real-Time Face Detection using OpenCV and Flask

This repository contains the code to create a real-time face detection application using OpenCV and Flask.

## Prerequisites

- Python 3.x

## Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yourusername/your-repo-name.git
   cd your-repo-name
   ```

2. **Create a virtual environment and activate it**:
   ```bash
   python -m venv venv
   venv\Scripts\activate  # For Windows
   # OR
   source venv/bin/activate  # For macOS/Linux
   ```

3. **Install the required packages**:
   ```bash
   pip install flask opencv-python numpy
   ```

## Project Structure
```
project_folder/
│
├── app.py
├── requirements.txt
└── templates/
    └── index.html
```

## Code

### `app.py`
```python
from flask import Flask, render_template, Response
import cv2

app = Flask(__name__)

camera = cv2.VideoCapture(0)

def generate_frames():
    while True:
        success, frame = camera.read()
        if not success:
            break
        else:
            ret, buffer = cv2.imencode('.jpg', frame)
            frame = buffer.tobytes()
            yield (b'--frame\r\n'
                   b'Content-Type: image/jpeg\r\n\r\n' + frame + b'\r\n')

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/video_feed')
def video_feed():
    return Response(generate_frames(), mimetype='multipart/x-mixed-replace; boundary=frame')

if __name__ == '__main__':
    app.run(debug=True)
```

### `templates/index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Face Detection</title>
</head>
<body>
    <h1>Real-Time Face Detection</h1>
    <img src="{{ url_for('video_feed') }}" width="100%">
</body>
</html>
```

## Running the Application

1. **Run the application**:
   ```bash
   python app.py
   ```
   
2. **Open your browser and navigate to**:
   ```
   http://127.0.0.1:5000/
   ```
![Screenshot (75)](https://github.com/user-attachments/assets/63702ef2-6a04-4050-b554-a358ac58076c)
![Screenshot (74)](https://github.com/user-attachments/assets/e9769a59-88ab-4a16-8fe0-087ce3ef973c)
![87654323 (2)](https://github.com/user-attachments/assets/e70ea5e3-30bc-420c-8e13-dd632d20e8cc)
![09876543](https://github.com/user-attachments/assets/3761b45b-3505-447a-a148-5873167271da)

## Usage Instructions 

1. **Download and Install Python**:
   - Visit [python.org](https://www.python.org/downloads/) to download and install Python. Ensure you check the box to add Python to your PATH during installation.

2. **Set Up the Project**:
   - Create a folder for your project and navigate into it using the command line.
   - Follow the steps in the "Setup" section to clone the repository, create a virtual environment, and install the required packages.

3. **Run the Project**:
   - Ensure your webcam is connected.
   - Run the `app.py` script to start the Flask server.
   - Open your web browser and go to `http://127.0.0.1:5000/` to see the real-time face detection in action.

4. **Understand the Code**:
   - `app.py` contains the main Flask application and the OpenCV logic to capture and stream video.
   - The HTML file `index.html` in the `templates` folder sets up the webpage that displays the video feed.

## Troubleshooting

- **Camera Not Detected**:
  - Ensure your camera is properly connected.
  - Check if another application is using the camera.
  
- **Dependencies Not Installed**:
  - Run `pip install -r requirements.txt` to ensure all dependencies are installed.

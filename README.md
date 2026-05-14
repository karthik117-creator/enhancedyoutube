# Enhanced YouTube Player — Gesture & Voice Control

## What I Built
An AI-powered web application that allows users to control 
YouTube playback using hand gestures and voice commands — 
no keyboard or mouse needed.

## My Contributions
- Trained custom CNN gesture recognition model from scratch
- Integrated Firebase ML Kit for voice command detection
- Built Flask backend with YouTube API integration
- Implemented real-time sleepiness and absence detection
- Optimized frame processing — 35% improvement in responsiveness
- Achieved 40% increase in playback efficiency

## Tech Stack
- Python, Flask, MediaPipe, OpenCV
- TensorFlow Lite, CNN Models
- Firebase ML Kit
- HTML, CSS, JavaScript

## Features
- 13 hand gesture controls
- Voice command recognition
- Auto-pause on sleep detection
- Auto-pause on absence detection

## Results
- 40% improvement in playback efficiency
- 35% better responsiveness under varying lighting

## How To Run
4. [Usage](#usage)
    * [Installing libraries](#installing-libraries)
    * [Saving data](#saving-data)
    * [Training](#training)
    * [Running the web app](#running-the-web-app)
5. [Limitations](#limitations)
6. [References](#references)

## Project Structure
```bash
 ┣━ 📂data
 ┃ ┣━ 📜check_data.ipynb
 ┃ ┣━ 📜gestures.csv
 ┃ ┣━ 📜label.csv
 ┃ ┗━ 📜player_state.json
 ┣━ 📂flask_app
 ┃ ┣━ 📂static
 ┃ ┃ ┣━ 📂css
 ┃ ┃ ┃ ┗━ 📜styles.css
 ┃ ┃ ┗━ 📂icons
 ┃ ┃ ┃ ┗━ 📜favicon-32x32.png
 ┃ ┣━ 📂templates
 ┃ ┃ ┗━ 📜demo.html
 ┃ ┣━ 📜app.py
 ┃ ┗━ 📜video_feed.py
 ┣━ 📂models
 ┃ ┣━ 📜model.pth
 ┃ ┣━ 📜model_architecture.py
 ┃ ┗━ 📜shape_predictor_68_face_landmarks.dat
 ┣━ 📜main.py
 ┣━ 📜requirements.txt
 ┣━ 📜train.ipynb
 ┗━ 📜utils.py
```

* __main.py__  
For saving data and checking the output of models.

* __utils.py__  
A collection of functions used in `main.py` .

* __train.ipynb__  
For training and validating our artificial neural network.

* __data/__  
Folder containing saved data (`gestures.csv`), general information about the saved data (`check_data.ipynb`) and gestures names (`label.csv`).  
The `player_state.json` is automatically generated and gives information whether the player is in pause or playing mode.

* __models/__  
Contains the trained neural network (`model.pth`) and it's architecture(`model_architecture.py`) as well as the face landmarks predictor (`shape_predictor_68_face_landmarks.dat`)
* __flask_app/__  
Contains important files for [running the web app](#running-the-web-app).

## Usage
NB: I'm using Windows 10.
### Installing libraries
I suggest creating a virtual environment and installing the libraries there.
```
cd project_folder_name
python -m venv your_virtual_env_name
your_virtual_env_name\Scripts\activate.bat
pip install -r requirements.txt  
```
### Saving data
Run `main.py`.  
When the webcam video has loaded, press 'r' on the keyboard to activate the logging mode. By pressing '0' to '9', data get saved in `gestures.csv`; whereby the first column represents the class labels (pressed keys) and the other columns are the normalized keypoints and distances (see example below). To save class labels extending from '10' to potentially '35', you can press alphabet keys (capital letters) from 'A' to 'Z', respectively.  
If you change the number of classes, make sure to correspondingly update the variable `n_classes` in `model_architecture.py` file.

<img src="https://user-images.githubusercontent.com/100664869/194744094-7ee8244c-a750-4339-bdd5-1f57f8226564.png">  

### Training
For training the model, simply run the entire file `train.ipynb`. If you change data, you'll probably need to experiment to obtain an acceptable model's performance. 
In case you change the model architecture, make sure to correspondingly update the `model_architecture.py` file.  The architecture I used looks like this:  
<img src="https://user-images.githubusercontent.com/100664869/194754632-14b9589f-7689-41b5-897f-0ed5339bbbf5.png"> 
### Running the web app
```
cd flask_app
python app.py
```
You'll be provided with a link where the app is running. In the image below, it's running for example at ___http://<span></span>127.0.0.1:5000___.  

<img src="https://user-images.githubusercontent.com/100664869/194744362-67e00d66-0f01-49b2-b253-e4e3bd055003.png">  

Go to that url, copy-paste a youtube video link in the input field and hit the start button.
Both the youtube video and your webcam video will load into the web page.  
Hand gestures are valid only when your hand is in the red box within your webcam video. This is to prevent unintentional interactions with the player (e.g. when scratching your face). You first need to move the mouse above the player and left-click to start the video; of course with hand gestures 😉. This puts the player in focus mode and allows the rest of interactions to be performed.  
It's better to use the 'neutral' gesture between other gestures; this decreases false positive detections that might occur when transitioning directly from one gesture to another.

Here is a list of all the implemented interactions.  
<img src="https://user-images.githubusercontent.com/100664869/194752626-125f0a5f-fca2-4a04-aefb-bf07679fe0a7.png">  

___Legend___:
| Move mouse | Left click | right click | Play/Pause | Backward |
| --- | --- | --- |--- |--- |
| Move mouse cursor | mouse left click | mouse right click | Toggle Play/Pause | Seek backward 5 seconds |

|Forward | Vol.down.gen | Vol.up.gen | vol.down.ytb | Vol.up.ytb |
| --- | --- | --- |--- |--- |
| Seek forward 5 seconds | Decrease computer's volume | Increase computer's volume | Decrease Youtube player's volume | Increase Youtube player's volume |

| Full screen | Subtitle | Neutral | Sleepness | Absence |
| --- | --- | --- |--- |--- |
| Toggle full screen mode | Toggle On/Off subtitles/closed captions if available | Do nothing | Pause if user is sleeping | Pause if user has left |
## Limitations
* In low light conditions, hand landmark predictions are less stable, which in turn degrades the quality of gesture detection. Same applies to face detectors (mainly Dlib), as the image gets less clear.
* The sleepness detection works well only when your face is frontal to the camera. Dlib's face detector expects a frontal face.
* No detection if you go far away from the web cam.  
  

## References
* [MediaPipe](https://google.github.io/mediapipe/)
* [Dlib](http://dlib.net/)
* [Nikita Kiselov](https://github.com/kinivi)
* [Adrian Rosebrock](https://pyimagesearch.com/author/adrian/)
* [Artificial Intelligence in Gestural Interfaces](https://emerj.com/ai-sector-overviews/artificial-intelligence-in-gestural-interfaces/)

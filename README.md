# Peaky Blinders CRE[AT]E App

This project was made our team, the "Peaky Blinders" for MIT's CRE[AT]E challenge in assistive technology.
 
A desktop app that turns your webcam into an AI-powered note-taker. Point your camera at a whiteboard, diagram, or page, press a button, and the model describes or transcribes what it sees. Results can be read aloud, copied to your clipboard, and are logged automatically.
 
Built with Python, Pygame, OpenCV, and the Google Gemini API.
 
## Features
 
- **Live camera preview** inside a resizable Pygame window
- **One-click capture and analysis**: freezes the frame and sends it to Gemini (`gemini-2.0-flash`) along with your prompt
- **Text-to-speech**: reads the response aloud using gTTS
- **Copy to clipboard**: copy the response with a single click
- **Editable prompt**: change what the AI is asked to do from within the app; your prompt is saved between sessions
- **Response history**: every response is appended to `history.txt`
- **Responsive layout**: buttons, preview, and text wrap rescale as you resize the window

## Requirements
 
- Python 3.9+
- A webcam
- A [Google Gemini API key](https://aistudio.google.com/app/apikey)
- Internet connection (Gemini and gTTS both need it)
### Python dependencies
 
```
google-genai
pillow
opencv-python
numpy
gTTS
pygame
pyperclip
```
 
## Installation
 
```bash
git clone https://github.com/davidgu826/AI-Vision
cd AI-Vision
pip install google-genai pillow opencv-python numpy gTTS pygame pyperclip
```
 
## Setup
 
1. **Add your API key.** In main.py, replace the placeholder:
```python
client = genai.Client(api_key='API_KEY')
```
 
## Usage
 
```bash
python main.py
```
 
### Controls
 
| Control | Action |
|---|---|
| **Capture button** (green) | Pauses the live feed, captures a frame, and sends it to Gemini. Click again to resume the live feed. |
| **Space** | Capture and analyze a frame (home screen) |
| **Speak button** | Reads the latest response aloud |
| **Copy button** | Copies the latest response to the clipboard |
| **Edit button** | Opens Edit Mode to change the prompt |
| **Quit button** | Exits the app and saves the prompt |
| **Esc** | Leave Edit Mode and return to the home screen |
| **Backspace** (Edit Mode) | Delete characters; hold to delete faster |
 
### Edit Mode
 
Type directly to modify the prompt sent to the AI. Only printable ASCII characters are supported. Press **Esc** to return home. The prompt is saved to `prompt.txt` when you quit.
 
### Default prompt
 
> If there are whiteboards, write down what is written on the board. If there are diagrams, describe the diagrams. If there are no diagrams or boards, respond with "None detected"
 
## Files Generated
 
| File | Purpose |
|---|---|
| `prompt.txt` | Your saved prompt, loaded on startup |
| `history.txt` | Log of every AI response, one per capture |
| `text.mp3` | Temporary audio file for text-to-speech |

 
## How It Works
 
1. OpenCV reads frames from the default webcam (`VideoCapture(0)`).
2. Frames are converted to RGB, wrapped as PIL images, and drawn into the Pygame window.
3. On capture, the frame and your prompt are sent to `gemini-2.0-flash` through the `google-genai` client.
4. The response is word-wrapped to the window width, displayed, and appended to `history.txt`.
5. The speak button synthesizes the response with gTTS and plays it through `pygame.mixer`.

All assets used are in the assets folder.

## Troubleshooting
 
- **Black or frozen preview**: make sure no other app is using the webcam, or change the camera index in `cv2.VideoCapture(0)`.
- **File-not-found errors on launch**: check that the `assets/` folder and its files exist.
- **API errors**: confirm your API key is valid and has access to the Gemini model.
- **No audio**: gTTS requires internet access, and your system audio output must be working.
 

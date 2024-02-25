# Audio Transcription from YouTube Videos using OpenAI's Whisper

## Description:

This Python project empowers you to seamlessly download YouTube videos, extract their audio tracks, and transcribe the speech content using OpenAI's powerful Whisper model.

## Key Features:

 - Intuitive Download: Effortlessly fetch YouTube videos in MP4 format, prioritizing high resolution and file size for optimal quality.
 - Efficient Extraction: Extract audio tracks from downloaded videos using ffmpeg, creating separate MP3 files for clarity.
 - Robust Transcription: Leverage Whisper's capabilities to transcribe the extracted audio, generating accurate and insightful text representations.
 - Clear Output: Display the transcribed text in a user-friendly format, providing valuable insights into the video's content.

## Installation:

1. Set up your environment: Ensure you have Python (version 3.6 or later) and Jupyter Notebook installed.
2. Install dependencies:
```bash
pip install -q openai pytube ffmpeg-python git+https://github.com/openai/whisper.git
```

## Usage:

1.Import necessary libraries:
```bash
import ffmpeg
import requests
import os
import openai
import pytube
import whisper
from pytube import YouTube
```

2. Define download function:
```bash
def download_video_mp4(youtube_url):
    yt = YouTube(youtube_url)
    video = yt.streams.filter(progressive=True, file_extension='mp4').order_by('resolution').desc().first()
    video.download()
    print('Video downloaded')
    return 1
```

3. Define audio extraction function:
```bash
def create_audio_file(video_filename):
    audio_filename = video_filename.replace(".mp4", ".mp3")
    stream = ffmpeg.input(video_filename)
    stream = ffmpeg.output(stream, audio_filename)
    ffmpeg.run(stream)
    return audio_filename
```

4. Download and extract audio:
```bash
# Replace "https://youtu.be/..." with the actual YouTube URL
youtube_url = "https://youtu.be/LB4MVdpajsU"
download_video_mp4(youtube_url)

video_filename = os.listdir()[1]  # Get the downloaded video filename
audio_filename = create_audio_file(video_filename)
```

5. Load Whisper model and transcribe:
```bash
model = whisper.load_model("base")

def transcribe(audio_filename):
    with open(audio_filename, 'rb') as f:
        audio_data = f.read()
    result = model.transcribe(audio_data)
    return result["text"]

yt_text = transcribe(audio_filename)
```

6. Display transcribed text:
```bash
from pprint import pprint
pprint(yt_text)
```

## Additional Notes:

- Replace "https://youtu.be/..." with the actual YouTube URL you want to transcribe.
- Experiment with different Whisper model sizes (base, medium, large) to potentially improve transcription accuracy, but be aware of the associated computational cost.

## Contributing
Contributions are welcome! If you find any bugs or have suggestions for improvements, feel free to open an issue or submit a pull request.

## License
This project is licensed under the MIT License.

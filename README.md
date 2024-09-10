# YouTube Video Transcription

This project provides a streamlined approach to downloading a YouTube video, extracting its audio, and transcribing the audio content using OpenAI's Whisper model.

## Features

- **Download YouTube Videos**: Fetches the highest resolution MP4 video from a given YouTube URL.
- **Extract Audio**: Converts the downloaded video into an MP3 audio file.
- **Transcribe Audio**: Uses OpenAI's Whisper model to transcribe the audio to text.

## Installation

To get started, you'll need to install the required Python packages. You can do this via pip:

```bash
pip install -q openai pytube ffmpeg-python git+https://github.com/openai/whisper.git
```

## Usage
### 1. Download a YouTube Video

Use the download_video_mp4 function to download a YouTube video.

```python
from pytube import YouTube

def download_video_mp4(youtube_url):
    yt = YouTube(youtube_url)
    video = yt.streams.filter(progressive=True, file_extension='mp4').order_by('resolution').desc().first()
    video.download()
    print('Video downloaded')
    return 1

download_video_mp4("https://youtu.be/LB4MVdpajsU")
```

### 2. Extract Audio from Video

Convert the downloaded MP4 video to MP3 format using ffmpeg.

```python
import ffmpeg

def create_audio_file(video_filename):
    audio_filename = video_filename.replace(".mp4", ".mp3")
    stream = ffmpeg.input(video_filename)
    stream = ffmpeg.output(stream, audio_filename)
    ffmpeg.run(stream)
    return audio_filename

create_audio_file("your_video_filename.mp4")
```

### 3. Transcribe the Audio

Use the Whisper model to transcribe the audio file.

```python
import whisper

model = whisper.load_model("base")

def transcribe(audio_path):
    result = model.transcribe(audio_path)
    return result["text"]

yt_text = transcribe("your_audio_filename.mp3")
print(yt_text)
```

## Notes
- Make sure `ffmpeg` is installed on your system. For installation instructions, visit [FFmpeg's official website](https://www.ffmpeg.org/).
- The `pytube` library downloads the video in the highest available resolution by default.
- Whisper models require a decent amount of computational resources; make sure your system meets the necessary requirements.
  
## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE.txt) file for details.

# Whisper Local YouTube Subtitle Generation Guide (No Key Required): From Beginner to AI Correction

[中文版](README.md)

* [Youtube Tutorial - Whisper YouTube Chinese Subtitle Generation Guide (No Key Required): From Beginner to AI Correction](https://youtu.be/E-X3kp8wCIg)

Rely on [whisper](https://github.com/openai/whisper) for your YouTube Chinese subtitles.

The emphasis here is on Chinese, not because it only recognizes Chinese (it certainly works for English too), but because its performance in Chinese is also excellent.

Whisper does not require any AI KEY because it performs local recognition directly through a downloaded model.

## Installation

```cmd
pip install -U openai-whisper
```

Processing videos usually requires installing ffmpeg (for handling audio/video formats). Please run the following (assuming you are on Debian/Ubuntu):

```cmd
sudo apt update && sudo apt install ffmpeg
```

## Tutorial

If you are a general user aiming for meeting transcriptions or video subtitles, using the `medium` model is sufficient.

Whisper offers various models like tiny, base, small, and large. `medium` provides a good balance between speed and accuracy.

If you seek the highest accuracy, you can try `large`, but it requires more computational resources. If resources are limited or speed is a priority, you can choose a smaller model.

Usage:

```cmd
whisper test.mkv --language Chinese --model medium --output_format srt
```

The first execution will take longer as it needs to download the `medium` model.

Here, we are only generating an srt subtitle file. It works not only for video files but also for audio files like `.mp3`.

A quick note:

If you don't have a graphics card, it is recommended to use [Google Colab](https://colab.google/).

I use a T4 GPU (free, but with daily usage limits).

![alt tag](https://cdn.imgpile.com/f/kEJuw8y_xl.png)

The `!` tells Colab that the following line is not Python code and should be executed as a command in the terminal.

![alt tag](https://cdn.imgpile.com/f/eurDpGD_xl.png)

For a video of about 10 minutes, the `CPU` took 35 minutes, while the `GPU` took 3 minutes.

In my own experience, I find the `medium` model to be more ideal, as I've noticed that `large-v3` does not handle Chinese sentence segmentation very well.

## Workflow: Whisper Draft -> AI + Prompt for Correcting Proper Nouns

I then take the generated srt subtitle file and use an AI with a prompt to correct the subtitles (I use Gemini 2.5 Pro).

I usually give it the following prompt:

```text
Please correct the following text according to the srt format, without altering the timestamps or format structure.

Help me correct the proper nouns.

This is a reference to a guide on Dify installation and connection with Ollama (Docker Compose) for RAG.

https://github.com/twtrubiks/dify-ollama-docker-tutorial/blob/main/dify.md

<I will then paste the entire srt subtitle file here>
```

I've found that this method results in highly accurate subtitles (over 90% correct).

## Comparison with Google AI Studio

I have also tried pasting a prompt directly into Google AI Studio, similar to this:

```text
You are a professional employee who generates srt subtitle files.

The format example is:

Timestamp line (00:00:00,000 --> 00:00:02,500)

This is one of the most crucial parts of an SRT file, as it precisely defines when the corresponding subtitle text should be displayed.

Format: HH:MM:SS,ms --> HH:MM:SS,ms

HH: Hours (00-99)
MM: Minutes (00-59)
SS: Seconds (00-59)
ms: Milliseconds (000-999) - Note: The SRT standard uses a comma (,) as the separator between seconds and milliseconds.
-->: Separator, indicating from "start time" to "end time".
00:00:00,000 (Start time):

This indicates that the corresponding subtitle text should start appearing on the screen at the precise moment of 0 hours, 0 minutes, 0 seconds, and 0 milliseconds into the video.
00:00:02,500 (End time):

This indicates that the corresponding subtitle text should disappear from the screen at the precise moment of 0 hours, 0 minutes, 2 seconds, and 500 milliseconds into the video.

- If a paragraph is too long, the subtitle should not exceed three seconds.
```

However, I found the error rate to be very high. Sometimes the srt format was incorrect, or a subtitle segment towards the end would be excessively long.

After all, it's better to leave professional tasks to a tool like Whisper.

## Generating Subtitles for a Specific Time Range

If you only want to generate subtitles for a specific interval, you can use `ffmpeg` to cut the video:

```cmd
ffmpeg -i test.mkv -ss 5 -to 8 -c copy clipped_test.mkv
```

-ss 5: Start time, from the 5th second of the video (5s = 0:05)

-to 8: End time, until the 8th second of the video (8s = 0:08)

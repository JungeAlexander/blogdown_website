---
title: 'Using a Raspberry Pi as a sound-activated recorder'
author: 'Alexander Junge'
date: '2020-06-13'
slug: rbpi-sound-detector-poc
categories:
  - Raspberry Pi
tags:
  - Python
draft: false
---

Using a [Raspberry Pi 4 Model B](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/) and
a [Seeed ReSpeaker Mic Array v2.0](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/) connected via USB,
I built a simple sound-activated recorder.
The Raspberry Pi records one-minute sound snippets at a time and, using a very simple heuristic,
detects sound in the snippet.
Snippets where no sound was detected are deleted.
Snippets where sound was detected are converted to MP3 by a scheduled job and kept.
This was a fun PoC project that could be extended in many directions.

There are two script enabling this recording. 
The first script, called `record_chunks.py`, records the sound snippets as WAV files and names
them after the date and start time of the recording. Some minor error handling is
included to retry the recording three times if it fails:

```python
from datetime import datetime
import os
import subprocess
import sys
import time

data_dir = "data"

while True:
    start_time = datetime.now()
    output_dir = os.path.join(data_dir, start_time.strftime("%Y%m%d"))
    os.makedirs(output_dir, exist_ok=True)
    output_file = os.path.join(
        output_dir, start_time.strftime("%Y%m%d-%H-%M-%S") + ".wav"
    )
    
    for attempt in range(3):
        print(f"Started recording to {output_file} (attempt {attempt}).")
        try:
            subprocess.run(f"arecord -D plughw:2 -d 60 -f cd {output_file}", shell=True, check=True)
        except subprocess.CalledProcessError:
            time.sleep(60)
        else:
            print(f"Finished recording to {output_file}.")
            break
    else:
         print(f"Recording to {output_file} failed. Exiting.")
         sys.exit(1)
```

The second script, called `filter_chunks.py` detects sounds and converts WAV to MP3.
The sound detection is VERY simple and just looks for a signal of certain strength in
the WAV file. To be honest, I am unsure how much sense this heuristic makes but it
looked like an okay starting point after I plotted some of the WAV signals.
This script is intended to run periodically, e.g. every 5 minutes using a cron job:

```python
from datetime import datetime
from glob import glob
import subprocess
import os

import numpy as np
from scipy.io.wavfile import read as read_wavfile

data_dir = "data"

start_time = datetime.now()
output_dir = os.path.join(data_dir, start_time.strftime("%Y%m%d"))
glob_pattern = os.path.join(output_dir, "*.wav")

def get_max_absolute_signal(wav_file):
    samplerate, data = read_wavfile(wav_file)
    return np.abs(data).max()

for wav_file in glob(glob_pattern):
    max_signal = get_max_absolute_signal(wav_file)
    if max_signal < 20000:
        os.remove(wav_file)
    else:
        mp3_file = wav_file[:-len(".wav")] + ".mp3"
        subprocess.run(f"lame --preset standard {wav_file} {mp3_file}", shell=True, check=True)
        os.remove(wav_file)
```

All code can be found on [Github](https://github.com/JungeAlexander/langdev).
For example, I am considering to extend this further and train a machine learning model to
differentiate different kinds of sounds.
This would require a manually labeling a few sound snippets first.
### How to Transcribe Audio to text (Google Colab Version)👇

### Step 1: Clone the Fairseq Git Repo

```
import os

!git clone https://github.com/pytorch/fairseq

# Get the current working directory
current_dir = os.getcwd()

# Create the directory paths
audio_samples_dir = os.path.join(current_dir, "audio_samples")
temp_dir = os.path.join(current_dir, "temp_dir")

# Create the directories if they don't exist
os.makedirs(audio_samples_dir, exist_ok=True)
os.makedirs(temp_dir, exist_ok=True)


# Change current working directory
os.chdir('fairseq')

!pwd
```
### Step 2: Install requirements and build
Be patient, takes some minutes

```!pip install --editable ./ ```

### Step 3: Install Tensor Board
```!pip install tensorboardX```

### Step 4: Download your preferred model
Un comment to download any
```
# # MMS-1B:FL102 model - 102 Languages - FLEURS Dataset
# !wget -P ./models_new 'https://dl.fbaipublicfiles.com/mms/asr/mms1b_fl102.pt'

# # MMS-1B:L1107 - 1107 Languages - MMS-lab Dataset
# !wget -P ./models_new 'https://dl.fbaipublicfiles.com/mms/asr/mms1b_l1107.pt'

# MMS-1B-all - 1162 Languages - MMS-lab + FLEURS + CV + VP + MLS
!wget -P ./models_new 'https://dl.fbaipublicfiles.com/mms/asr/mms1b_all.pt'
```

### Step 5: Upload your audio(s)
Create a folder on path '/content/audio_samples/'  and upload your .wav audio files that you need to transcribe
e.g. '/content/audio_samples/small_trim4.wav'
**Note:** You need to make sure that the audio data you are using has a sample rate of 16000
You can easily do this with FFMPEG like the example below that converts .mp3 file to .wav and fixing the audio sample rate
```
ffmpeg -i .\small_trim4.mp3 -ar 16000 .\wav_formats\small_trim4.wav
```


### Step 6: Run Inference and transcribe your audio(s)
Takes some time for long audios
```
import os

os.environ["TMPDIR"] = '/content/temp_dir'
os.environ["PYTHONPATH"] = "."
os.environ["PREFIX"] = "INFER"
os.environ["HYDRA_FULL_ERROR"] = "1"
os.environ["USER"] = "micro"

!python examples/mms/asr/infer/mms_infer.py --model "/content/fairseq/models_new/mms1b_all.pt" --lang "swh" --audio "/content/audio_samples/small_trim4.wav"
```

After this you'll get your preffered transcription
I have this Collab Example in my GitHub Repo👉 [fairseq_meta_mms_Google_Colab_implementation](https://github.com/epk2112/fairseq_meta_mms_Google_Colab_implementation)




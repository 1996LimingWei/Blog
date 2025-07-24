# When we run the project, following error occurred:
```
Python error: WARNING: ffmpeg not found. The downloaded format may not be the best available. Installing ffmpeg is strongly recommended: https://github.com/yt-dlp/yt-dlp#dependencies

Python error: Traceback (most recent call last):
  File "/Users/leo/Desktop/Project/FunTrip/server/scripts/yt_downloader.py", line 13, in <module>

Python error:     audioUrl = info['entries'][0]['requested_formats'][1]['url']
KeyError: 'requested_formats'
```

### Step 1: locate the error file

Since the issue arises from /Users/leo/Desktop/Project/FunTrip/server/scripts/yt_downloader.py, we locate the file
We also need to manually set songName for 
```
# songName = sys.argv[1]
songName = "Payphone"
```


### Step 2: Debug locally

cd to the file dir and run the yt_downloader.py using python：
```
(py39) leo@MacBookPro scripts % python yt_downloader.py
WARNING: ffmpeg not found. The downloaded format may not be the best available. Installing ffmpeg is strongly recommended: https://github.com/yt-dlp/yt-dlp#dependencies
Traceback (most recent call last):
  File "/Users/leo/Desktop/Project/FunTrip/server/scripts/yt_downloader.py", line 14, in <module>
    audioUrl = info['entries'][0]['requested_formats'][1]['url']
KeyError: 'requested_formats'
```
It shows the same error as we saw before, we set a break point there to debug. 

### Step 3: install needed package

Install ffmpeg in terminal (py39) leo@MacBookPro FunTrip % brew install ffmpeg does not work because it says:
```
Error: unknown or unsupported macOS version: :dunno
```
We need to install the binary downloaded directly from official website: https://evermeet.cx/ffmpeg/ (This is for macOS)

Then we give proper permission to allow this in macOS. Then, we run 
```
sudo mv ~/Downloads/ffmpeg /usr/local/bin/ffmpeg
sudo chmod +x /usr/local/bin/ffmpeg
```
in terminal to move the binary to system's PATH.

### Step 4: Validate

Now we can see the error is gone
<img width="577" height="189" alt="截屏2025-07-24 12 20 13" src="https://github.com/user-attachments/assets/aad2aae3-f2ea-4305-a7b2-eb18d425a93c" />



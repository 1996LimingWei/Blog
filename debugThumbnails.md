# How to debug in a react project
When I worked on:https://github.com/siwu-945/FunTrip/pull/43, I found a issue where the thumbnail of searched songs not shown in the research result.

<img width="363" height="414" alt="截屏2025-07-20 13 56 45" src="https://github.com/user-attachments/assets/a91215af-41a0-4a13-8b72-134c34257c0b" />

I followed this steps to debug the issue.

## Step 1 locate the behind-the-scene cause of the issue

In this case, the frontend user sees thumbnail of searched result being invalid. We know from [offical doc](https://github.com/yt-dlp/yt-dlp) that thumbnail can be retrived using thumbnail URL and it is one of the attributes of returning result of a song's yt-dlp audioURL.


## Step 2 locate the code section corresponding to the cause
Where do we get the thumbnail URL? From codebase, we can search thumbnail in the explorer, then we find that we are trying to get it in server/scripts/yt_search.py. 

<img width="641" height="393" alt="截屏2025-07-20 14 07 55" src="https://github.com/user-attachments/assets/557f792a-1de0-4dcb-854c-d7830e6ee9f2" />

So the question now is, did we really get the thumbnail URL in this part of the code?

## Step 3 Use IDE's debugger to debug

After locating the code, we can set breakpoints in the place to valid our question above. We set a break point at results.append(result) to see if the entry returning from yt-dlp is actually having our thumbnail URL.

<img width="472" height="371" alt="截屏2025-07-20 14 17 12" src="https://github.com/user-attachments/assets/e569daed-707a-413f-9fbc-d50027531b83" />

At the breakpoint, we can see the thumbnail URL is retrived, but it has a different name: "thumbnails", not thumbnail. So this explain why our thumbnail key did not get any data.

Now, we can fix the "thumbnail" to "thumbnails" and resolve the issue.
Here is the fix: https://github.com/siwu-945/FunTrip/pull/47

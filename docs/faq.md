<!-- docs/faq.md -->
## Having trouble logging in?
Almost 100% of the time, the root cause of this is something not right in your local browser cache. To bypass your browser cache, simply open a new Chrome incognito window or whatever the equivilent is called with the browser you're using and try logging in there. If that does work, but the normal way didn't, then learn how to clear your cache you retard.


## Media stream buffering/glitching?
![fas-wifi](/_media/wifi.svg)
In your browser, click the settings cog button in the lower screen underneith the progress bar, ensure the `QUALITY` dropdown menu is set to `AUTO`, and click `Playback Data`. Under Playback Info, determine the play type displayed next to `Play method:`.
If it's `DirectPlay`, this means the server is streaming the direct media file to your computer without a problem, however the culript causing intermittent buffering/glitching is most likely due to you not having enough bandwidth to support the playback of the media file.

![Settings](/_media/cog.png) ... ![](/_media/playbackdata.png)

You can also get a sense for how much bandwidth is required by looking at the video's bitrate info which is available by clicking on the ellipsis of the media's image poster, then clicking `Media Info`.

![ellipsis](/_media/ellipsis.png) ... ![bitrate](/_media/bitrate.png)

The server (the web UI) actually does have logic which determines that your network's ingress bandwidth is insufficient to play the video and will automatically start trying to transcode the file from it's original form into a format which is more compatible with your device. When this does happen, video's will have a delay of a second or two before playing normally. Often more reasons for the server deciding to transcode the media is found under `Play method:` discussed fruther above. If it's actively transcoding, the Play method will state `HLS` and not `DirectPlay`.


## Media won't play?
* First, try to eliminate the obvious reasons such as using a device so old that it really should be thrown away. If you're using old shit, that's on you.
* Go to [librespeed.org](https://librespeed.org/) or [fast.com](https://fast.com/) in another browser tab and run a speedtest and note how much bandwidth you have at your disposal dedicated to downloading, the uploading part doesn't mean shit. Make sure that your connection is idle while you test, otherwise you won't get even remotely accurate results unless you or your ISP is using QoS.
* You will probably be fine with 25 Mbps for most video's, however I know for a fact there are some huge files including 4K content which requires a download speed of at least 50-75 Mbps. General rule of thumb to abide by, if the contents bitrate => 10,000 kbps, then it is going to require a fat pipe.

  
The server actually does have logic which determines that your network's ingress bandwidth is insufficient to play the video and will automatically start trying to transcode the file from it's original form into a format which is more compatible with your device. When this does happen, video's will have a delay of a second or two before playing normally. Often more reasons for the server deciding to transcode the media is found under `Play method:` discussed fruther above. If it's actively transcoding, the Play method will state `HLS` and not `DirectPlay`.


## Subtitles not loading?
🎬 This problem mostly occurs with episodes of TV shows/series, but could happen to some movies as well. After you have selected the subtitle and it fails, ***wait about 30-45 seconds and try selecting the same subtitle again.***
The root issue results from the server having to retrieve the entire media file from the data source before the subtitle stream can be extracted and pushed out to the client. Most of the TV episodes are large files so it takes about 30 seconds to pull it down at between 250-1000 Mbps.


## Supported devices or platforms?
* PC, Mac, Linux with modern browser such as Google Chrome/Chromium, Firefox, Microsoft Edge, or Safari
  * Windows (7/8/10), Linux kernel 2.4 or higher, MacOS
* All modern smartphones/tablets with HTML5 supported browsers listed above
* Amazon Fire TV Stick ([native Jellyfin app](https://www.amazon.com/Jellyfin/dp/B081RFTTQ9/))
* Roku ([native Jellyfin app](https://channelstore.roku.com/details/cc5e559d08d9ec87c5f30dcebdeebc12/jellyfin))
* Kodi 19+ ([native Jellyfin plugin](https://jellyfin.org/posts/kodi-0-5-0/))


## What is transcoding?
Transcoding is a compute-resource-heavy procedure the media server can choose to run which converts the source video or audio codec of a media file to be as compatibile as possible with whatever device you're watching on. Jellyfin tends to not like ***OLD*** Apple products, so avoid those if you can. Newer Apple devices are fine. Chrome/Chromium works fine on every device I've tried it on in addition to the official Jellyfin app for Amazon Fire TV Stick.

There are only 3 posible status' for what the server is doing to your video.
  1. Direct Play
  2. Direct Stream (quick media container re-packaging task/not even noticeable)
  3. HLS (video/audio codec decoding and re-encoding (transcoding) task---compute-intensive process consuming ~50-99% cputime)


## How to stream multiple versions of a film?
For some movies and tv shows there are multiple versions of the same film available to stream. Additionally for other films, there are multiple versions of the same film but with a distinctive difference, such as REMASTERED EDITION, Alternate Ending, Director's Cut, Theatrical Edition etc..There are also multiple versions of films in which the only difference is movie quality/aspect size like 720p vs 1080p vs 2160p (4K).

You'll notice for the movie, Avatar (2009), it has 4 different versions available, as can be noted from the yellow number 4 in the top left corner of the movies poster.

![Avatar](/_media/avatar.png) ![Avatar2](/_media/avatar2.png)

Another example is `I Am Legend (2007)` with an optional ***Alternate Ending***.

![Alternate_Ending](/_media/alternate_ending.png)


## Media storage info?
Over 16 terabytes as of 01/28/2021. Three words for you, Google Drive and eBay. Total cost = $4 * 3 for redundancy.
![Google Drive Storage](/_media/gdrive-storageused.png)
> Google Cloud Console API Metrics for Google Drive
![Google Cloud API rates](/_media/gcloud-01.png)
![Google Cloud API rates](/_media/gcloud-02.png)

If you have multiple Google Drive accounts, server-side copy/sync operations are supported with Rclone even if your remote is crypted:

![Google Drive server-side copy operationg with Rclone](/_media/rclone-serversidecopy.png)


## Where is the status page?
[status.travisflix.com](https://status.travisflix.com/)

Uptime over the standard 30 day time period is: 99.989%

![TravisFlix Uptime](/_media/travisflix_uptime.png)


## Status of the media sync processes?

The badges below show a real-time representation of the media encryption and syncing processes between the origin and destination Google Drive accounts. These GDrive accounts are what feed the video files to the server which then streams to your eyeballs.

  1) OVERALL STATUS
  2) `rclone-sync-video`: Rclone media sync operation which encrypts media and pushes to Google Drive aka--"GCrypt"
  3) `rclone-sync-p0ds0smb`: Rclone data sync operation to push/pull encrypted **NON**-media data to GCrypt (FreeNAS pools > Encrypt > Google Drive)
  4) `rclone-servercopy`: Rclone server-side sync job from gdrive-usmba:gcrypt to gdrive-gdrive01dvecs:gcrypt and gdrive-gdrive02dvecs:gcrypt
  5) `rclone-servercopy-media`: Server-side media-only rclone sync operation which pushes changes to multiple GDrive accounts for redundancy

      | Type    | Up, late, or down | Up, down |
      |------------------------|------------------------|------------------------|
      | 1. Overall status | ![Custom badge](https://img.shields.io/endpoint?url=https%3A%2F%2Fhealthchecks.io%2Fbadge%2Fbd39864f-49a7-4916-b223-8e2ac5%2Fdc1ecPD8.shields) | ![Custom badge](https://healthchecks.io/badge/bd39864f-49a7-4916-b223-8e2ac5/dc1ecPD8-2.svg) | 
      | 2. rclone-sync-video | ![Custom badge](https://img.shields.io/endpoint?url=https%3A%2F%2Fhealthchecks.io%2Fbadge%2Fbd39864f-49a7-4916-b223-8e2ac5%2Fgp7x30yH%2Frclone-sync-video.shields) | ![Custom badge](https://img.shields.io/endpoint?url=https%3A%2F%2Fhealthchecks.io%2Fbadge%2Fbd39864f-49a7-4916-b223-8e2ac5%2Fgp7x30yH-2%2Frclone-sync-video.shields) |
      | 3. rclone-sync-p0ds0smb | ![Custom badge](https://img.shields.io/endpoint?url=https%3A%2F%2Fhealthchecks.io%2Fbadge%2Fbd39864f-49a7-4916-b223-8e2ac5%2FF-VXXpPQ%2Frclone%25252Ffreenas.shields) | ![Custom badge](https://healthchecks.io/badge/bd39864f-49a7-4916-b223-8e2ac5/F-VXXpPQ-2/rclone%252Ffreenas.svg) |
      4. rclone-servercopy | ![Custom badge](https://img.shields.io/endpoint?url=https%3A%2F%2Fhealthchecks.io%2Fbadge%2Fbd39864f-49a7-4916-b223-8e2ac5%2FfDZmnLdj%2Frclone-servercopy.shields) | ![Custom badge](https://img.shields.io/endpoint?url=https%3A%2F%2Fhealthchecks.io%2Fbadge%2Fbd39864f-49a7-4916-b223-8e2ac5%2FfDZmnLdj-2%2Frclone-servercopy.shields) |
      5. rclone-servercopy-media | ![Custom badge](https://img.shields.io/endpoint?url=https%3A%2F%2Fhealthchecks.io%2Fbadge%2Fbd39864f-49a7-4916-b223-8e2ac5%2FdF-MGYh3%2Frclone-servercopy-media.shields) | ![Custom badge](https://img.shields.io/endpoint?url=https%3A%2F%2Fhealthchecks.io%2Fbadge%2Fbd39864f-49a7-4916-b223-8e2ac5%2FdF-MGYh3-2%2Frclone-servercopy-media.shields) |

      ~~~ json
      Overall status:
      https://healthchecks.io/badge/bd39864f-49a7-4916-b223-8e2ac5/dc1ecPD8.json
      rclone-video-sync:
      https://healthchecks.io/badge/bd39864f-49a7-4916-b223-8e2ac5/gp7x30yH/rclone-sync-video.json
      rclone-sync-p0ds0smb:
      https://healthchecks.io/badge/bd39864f-49a7-4916-b223-8e2ac5/F-VXXpPQ/rclone%252Ffreenas.json
      ~~~


## Looking for a way to donate?
Bitcoin (₿) would be preferred over USD at the address below. Any denomination would be greatly appreciated.

> Bitcoin current price: ***<iframe src="//btc.travisflix.com" width="100%" height="35px" style="border:0px;overflow-y:hidden;" scrolling="no"></iframe>***

<!--<iframe src="gobitcoin.io_price.html" width="100%" style="border:0px;"></iframe> -->

* Operating expense to run travisflix.com on Linode PaaS = $10 per month
  * however, I don't pay myself...
  * if I were to pay for cloud storage legitimately, that cost would easily hit $200/month---$2400/year

> ***Option #1***<br>
>
> [![GreenAddress Permanent Payment URL](/_media/greenaddress-donate-green.png)](https://greenaddress.it/pay/GA2GtnSV73LMTzpauEKZsJnRD1yxWf/)<br>
> [32Z8bRQPcip4avcGWujSqLGAmDt52m1Wy4](bitcoin:32Z8bRQPcip4avcGWujSqLGAmDt52m1Wy4)
>
> Here is the related QR code associated with my BTC address beginning with "32Z8bRQ":<br>
> ![BTC](/_media/bitcoin_32Z8bRQPcip4avcGWujSqLGAmDt52m1Wy4.png)
---
> ***Option #2***<br>
>
> [bc1q690p3utevcus3mscnq5anegz7a3m7cjv4vvd6g](bitcoin:bc1q690p3utevcus3mscnq5anegz7a3m7cjv4vvd6g)
> 
> Here is the related QR code associated with my BTC address beginning with "bc1q690":<br>
> ![BTC](/_media/bc1q690p3utevcus3mscnq5anegz7a3m7cjv4vvd6g.png)


## Still have questions?
Send me an email at <travisrunyard@gmail.com> or join the Telegram room at [t.me/travisflix](https://t.me/travisflix). My contact information is also available online at [travisyard.run](https://travisyard.run/).

---

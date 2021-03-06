<!-- docs/faq.md -->
## Having trouble logging in?

:far fa-frown: Usually the cause of this is an issue with the cookies in your local browser cache. To test, bypass the browser cache by opening a new incognito window :fas fa-user-secret: or whatever the equivilent is called with the browser you are using and try to login there. If that works but the normal way did not, then [clear your browser cache/cookies](https://support.google.com/accounts/answer/32050?co=GENIE.Platform%3DDesktop&hl=en) :fas fa-cookie-bite:.

If you would like to help keep the site running, go to [How to Support/Donate?](#how-to-supportdonate).

> Make sure that you checkmark cookies from the advanced section. Be aware that **this action will delete cookies for all sites instead of only travisflix.com**.

![browser-cache-01](_media/browser-cache-01.png)
![browser-cache-02](_media/browser-cache-02.png)

> If you feel the issue resides on the server-side, [check the status page](#where-is-the-status-page) and/or the [real-time server statistics](#do-you-have-server-statistics).


## Media stream buffering/glitching?

### ![wifi-users](_media/wifi-20x16.png) Wireless Users

:performing_arts: I wanted to include the following tip at the very top of this section because I had this exact problem. If you are a WiFi user, the first troubleshooting step you can perform on your own is to get off the wireless and hardwire yourself in with a standard CAT 5E/6 :fas fa-ethernet: ethernet cable. If you only have CAT 5, throw that shit away.

If you insist on using WiFi, then you are going to need to ensure that you are connected to a SSID that uses **802.11ac** and is using a **wide band such as 80 or 160**.

> The **802.11n wireless standard can utilize both 2.4 and 5 Ghz frequencies**. Just because you assume you are on 5 Ghz (*by connecting to a SSID with "-5G" appended*), that doesn't technically indicate you are on 802.11ac.

![wifi-standards](_media/wifi-standards-quick-comparison-table.jpg)


### ![wired-users](_media/network-wired-20x16.png) Hardwire Users

1) During video playback—In your browser click the settings/cog (:fa fa-cog:) button underneith the progress bar to the right of the video controls
2) Ensure **QUALITY** is set to **AUTO**
3) Click `PLAYBACK DATA`
4) Under Playback Info—Determine the play type displayed next to **Play method:**
5) If it's **"DirectPlay"**, this means the server is streaming the direct media file to your computer without a problem, however the cause of intermittent buffering/glitching could be due to you not having enough bandwidth to support the playback of the media file. This doesn't necessarily mean your ISP connection doesn't have enough bandwidth, but could mean:
    * Your connection to your home router or switch has an issue
    * The available bandwidth is being consumed by another user (ie. Bittorrent without speed limits defined)
    * Your internet traffic is being shaped by your ISP with observable negative impact on its customers
    * There's probably many more, I just can't think of any right now
    * See [Video not streaming?](#video-not-streaming) below for help running a bandwidth test

![Playback](_media/playbackdata.png)

You can also get a sense for how much bandwidth is required by looking at the video's bitrate info which is available by clicking on the ellipsis of the media's image poster, then clicking Media Info.

![ellipsis](_media/ellipsis.png) ... ![bitrate](_media/bitrate.png)


## Video not streaming?

:game_die: First, try to eliminate the more obvious culprits such as using an old device, system, operating system, or browser.

If you would like to help keep the site running, go to [How to Support/Donate?](#how-to-supportdonate).

> The streaming software (Jellyfin) does have logic which determines that your networks ingress (download) bandwidth is insufficient to stream the video playback and will automatically start attempting to [transcode](https://en.wikipedia.org/wiki/Transcoding) the media from its original format into a format that is more compatible with your device. When this happens you will have a delay of at least a few seconds before playback (usually between 5-15 seconds). Additional reasons for transcoding can be found under **"Play method:"** discussed further above. If it's transcoding, the play method will state **HLS** and not **DirectPlay**.

1. [Check if multiple versions are available](#how-to-stream-multiple-versions-of-a-film) of the stream you are having trouble with.
    * If multiple versions exist then try selecting the version in the drop-down menu which was not selected by default, then hit Play.
    * Versions with **RARBG** in the suffix is recommended if the other is not working for you.
    
    ![multiple versions](_media/multiple-vers.png)
    
2. Make sure that your network connection is idle (at the router WAN port, not only your PC if you have multiple devices), otherwise results will be inaccurate.
3. Open [speed.travisflix.com](http://speed.travisflix.com/) (tests speed directly from streaming server), or [fast.com](https://fast.com/) and run a few tests.
4. Note how much bandwidth you have at your disposal dedicated to downloading (The **Download** bandwidth is the only important factor).
5. As a general guideline, you should have a minimum of 20 Mbps of banwdidth available at your disposal for the download stream (called ingress), however there are some large media files including 4K content which requires a faster download speed of 40+ Mbps.
    * Testing has shown the average bandwidth required for playback of a 100MB sample video is 11.74 Mbps

    ```shell-session
    vnstat -i enp4s0 -tr 30
    44816 packets sampled in 30 seconds
    Traffic average for enp4s0

    rx        11.74 Mbit/s           993 packets/s
    tx       337.67 kbit/s           499 packets/s
    ```

    * Further testing has shown that during the loading of a video stream, your device tries to download almost as much as possible probably to buffer the video more than normal, and under the remainder of the stream you really only use an average of 4–7 Mbps (however about every 30s, the device downloads and buffers the video using a bandwidth of 30–50 Mbps)

    ![Video Stream Bandwidth Graph](_media/network-bandwidth-video-stream.png)
    ![Video Stream Bandwidth Graph 2](_media/network-bandwidth-video-stream2.png)

6. General rule of thumb—if the content's bitrate > 10,000 Kbps it is going to require a faster connection. Maybe this is the time to upgrade to Frontier/Fios? 

```shell-session
$ speedtest-cli
Retrieving speedtest.net configuration...
Testing from Frontier Communications.
Retrieving speedtest.net server list...
Selecting best server based on ping...
Hosted by Whitesky Communications LLC (Los Angeles, CA) [29.64 km]: 16.822 ms
Testing download speed ......................................................................................
Download: 79.41 Mbit/s
Testing upload speed ........................................................................................
Upload: 122.33 Mbit/s
```


## Subtitles not loading?

:far fa-closed-captioning: This problem mostly occurs with TV show episodes, but could occur with some movies as well. After you have selected the subtitle and it fails, wait about 60 seconds and re-select the same subtitle again.
The root cause results from the server needing to retrieve the entire media file from the data-source before the subtitle stream can be extracted and pushed out to the client. Most TV episodes are large media files and so it will take roughly 30–60 seconds to download the entire episode. In rare cases it may take even longer than 60 seconds, especially if the server is heavily loaded.


## Media Codec Information & Standards

> Video: **[H.264 (AVC)](https://www.streamingmedia.com/Articles/Editorial/What-Is-.../What-Is-H.264-74735.aspx)** / Audio: **[AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) 5.1CH**

Virtually all media on the site will be using the video/audio codec specs listed below.
The actual media file itself may be in either the standard "mp4" ([Mpeg-4](https://en.wikipedia.org/wiki/MPEG-4_Part_14)), or "mkv" ([Matroska](https://en.wikipedia.org/wiki/Matroska)), which are referred to as containers. Containers themselves have nothing to do with the codec used for the video and audio streams. Containers can be thought of as universally recognized digital media delivery packages. However despite this understanding, shitty media handling implementation by the major mobile OS developers (you know the ones I'm talkin' about) may force the server to strip the container off of the internal media streams before repackaging and delivering it to mobile devices in a format more easily consumed, such as "ts" ([Transport Stream](https://en.wikipedia.org/wiki/MPEG_transport_stream)).

> A transport stream encapsulates a number of other substreams, often packetized elementary streams (PESs) which in turn wrap the main data stream using the MPEG codec or any number of non-MPEG codecs (such as AC3 or DTS audio, and MJPEG or JPEG 2000 video)

|Video||
|-----|-----|
| Format: | AVC |
| Format/Info: | Advanced Video Codec |
| Format profile: | High@L4.1 |
| Format settings: | CABAC / 4 Ref Frames |
| Codec ID/Info: | Advanced Video Coding |
| Bit rate: | 2 500 kb/s |
| Frame rate mode: | Constant |
| Frame rate: | 23.976 (23976/1000) FPS |
| Color space: | YUV |
| Bit depth: | 8 bits |
| Scan type: | Progressive |
| Writing library: | x264 core 152 r2851M ba24899 |
|**Audio**||
| Format: | AAC LC |
| Format/Info: | Advanced Audio Codec Low Complexity |
| Codec ID: | mp4a-40-2 |
| Bit rate mode: | Constant |
| Bit rate: | 224 kb/s |
| Channel(s): | 6 channels |
| Channel layout: | C L R Ls Rs LFE |
| Sampling rate: | 48.0 kHz |
| Frame rate: | 46.875 FPS (1024 SPF) |
| Compression mode: | Lossy |


## Supported devices or platforms?

> Official documentation: [jellyfin.org/docs/general/clients/index.html](https://jellyfin.org/docs/general/clients/index.html).

* :fas fa-desktop: PC/Mac/Linux with modern browser such as Chrome/Firefox/Edge/Safari
  * :fab fa-windows: Windows 7-10, :fab fa-linux: Linux kernel 2.x+, :fab fa-apple: MacOS 10.12–current
* :fas fa-mobile-alt: Smartphones (:fab fa-android:/:fab fa-apple:/:fab fa-windows:) and tablets with modern browsers
* :fab fa-amazon: Amazon Fire TV Stick ([native Jellyfin app](https://www.amazon.com/Jellyfin/dp/B081RFTTQ9/))
* :fas fa-tablet-alt: Roku ([native Jellyfin app](https://channelstore.roku.com/details/cc5e559d08d9ec87c5f30dcebdeebc12/jellyfin))
* :fas fa-tablet: Kodi 19+ ([native Jellyfin plugin](https://jellyfin.org/posts/kodi-0-5-0/))


## Downloading movies & shows?

Downloading media is now **limited to 3 simultaneous** downloads at a time per IP address. Recently, the server has been overloaded when processing many concurrent downloads, so I didn't really have a choice but to use connection limiting on the webserver.
Logging in with multiple browsers will not give you additional download ability, because the limit is per IP address. Using third party download tools will also not increase the download speed. The download speed is not capped or limited in any way. The bottleneck is the servers CPU resources available for decrypting the media from the datasource. As I have already disclosed the datasource in the [Media storage info?](#media-storage-info) section, the datasource is indeed Google Drive.

If you would like to help keep the site running, go to [How to Support/Donate?](#how-to-supportdonate).

```
limit_conn_zone $binary_remote_addr zone=addr:10m;

location ~ ^/Items/(.*)/Download$ {
        proxy_pass http://jellyfin_server;
        limit_conn addr 3; # Number of simultaneous downloads per IP
        limit_conn_status 429;
        proxy_buffering on;
}
```


## Remote control of shared devices?

Sorry, feature had been broken ever since I applied network-level firewall rules. You should now be able to control your other devices as long as you are logged in with the same account. Problem was inbound UDP/1900 for DLNA not open, whoopsie daisy. [TravisFlix](https://travisflix.com) traverses two independent reverse proxies as well so things can get a little confusing and hard to keep track of for me.

If you would like to help keep the site running, go to [How to Support/Donate?](#how-to-supportdonate).

1. Log in with your browser first
2. Log in with the device you are wanting to remotely control (must not be identicle to the controlling device, needs separate internal ID's)
3. Immediately after step 2, click the icon highlighted in the image below
4. Then click on the device you are wanting to control
5. Once that icon displays the controlled device next to it, search/browse the desired media and click play like normal
6. It should now be playing what you clicked on your remotely controlled device

![Remote Control 01](_media/remote-control-01.png)

![Remote Control 02](_media/remote-control-02.png)


## Can I upload files?

:fas fa-cloud-upload-alt: Indeed.

> Login with these credentials: **username = travisflix / password = upload**

1. Go to [upload.travisflix.com](https://upload.travisflix.com)
2. Either click where it says to, or use file explorer and drag-and-drop
    * You can select multiple files per upload session
    * The maximum file size **upload limit is 5 GB**
    * All uploaded files are automatically deleted after 24 hours
3. Have a beer and wait
4. When the upload is complete, use the handy email form presented to you
    * Under "*Your friend's email*" enter the email address <travisrunyard@gmail.com>
    * In the event you have a catastrophic case of trigger-finger and have skipped the step above, send me an email to let me know what you have uploaded (date/time of upload would be helpful)<br>
    ![upload email](_media/upload.travisflix.com-email.png)


## VPN remote users: allowed countries?

:fas fa-globe-americas: VPN users: make sure that you are **connecting from** one of these countries.

:triangular_flag_on_post: Countries allowed to access the site:

1. United States :toilet: :fas fa-beer: :fas fa-cannabis: :fas fa-flag-usa:
2. Canada :fas fa-cannabis: :mute:
3. Switzerland :children_crossing:
4. Sweeden :no_smoking:
5. Netherlands :fas fa-cannabis: :syringe:
6. Singapore :fas fa-transgender-alt:
7. Mexico :fas fa-pepper-hot:
8. Finland ![Finland flag](_media/finland-flag-sm.png)
9. Ireland ![Ireland flag](_media/ireland-flag-sm.png)
10. United Kingdom :fas fa-pound-sign:


If you do not connect from these countries, you will see:

![Cloudflare error 1020](https://travisflix.com/help/_media/error1020.jpg)

If you would like to request another country be allowed, join the Telegram room at [t.me/travisflix](https://t.me/travisflix) or contact me directly (no promises) <travisrunyard@gmail.com>.

If you would like to help keep the site running, go to [How to Support/Donate?](#how-to-supportdonate).


## Video not filling the screen?

* :movie_camera: Movies are composed in a variety of shapes, called aspect ratios. Most of these aspect ratios do not match the exact aspect ratio of your widescreen TV or computer monitor. Most older movies were made primarily in the 1.37:1 aspect ratio. This means that the image is 1.37 times as wide as it is high.
* A typical widescreen HDTV set has an aspect ratio of 1.78:1. This means that it is 1.78 times as wide as it is high. This ratio was determined several years ago by looking at all the aspect ratios in use and 1.78:1 fit every aspect ratio within its borders in some way. Very few movies were ever produced in 1.78:1 (the most notable being Toy Story), so you are going to see black bars on many movies that are shown in their original aspect ratio.
* Films with an **aspect ratio of less than 1.78:1 will have black bars displayed on the sides of a widescreen HDTV. A movie with an aspect ratio greater than 1.78:1 will have black bars at the top and bottom.** :left_right_arrow:


## How to stream multiple versions of a film?

:fas fa-signal: Some movies/shows have multiple versions available to stream. These versions will either state different qualities (1080p vs 720), or there can be special edititions such as: remastered editions, alternate endings, directors cuts, theatrical editions etc.

:fas fa-info-circle: *Notice for the movie "Avatar" there are four different versions available as indicated by the yellow circle with number 4 in the top left corner.*

![Avatar](_media/avatar3.png)

Apocolypse Now has 3 distinct editions available:

![versions](_media/versions.png)


## Plain-text media list/index

These plain-text files contain an index of all media for which it corresponds. The files are regenerated every hour 24/7/365 for the most up-to-date media info.

:fas fa-list: [/movies.txt](https://travisflix.com/movies.txt)<br>
:fas fa-list: [/shows.txt](https://travisflix.com/shows.txt)<br>
:fas fa-list: [/standup.txt](https://travisflix.com/standup.txt)<br>
:fas fa-list: [/motogp.txt](https://travisflix.com/motogp.txt)<br>
:fas fa-list: [/tech.txt](https://travisflix.com/tech.txt)<br>
:fas fa-list: [/tennis.txt](https://travisflix.com/tennis.txt)<br>
:fas fa-list: [/podcasts.txt](https://travisflix.com/podcasts.txt)<br>
:fas fa-list: [/starcraft.txt](https://travisflix.com/starcraft.txt)

Bash script that generates the files:

```bash
#!/usr/bin/env bash

nginx_www='/usr/local/linuxserver-nginx/config/www'

if [[ -d /usr/local/jellyfin/media && -f /usr/local/jellyfin/media/scriptcheck ]]; then
    # rclone mount exists, no need to remount

    # exit script if variable is empty
    [ -z "$nginx_www" ] && { echo "Error: variable nginx_www is not set or empty"; exit 1; }

    # refresh public text files with media index
    cd /usr/local/jellyfin/media
    find /usr/local/jellyfin/media/video-movies -mindepth 1 -maxdepth 1 -type d -printf '%f\n' | sort > $nginx_www/movies.txt
    find /usr/local/jellyfin/media/video-shows -mindepth 1 -maxdepth 1 -type d -printf '%f\n' | sort > $nginx_www/shows.txt
    tree --noreport -d --charset=en_US.utf8 video-shows >> $nginx_www/shows.txt
    find /usr/local/jellyfin/media/video-standup -mindepth 1 -maxdepth 1 -type d -printf '%f\n' | sort > $nginx_www/standup.txt
    find /usr/local/jellyfin/media/video-tennis -mindepth 1 -maxdepth 1 -type d -printf '%f\n' | sort > $nginx_www/tennis.txt
    find /usr/local/jellyfin/media/video-starcraft -mindepth 1 -maxdepth 1 -type f -printf '%f\n' | sort > $nginx_www/starcraft.txt
    find /usr/local/jellyfin/media/video-tech -mindepth 1 -maxdepth 1 -type d -printf '%f\n' | sort > $nginx_www/tech.txt
    find /usr/local/jellyfin/media/podcasts -mindepth 1 -maxdepth 2 -type d -printf '%f\n' | sort > $nginx_www/podcasts.txt
    tree --noreport --charset=en_US.utf8 podcasts >> $nginx_www/podcasts.txt
    # following two sort reversed
    find /usr/local/jellyfin/media/video-uncategorized -mindepth 1 -maxdepth 2 -type d -printf '%f\n' | sort --reverse > $nginx_www/motogp.txt
    tree --noreport --charset=en_US.utf8 -r video-uncategorized >> $nginx_www/motogp.txt
else
    exit 1

fi
```

If you would like to help keep the site running, go to [How to Support/Donate?](#how-to-supportdonate).


## Where is the status page?

:fas fa-server: [status.travisflix.com](https://status.travisflix.com/)

The 30 day cumulative HTTP uptime for travisflix.com has on average been between **99.90%** - **99.96%**

If you would like to help keep the site running, go to [How to Support/Donate?](#how-to-supportdonate).

> ![info-circle](_media/info-circle_22x22.png) This percentage is referred to as "SLA" (service level agreement). For instance, the [Google Workspace SLA](https://workspace.google.com/intl/en/terms/sla.html) is 99.9% for Search, Gmail, Docs, Drive, Chat, Voice, etc.
> You can view the status for Google here [google.com/appstatus](https://www.google.com/appsstatus).

![TravisFlix Uptime](_media/travisflix-status-metrics-06042021.png)

![TravisFlix Uptime](_media/travisflix-status-06042021.png)

![TravisFlix Uptime](_media/travisflix-status-all-06042021.png)


## Do you have server statistics?

:fas fa-server: [statistics.travisflix.com](https://statistics.travisflix.com/)

If you would like to help keep the site running, go to [How to Support/Donate?](#how-to-supportdonate).

<iframe src="//statistics.travisflix.com/netdata.html" width="100%" height="870px" style="border:0px;overflow-y:visible;" scrolling="yes"></iframe>


## Status of the media sync processes?

:black_joker: The badges below show a real-time representation of the media encryption and syncing processes between the origin and destination Google Drive accounts. These GDrive accounts are what feed the video files to the server which then streams to your eyeballs.

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


## How to Support/Donate?

![₿](_media/BTC-20px.png) Bitcoin (₿) is my preferred unit and medium of currency transfer. However if you do not have or know how to use Bitcoin, I'll provide a couple alternative methods below.

###### REALTIME BITCOIN PRICE: **<iframe src="//btc.travisflix.com" width="100%" height="35px" style="border:0px;overflow-y:hidden;" scrolling="no"></iframe>**

* :fa fa-linode: Linode Platform-as-a-Service (PaaS) operating cost is $10/mo
  * :heavy_dollar_sign: However I don't pay myself
  * :heavy_dollar_sign: The cost to run cloud data storage legitimately would range from $150-200/mo or $1800-2400/yr

> **Option #1**
> [![GreenAddress Permanent Payment URL](_media/greenaddress-donate-green.png)](https://greenaddress.it/pay/GA2GtnSV73LMTzpauEKZsJnRD1yxWf/)<br>
> [32Z8bRQPcip4avcGWujSqLGAmDt52m1Wy4](bitcoin:32Z8bRQPcip4avcGWujSqLGAmDt52m1Wy4)
>
> Here is the related QR code associated with my BTC address beginning with "32Z8bRQ":<br>
> ![BTC](_media/bitcoin-32Z8bRQPcip4avcGWujSqLGAmDt52m1Wy4.png)
---
> **Option #2**
> [bc1q690p3utevcus3mscnq5anegz7a3m7cjv4vvd6g](bitcoin:bc1q690p3utevcus3mscnq5anegz7a3m7cjv4vvd6g)
> 
> Here is the related QR code associated with my BTC address beginning with "bc1q690":<br>
> ![BTC](_media/bc1q690p3utevcus3mscnq5anegz7a3m7cjv4vvd6g.png)


## Media storage info?

:fab fa-google-drive: **20.09 TB** as of 07/2021<br>

![GDrive Storage 01](_media/gdrive-storage-01.png)

![GDrive Storage 02](_media/gdrive-storage-02.png)

Google Cloud Console API Metrics for Google Drive
![Google Cloud API rates](_media/gcloud-03.png)


## Still have questions?

:incoming_envelope: Send me an email at <travisrunyard@gmail.com> or join the Telegram room at [t.me/travisflix](https://t.me/travisflix). My contact information is also available online at [travisyard.run](https://travisyard.run/).

---

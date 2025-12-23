# Gstreamer
Gstreamer (gst-launch-1.0) Encoder ไปยัง SRT Server และ icecaste Server

Server
gst-launch-1.0 srtsrc uri="srt://:9000?mode=listener&latency=200" ! queue ! srtsink uri="srt://:9001?mode=listener&latency=200"


Encoder
ffmpeg -f alsa -i hw:4,0        -acodec aac -profile:a aac_low        -ar 48000 -ac 2 -b:a 256k        -f mpegts "srt://127.0.0.1:9000?mode=caller&latency=200"

Player
ffplay "srt://172.16.3.42:9001?mode=caller&latency=200"



=========================================
Encod to radiostl1.mcot.net

ffmpeg -f alsa -i hw:4,0        -acodec aac -profile:a aac_low        -ar 48000 -ac 2 -b:a 256k        -f mpegts "srt://radiostl1.mcot.net:9000?mode=caller&latency=200"

=============
SRT and Icecast from hw (Rocket Stream)

gst-launch-1.0     alsasrc device=hw:3,0 ! audioresample ! audioconvert     ! tee name=t     t. ! queue ! opusenc bitrate= 256000 ! mux1.     mpegtsmux name=mux1 ! srtsink uri="srt://radiostl1.mcot.net:9004?mode=caller&latency=200"     t. ! queue ! opusenc bitrate=128000 ! oggmux     ! shout2send ip=radiostl1.mcot.net port=8000 password=hackmeA mount=/stlII username=source


SRT and Icecast frome file

gst-launch-1.0 -v \
  filesrc location=media1.opus ! oggdemux ! opusparse ! opusdec ! audioconvert ! audioresample ! tee name=t \
    t. ! queue ! opusenc bitrate=256000 ! mpegtsmux name=mux1 ! srtsink uri="srt://radiostl1.mcot.net:9004?mode=caller&latency=200" \
    t. ! queue ! opusenc bitrate=128000 ! oggmux ! shout2send ip=radiostl1.mcot.net port=8000 username=source password=hackmeA mount=/stlII

=============
For this encoder 
sudo nano /etc/systemd/system/srt-encoder.service

sudo systemctl daemon-reload
sudo systemctl status srt-encoder
sudo systemctl start srt-encoder
sudo systemctl restart srt-encoder









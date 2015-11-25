# ytcast

A command-line client for playing YouTube videos in the built-in
Chromecast YouTube app.

Uses DIAL/UPnP (for discovery), plain old HTTP (for loading videos) and
RAMP (for talking to the app when a video is playing), not CASTv2. I
thought these older protocols had been broken with a firmware update in
2014, but I tried my Chromecast again when I bought a new TV in 2015 and
they worked! So I cleaned this up and put it in its own repository.

# Usage

To start a video:

    ytcast VIDEO_URL

Various things you can do control a playing video:

    ytcast --volume PERCENTAGE
    ytcast --mute
    ytcast --unmute
    ytcast --pause
    ytcast --resume
    ytcast --seek MM:SS

To show the status of a playing video and periodically update until it
stops playing:

    ytcast --wait

(You could use this to drive a playlist, by then loading another video,
sleeping a bit, and using `--wait` again.)

To stop playback and quit the app entirely (it's a package deal), use:

    ytcast --quit

# Discovery

By default it just finds the first Chromecast device on your local
network. If you need to specify a name, use

    ytcast --name DEV_NAME

Or if you want to skip discovery entirely and manually specify the host
and port:

    ytcast --host HOST --port PORT

Use these in conjunction with whatever other arguments you would be
using from the previous section.

# Debugging

You can dump the app information with:

    ytcast --info

To turn on debugging (print all RAMP messages sent and received, and
additional information about connections):

    ytcast --debug

# Limitations

I was able to piece together RAMP commands (it's proprietary) for
everything except loading a video into the currently running instance of
the app. Unfortunately, this means that loading a video always restarts
the app and drops any `--wait` connections you might have open. Please
contact me if you have any ideas.

When the app is starting up, it will sometimes give you a new websocket
session but then immediately close it, so you may have better luck
waiting 10-15 sec rather than relying on the retry mechanism.

If there is no Chromecast accessible on your local network, discovery
will retry forever.

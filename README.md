# Spotify-AdMuter (for LINUX)

Your Party with [Spotify](https://www.spotify.com) - without ads!

[![Spotify-AdMuter](https://github.com/RJVB/Spotify-AdMuter/raw/master/Spotify-AdMuter.png)](https://github.com/RJVB/Spotify-AdMuter)

We all love Spotify, but sometimes people (like us) want to throw a party without having to listen to interrupting ads before having bought [Spotify Premium](https://www.spotify.com/premium/). Well, with this muter project, now you can! Ads are still served, so Spotify are not prevented from fulfulling its legally binding promises to their sponsors. This script just automates what everyone could (and is entitled to, IMHO) do: mute sound while ads play, or listen to something else in the meantime.

**Officially, this is for testing purposes ONLY!** Spotify is a fantastic service and worth every penny. This script is **NOT** meant to circumvent buying premium! Please do consider listening attentively to every ad, or switching to premium to support Spotify - especially if you're going to use it on mobile. But if the script does not work for you, help us improve it!

### Dependencies

Utilities used in the script:

  - xprop
  - pactl
  - notify-send
  - xdotool

You will also need to have one of the following audio/media players installed, in case you want to use anything other than the default "simple" muting method:

  - mpv
  - vlc
  - mplayer
  - mpg321
  - avplay
  - ffplay

Please consult the Settings section below for information on setting a custom audio player.

[![openSUSE](https://news.opensuse.org/wp-content/uploads/2014/11/468x60.png)](http://www.opensuse.org/)

Install all utilities + VLC on **[openSUSE](http://www.opensuse.org/)** with:

    sudo zypper in binutils pulseaudio-utils libnotify-tools xdotool vlc

[![Ubuntu](http://spreadubuntu.neomenlo.org/files/banner-468x60.png)](http://www.ubuntu.com/)

Install all utilities + VLC on **[Ubuntu](http://www.ubuntu.com/)** with:

    sudo apt-get install x11-utils pulseaudio-utils libnotify-bin xdotool vlc

### Installation

**Automated Installation**

Grab the latest release of `Spotify-AdMuter`:

    git clone https://github.com/RJVB/Spotify-AdMuter.git

Run the provided installer:

    cd Spotify-AdMuter
    ./install.sh

**Troubleshooting**

- `Spotify-AdMuter` has been tested to work with Spotify 0.9.x. Support for the 1.0.x beta releases has been implemented, but still needs more testing. If you run into any bugs while using `Spotify-AdMuter` with a Spotify beta release please report them on the bug tracker.

- If you've installed Spotify from any source other than the official repository please make sure that the `spotify` executable is in your `PATH`.

    You can create a symbolic link, if necessary (e.g. linking `my-spotify` to `spotify` if you are using the user installation of [spotify-make](https://github.com/leamas/spotify-make)).

- The installer script will install `Spotify-AdMuter` to `$HOME/bin`, which should be recognized by Ubuntu and openSUSE.

- If `$HOME/bin` didn't exist before, a relog might be necessary to complete the installation.

    Technical explanation: Ubuntu automatically adds `$HOME/bin` to your `PATH` if it exists when you log in. Relogging reloads `$HOME/.profile` and updates your `PATH`.

    If the script doesn't work after relogging you can either [manually add](http://askubuntu.com/q/3744) `$HOME/bin` to your `PATH` or follow the installation instructions below. This also applies if you're using a distro that is configured differently than Ubuntu/OpenSUSE or if you want to install the script in a custom location.

- If GUI and text are too small, you can scale the application by editing the desktop file:

        cd /home/<your username>/.local/share/applications
        sudo gedit Spotify\ \(AdMuter\).desktop

    Add `--force-device-scale-factor=2` to scale it with factor 2:

        Exec=spotify-wrapper.sh --force-device-scale-factor=2 %U

**Manual Installation**

1. Copy `spotify-admuter.sh` and `spotify-wrapper.sh` to your `PATH` (e.g. `$HOME/bin` or `/usr/local/bin` on openSUSE and Ubuntu) and make both of them executable with `chmod +x spotify-admuter.sh` and `chmod +x spotify-wrapper.sh`.

2. Copy `Spotify (AdMuter).desktop` to `$HOME/.local/share/applications` or any other folder your distro reads `.desktop` launchers from (e.g. `/usr/share/applications`, `/usr/local/share/applications`).

The default configuration file will be written automatically on the first startup of the script.

### Usage

If you installed `Spotify-AdMuter` correctly, a new entry called `Spotify (AdMuter)` should appear in your menu. This launcher will start Spotify in ad-free mode. The script will terminate automatically as soon as Spotify exits. As mentioned before, **this is for testing purposes ONLY** so use this new entry only when your purposes are **testing**.

**Important note:** Please make sure you don't have notifications disabled in your Spotify configuration (`ui.track_notifications_enabled=true` in `~/.config/spotify/User/<your username>/prefs`).

### Settings

The configuration file for `Spotify-AdMuter` is located under `$HOME/.config/Spotify-AdMuter/Spotify-AdMuter.cfg`. There are several settings that control how `Spotify-AdMuter` operates:

**Modes**

`CUSTOM_MODE` controls the ad blocking mode. The following modes are available:

- `simple`: mute Spotify, unmute when ad is over
- `interstitial`: mute Spotify, play random local track, stop and unmute when ad is over
    + If the local track is shorter than the ad, `Spotify-AdMuter` will automatically try to loop it. This will only work with players that support a loop option. If you are planning to use this feature with a custom player make sure to also supply a custom loop option in your configuration file.
- `continuous`: mute Spotify, play random local track, stop and unmute when track is over
    + You can skip the local track as soon as the ad is over. To do so, simply press Play or Forward/Next in your Spotify client (or use the corresponding hotkeys).
    + Please note that the `continuous` ad blocking mode works best with tracks that are longer than the average ad duration (≈30-45s). If a custom track ends prematurely or is shorter than the current ad, `Spotify-AdMuter` will switch to the next random local track in line.

The default ad blocking mode is `continuous`.

`Spotify-AdMuter` will always fall back to `simple` mode if no local tracks are found and/or if no supported music player is available on the system.

**Local Playback**

The following settings control local music playback during ads:

- `CUSTOM_PLAYER`: local music player to use; chosen automatically by default
- `CUSTOM_LOOPOPT`: loop option for custom player (e.g. `-loop 0`); we recommend setting this if you are planning to use interstitial adblocking mode
- `CUSTOM_VOLUME`: volume of local playback; set to 100 by default
- `CUSTOM_MUSIC`: local track to play / local music directory to choose tracks from; set to XDG standard music directory by default (e.g. `$HOME/Music`)

**Debug Setting**

You can make the CLI output more verbose and enable the log file by setting `DEBUG` to `1`. The log will be written to `$HOME/.Spotify-AdMuter.log` and replaced each time `Spotify-AdMuter` runs.

### Important Notes

Please make sure to always use the provided launcher when running Spotify. Under some circumstances Spotify might remain muted when exiting the application. This is a technical limitation with `PulseAudio`. `Spotify-AdMuter` automatically unmutes Spotify when initially run.

If, for some reason, Spotify does remain muted you can use the following command to unmute it manually while it's running:

```bash
for i in $(LC_ALL=C pactl list | grep -E '(^Sink Input)|(media.name = \"Spotify\"$)' | cut -d \# -f2 | grep -v Spotify); do pactl set-sink-input-mute "$i" no; done
```

### Similar Projects

- [blockify](https://github.com/mikar/blockify) - automatic/blacklist-based ad-blocker written in Python
- [spotify_ad_blocker_linux.rb](https://github.com/superr4y/hacks/blob/master/spotify/spotify_ad_blocker_linux.rb) - automatic ad-blocker written in Ruby
- [spotify-blacklist-mute](https://github.com/ysangkok/spotify-blacklist-mute) - blacklist-based muting, written in Bash

### License

Many people have contributed to make our script become what it is today (huge shout-out to the initial creator [pcworld](https://github.com/pcworld)). If you are like us and think that it is very sad when projects die, please accept that all code here is fully licensed under GPL v3+. Have a look at the full [License](https://github.com/RJVB/Spotify-AdMuter/blob/master/LICENSE). Contribute pull requests!

**This product is not endorsed, certified or otherwise approved in any way by Spotify. Spotify is the registered trade mark of the Spotify Group. Use your brainz prior to formatting your HDD.**

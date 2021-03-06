# Audio KB & BKM

## Principle

![Sound Arch Simple Abstraction](http://www.alsa-project.org/main/images/2/22/Snd-driver-context.png)

> http://www.alsa-project.org/main/index.php/Minivosc#Understanding_ALSA_driver_architecture

## Codec

## Encoder & Decoder

### MAD: MPEG Audio Decoder

MAD is a high-quality MPEG audio decoder. It currently supports MPEG-1 and the MPEG-2 extension to lower sampling frequencies, as well as the de facto MPEG 2.5 format. All three audio layers — Layer I, Layer II, and Layer III (i.e. MP3) — are fully implemented.

MAD does not yet support MPEG-2 multichannel audio (although it should be backward compatible with such streams) nor does it currently support AAC.

MAD has the following special features:
- 24-bit PCM output
- 100% fixed-point (integer) computation
- completely new implementation based on the ISO/IEC standards
- available under the terms of the GNU General Public License (GPL)

> http://www.underbit.com/products/mad/

## Architecture

### Linux Sound Subsystem

![Linux Sound Subsystem](images/linux_sound_subsystem.jpg)

> http://www.embeddedlinux.org.cn/essentiallinuxdevicedrivers/final/images/ZTVyMWQ5OHB0Z2lzMC8vbWMzNWE5NC9yMzZnMjdhZ3AuaWNzaTAxLzNmM2hpZ2Y-.jpg

- The sound core, which is a code base consisting of routines and structures available to other components of the Linux-Sound layer.

  Like the core layers belonging to other driver subsystems, the sound core provides a level of indirection that renders each component in the sound subsystem independent of the others.

  The core also plays an important role in exporting the ALSA API to higher applications.

  The `/dev/snd/*` device nodes are created and managed by the ALSA core:
  - `/dev/snd/controlC0` is a control node (that applications use for controlling volume gain and such),
  - `/dev/snd/pcmC0D0p` is a playback device (p at the end of the device name stands for playback), and
  - `/dev/snd/pcmC0D0c` is a recording device (c at the end of the device name stands for capture).

  In these device names, the integer following C is the card number, and that after D is the device number.

  An ALSA driver for a card that has a voice codec for telephony and a stereo codec for music might export `/dev/snd/pcmC0D0p` to read audio streams destined for the former and `/dev/snd/pcmC0D1p` to channel music bound for the latter.

- Audio controller drivers specific to controller hardware.

  To drive the audio controller present in the Intel ICH South Bridge chipsets, for example, use the `snd_intel8x0` driver.

- Audio codec interfaces that assist communication between controllers and codecs.

  For AC'97 codecs, use the `snd_ac97_codec` and `ac97_bus` modules.

- An OSS emulation layer that acts as a conduit between OSS-aware applications and the ALSA-enabled kernel.

  This layer exports `/dev` nodes that mirror what the OSS layer offered in the 2.4 kernels.

  These nodes, such as `/dev/dsp`, `/dev/adsp`, and `/dev/mixer`, allow OSS applications to run unchanged over ALSA.

  The OSS `/dev/dsp` node maps to the ALSA nodes `/dev/snd/pcmC0D0*`, `/dev/adsp` corresponds to `/dev/snd/pcmC0D1*`, and `/dev/mixer` associates with `/dev/snd/controlC0`.

- Procfs and sysfs interface implementations for accessing information via `/proc/asound/` and `/sys/class/sound/`.

- The user-space ALSA library, `alsa-lib`, which provides the `libasound.so` object.

  This library eases the job of the ALSA application programmer by offering several canned routines to access ALSA drivers.

- The `alsa-utils` package that includes utilities such as `alsamixer`, `amixer`, `alsactl`, and `aplay`.

  Use `alsamixer` or `amixer` to change volume levels of audio signals such as line-in, line-out, or microphone, and `alsactl` to control settings for ALSA drivers.

  To play audio over ALSA, use `aplay`.



> http://www.embeddedlinux.org.cn/essentiallinuxdevicedrivers/final/ch13lev1sec2.html

### Linux Audio Architecture

- ALSA
- OSS
- JACK
- PulseAudio

![Audio Layer in Linux](images/audio-layers.jpg)

> http://tuxradar.com/content/how-it-works-linux-audio-explained

![Linux Audio Architecutre](http://thewelltemperedcomputer.com/Linux/Pic/DiagramAlsa.jpg)

> http://thewelltemperedcomputer.com/Linux/AudioArchitecture.htm

![Linux Sound Components](https://jan.newmarch.name/LinuxSound/Sampled/Architecture/layers.png)

> https://jan.newmarch.name/LinuxSound/Sampled/Architecture/

![Linux Audio Stack](http://linux-audio.com/images/linux-audio-stack.png)

![Linux Auido Components and Relationship](http://1.bp.blogspot.com/-jLXOZF_0txQ/T6K3HZ_3JII/AAAAAAAABrw/y-4zbxrWw-E/s1600/linux_audio.png)

> http://free-electrons.com/doc/embedded_linux_audio.pdf

### Android Audio Subsystem

![Android Audio Subsystem](images/Android_Audio_Stack.jpg)

> http://processors.wiki.ti.com/index.php/TI-Android-GingerBread-2.3.4-DevKit-2.1_PortingGuides#Audio

### ALSA - Advanced Linux Sound Architecture

Advanced Linux Sound Architecture (ALSA) is a software framework and part of the Linux kernel that provides an application programming interface (API) for sound card device drivers. Some of the goals of the ALSA project at its inception were automatic configuration of sound-card hardware and graceful handling of multiple sound devices in a system. ALSA is released under the GNU General Public License (GPL) and the GNU Lesser General Public License (LGPL).

![ALSA in Kernel](https://upload.wikimedia.org/wikipedia/commons/9/91/Linux_kernel_and_gaming_input-output_latency.svg)

>http://www.alsa-project.org/main/index.php/Main_Page
>
> https://en.wikipedia.org/wiki/Advanced_Linux_Sound_Architecture
>
> http://www.alsa-project.org/~frank/alsa-sequencer/

#### ALSA Structure

![ALSA Structure and Flow](images/alsa-structure-flow.gif)

> http://lac.linuxaudio.org/2003/zkm/slides/takashi_iwai-alsa_ruminations/lad.html

> http://www.alsa-project.org/~tiwai/lk2k/lk2k.html

#### ALSA Interface

**`/dev/snd/`**

  ```
  $ ls -l /dev/snd
  total 0
  drwxr-xr-x  2 root root       60  7月 20 11:11 by-path
  crw-rw----+ 1 root audio 116,  2  7月 20 11:11 controlC0
  crw-rw----+ 1 root audio 116,  4  7月 21 18:05 pcmC0D0c
  crw-rw----+ 1 root audio 116,  3  7月 21 18:11 pcmC0D0p
  crw-rw----+ 1 root audio 116,  5  7月 20 11:11 pcmC0D1c
  crw-rw----+ 1 root audio 116,  1  7月 20 11:11 seq
  crw-rw----+ 1 root audio 116, 33  7月 20 11:11 timer
  ```

**`/proc/asound/`**

  ```
  $ ls -l /proc/asound
  total 0
  dr-xr-xr-x 6 root root 0  7月 23 21:56 card0
  -r--r--r-- 1 root root 0  7月 23 21:56 cards
  -r--r--r-- 1 root root 0  7月 23 21:56 devices
  lrwxrwxrwx 1 root root 5  7月 23 21:56 I82801AAICH -> card0
  -r--r--r-- 1 root root 0  7月 23 21:56 modules
  dr-xr-xr-x 2 root root 0  7月 23 21:56 oss
  -r--r--r-- 1 root root 0  7月 23 21:56 pcm
  dr-xr-xr-x 2 root root 0  7月 23 21:56 seq
  -r--r--r-- 1 root root 0  7月 23 21:56 timers
  -r--r--r-- 1 root root 0  7月 23 21:56 version
  ```

- `/proc/asound/cards`
  List of available sound cards
- `/proc/asound/devices`
  List of card devices
- `/proc/asound/card<i>/id`
  Card identifier
- `/proc/asound/card<i>/pcm[c|p]<j>/info`
  Information about a capture (c) or playback (p) PCM device
- `/proc/asound/modules`
- `/proc/asound/version`
  ALSA version

![Create ALSA device files by hand](images/alsa_proc_dev.png)

> http://free-electrons.com/doc/embedded_linux_audio.pdf

#### ALSA Config

The `alsa-lib` package provides the `/usr/share/alsa/alsa.conf` file as the main entry point.
That file is responsible for including the full list of potential .asoundrc-format-type files on the system.

The asoundrc file is typically installed in a user's home directory
  ```
  $HOME/.asoundrc
  ```
and is called from
  ```
  /usr/share/alsa/alsa.conf
  ```
It is also possible to install a system wide configuration file as
  ```
  /etc/asound.conf
  ```

More details refer to [Global view of ALSA config file framework, executive summary](http://www.alsa-project.org/main/index.php/Asoundrc#Global_view_of_ALSA_config_file_framework.2C_executive_summary)

> http://www.alsa-project.org/main/index.php/Asoundrc

> http://www.volkerschatz.com/noise/alsa.html

#### ALSA Concepts

**Hardware - Parameters - Devices and Plugins**

**Sound cards and hardware devices**

ALSA arranges hardware audio devices and their components into a hierarchy of cards, devices and subdevices. It reflects the structure and capabilities of your hardware as seen by ALSA. If there is a discrepancy between a sound card's device structure and its documentation, this may be due to the driver not supporting all its features.

ALSA **cards** correspond one-to-one to hardware sound cards. They do not play a big part except that one can list the devices on each card. A card can be denoted by its ID (a string) or a numerical index starting at zero.

Most of ALSA's hardware access happens at the **device** level. The devices of each card are enumerated starting from zero. Different devices can be opened and used independently of each other. Typically, specifying a sound card and device will be sufficient to determine on which connector or set of connectors your audio signal will come out, or from which it is read.

**Subdevices** are the most fine-grained objects ALSA can distinguish. The most frequently encountered cases are that a device has a separate subdevice for each channel or that there is only one subdevice altogether. A device's subdevices can in principle be used independently, but playing a multi-channel signal to a subdevice will use (and block) the following subdevice(s) too. Like devices, subdevices are identified by a zero-based index.

**PCM parameters and the configuration space**

Digitised sound has a number of parameters such as the sampling rate, the number of channels and the format in which sample values are stored. If you have been programming OSS, you may be used to setting these parameters one after the other before playing a sound file. So what is that talk about a "configuration space" which one finds in the ALSA documentation?

The answer is that in reality things are not so simple: Some sound cards cannot combine all sample formats with all sampling rates or channel counts, for example. So the parameters are not independent. ALSA accounts for this fact by arranging sets of parameters in an n-dimensional space, the configuration space. One of these dimensions corresponds to the sampling rate, one to the sample format, and so on. If the parameters of one specific sound card are all independent, all legal configurations lie in one big n-dimensional box. In this case, one could describe them by giving separate ranges for all parameters. If the parameters are not independent, the set of allowed configurations is more complicated, and could not be expressed so simply.

When a hardware device is accessed with ALSA, parameters are not fixed independently of each other. Rather, the legal configuration space for a device is narrowed down successively by restricting specific parameters. This makes it possible, for instance, to set a minimal rather than exact sampling rate. It also potentially leads to the problem that it depends on the order in which the parameters are set which parameter set you end up with (see below). That said, an ALSA plugin is available which automatically chooses the most sensible hardware parameters and performs format conversion as needed. This and other plugins are described in the following section.

**ALSA devices and plugins**

To avoid confusion later on, a few brief notes on ALSA devices are in order. These are quite different from the hardware devices introduced above. ALSA devices are denoted by strings. They are defined in one of ALSA's configuration files (see below) and are basically wrappers for plugins.

It is no great exaggeration to say that ALSA consists almost entirely of plugins. Whenever a player or other program uses an ALSA device, plugins do the dirty work.

PCM plugins extends functionality and features of PCM devices. The plugins take care about various sample conversions, sample copying among channels and so on.

> http://www.volkerschatz.com/noise/alsa.html

> http://www.alsa-project.org/alsa-doc/alsa-lib/pcm_plugins.html

#### ALSA Utilities

The ALSA utils are a collection of small and often extremely powerful applications designed to allow users to control the various parts of the ALSA system.

- The `alsactl` application is a way to save settings for your device.
- The `alsamixer` application is an ncurses version of `amixer`.
- The `amixer` application is a command line app which allows adjustments to be made to a devices volume and sound controls.
- The `acconnect` and `aseqview` applications are for making MIDI connections and viewing the list of connected ports.
- The `aplay` and `arecord` applications are for commandline playback and recording of a number of file types including raw, wave and aiff at all the sample rates, bitdepths and channel counts known to the ALSA library.

> http://www.alsa-project.org/main/index.php/ALSA_User_Info

#### ALSA SoC Layer

The overall project goal of the ALSA System on Chip (ASoC) layer is to provide better ALSA support for embedded system on chip procesors (e.g. pxa2xx, au1x00, iMX, etc) and portable audio codecs. Currently there is some support in the kernel for SoC audio, however it has some limitations.

The ASoC layer is designed to address these issues and provide the following features:
- Codec independence. Allows reuse of codec drivers on other platforms and machines.
- Easy I2S/PCM audio interface setup between codec and SoC. Each SoC interface and codec registers it's audio interface capabilities with the core and are subsequently matched and configured when the application hw params are known.
- Dynamic Audio Power Management (DAPM). DAPM automatically sets the codec to it's minimum power state at all times. This includes powering up/down internal power blocks depending on the internal codec audio routing and any active streams.
- Pop and click reduction. Pops and clicks can be reduced by powering the codec up/down in the correct sequence (including using digital mute). ASoC signals the codec when to change power states.
- Machine specific controls: Allow machines to add controls to the sound card. e.g. volume control for speaker amp.

To achieve all this, ASoC basically splits an embedded audio system into 3 components:
- Codec driver: The codec driver is platform independent and contains audio controls, audio interface capabilities, codec dapm definition and codec IO functions.
- Platform driver: The platform driver contains the audio dma engine and audio interface drivers (e.g. I2S, AC97, PCM) for that platform.
- Machine driver: The machine driver handles any machine specific controls and audio events. i.e. turning on an amp at start of playback.

> http://www.rpsys.net/openzaurus/patches/alsa/info.html
>
> http://www.alsa-project.org/main/index.php/ASoC

![ASoC Sample AM/DM37x, OMAP35x](images/Asoc_architecture.png)

> https://processors.wiki.ti.com/index.php?oldid=218088

### PulseAudio

![Audio Layer in Linux](images/Pulseaudio-diagram-1000px.svg.png)

> https://ubuntuforums.org/showthread.php?t=1794581

<!--
![PulseAudio and ALSA](http://i.stack.imgur.com/IJkHa.png)
-->

> http://askubuntu.com/questions/581128/what-is-the-relation-between-alsa-and-pulseaudio-sound-architecture

PulseAudio runs a sound server, a background process accepting sound input from one or more sources (processes or capture devices) and redirecting it to one or more sinks (sound cards, remote network PulseAudio servers, or other processes).

One of the goals of PulseAudio is to reroute all sound streams through it, including those from processes that attempt to directly access the hardware (like legacy OSS applications). PulseAudio achieves this by providing adapters to applications using other audio systems, like aRts and ESD.

The main PulseAudio features include:

- Per-application volume controls.
- An extensible plugin architecture with support for loadable modules.
- Compatibility with many popular audio applications.
- Support for multiple audio sources and sinks.
- Low latency operation and latency measurement.
- A zero-copy memory architecture for processor resource efficiency.
- Ability to discover other computers using PulseAudio on the local network and play sound through their speakers directly.
- Ability to change which output device an application plays sound through while the application is playing sound (without the application needing to support this, and indeed without even being aware that this happened).
- A command-line interface with scripting capabilities.
- A sound daemon with command line reconfiguration capabilities.
- Built-in sample conversion and resampling capabilities.
- The ability to combine multiple sound cards into one.
- The ability to synchronize multiple playback streams.
- Bluetooth audio devices with dynamic detection.
- The ability to enable system wide equalization.

> https://en.wikipedia.org/wiki/PulseAudio

## Software

![Flow of Audio in Linux](images/flow_linux_audio.jpeg)

> https://medium.com/@YihuiXiong/record-play-audio-on-linux-8f59adf93710

### `arecord`

Alias to `aplay -C`.

`arecord` is a command-line soundfile recorder for the ALSA soundcard driver. 
It supports several file formats and multiple soundcards with multiple devices. 
If recording with interleaved mode samples the file is automatically split before the 2GB filesize.

> http://linux.die.net/man/1/aplay

### `aplay`

`aplay` - command-line sound recorder and player for ALSA soundcard driver.

```
Usage: aplay [OPTION]... [FILE]...

-h, --help              help
    --version           print current version
-l, --list-devices      list all soundcards and digital audio devices
-L, --list-pcms         list device names
-D, --device=NAME       select PCM by name
-q, --quiet             quiet mode
-t, --file-type TYPE    file type (voc, wav, raw or au)
-c, --channels=#        channels
-f, --format=FORMAT     sample format (case insensitive)
-r, --rate=#            sample rate
-d, --duration=#        interrupt after # seconds
-M, --mmap              mmap stream
-N, --nonblock          nonblocking mode
-F, --period-time=#     distance between interrupts is # microseconds
-B, --buffer-time=#     buffer duration is # microseconds
    --period-size=#     distance between interrupts is # frames
    --buffer-size=#     buffer duration is # frames
-A, --avail-min=#       min available space for wakeup is # microseconds
-R, --start-delay=#     delay for automatic PCM start is # microseconds 
                        (relative to buffer size if <= 0)
-T, --stop-delay=#      delay for automatic PCM stop is # microseconds from xrun
-v, --verbose           show PCM structure and setup (accumulative)
-V, --vumeter=TYPE      enable VU meter (TYPE: mono or stereo)
-I, --separate-channels one file for each channel
-i, --interactive       allow interactive operation from stdin
-m, --chmap=ch1,ch2,..  Give the channel map to override or follow
    --disable-resample  disable automatic rate resample
    --disable-channels  disable automatic channel conversions
    --disable-format    disable automatic format conversions
    --disable-softvol   disable software volume control (softvol)
    --test-position     test ring buffer position
    --test-coef=#       test coefficient for ring buffer position (default 8)
                        expression for validation is: coef * (buffer_size / 2)
    --test-nowait       do not wait for ring buffer - eats whole CPU
    --max-file-time=#   start another output file when the old file has recorded
                        for this many seconds
    --process-id-file   write the process ID here
    --use-strftime      apply the strftime facility to the output file name
    --dump-hw-params    dump hw_params of the device
    --fatal-errors      treat all errors as fatal
Recognized sample formats are: S8 U8 S16_LE S16_BE U16_LE U16_BE S24_LE S24_BE U24_LE U24_BE S32_LE S32_BE U32_LE U32_BE FLOAT_LE FLOAT_BE FLOAT64_LE FLOAT64_BE IEC958_SUBFRAME_LE IEC958_SUBFRAME_BE MU_LAW A_LAW IMA_ADPCM MPEG GSM SPECIAL S24_3LE S24_3BE U24_3LE U24_3BE S20_3LE S20_3BE U20_3LE U20_3BE S18_3LE S18_3BE U18_3LE U18_3BE G723_24 G723_24_1B G723_40 G723_40_1B DSD_U8 DSD_U16_LE
Some of these may not be available on selected hardware
The available format shortcuts are:
-f cd (16 bit little endian, 44100, stereo)
-f cdr (16 bit big endian, 44100, stereo)
-f dat (16 bit little endian, 48000, stereo)
```

Exmaples:

```
aplay -c 1 -t raw -r 22050 -f mu_law foobar
```
will play the raw file "foobar" as a 22050-Hz, mono, 8-bit, Mu-Law .au file.

```
arecord -d 10 -f cd -t wav -D copy foobar.wav
```
will record foobar.wav as a 10-second, CD-quality wave file, using the PCM "copy" (which might be defined in the user's .asoundrc file as:

```
pcm.copy {
  type plug
  slave {
    pcm hw
  }
  route_policy copy
}
```

```
arecord -t wav --max-file-time 30 mon.wav
```
record from the default audio source in monaural, 8,000 samples per second, 8 bits per sample. Start a new file every 30 seconds. File names are mon-nn.wav, where nn increases from 01. The file after mon-99.wav is mon-100.wav.

```
arecord -f cd -t wav --max-file-time 3600 --use-strftime %Y/%m/%d/listen-%H-%M-%v.wav
```
Record in stereo from the default audio source. Create a new file every hour. The files are placed in directories based on their start dates and have names which include their start times and file numbers.

![aplay in linux](http://www.embeddedlinux.org.cn/essentiallinuxdevicedrivers/final/images/MjltNHJhaS9kNy8zY3JncDA4dHMvOTMxZTZhZzU1LjhpMWYvcGdoaWZjaXMwM2c-.jpg)

### `alsamixer`

`alsamixer` - soundcard mixer for ALSA soundcard driver, with ncurses interface

> http://linux.die.net/man/1/alsamixer

### `amixer`

`amixer` - command-line mixer for ALSA soundcard driver

`amixer` allows command-line control of the mixer for the ALSA soundcard driver. amixer supports multiple soundcards.

`amixer` with no arguments will display the current mixer settings for the default soundcard and device. This is a good way to see a list of the simple mixer controls you can use.

> http://linux.die.net/man/1/amixer

### `sox`

SoX - Sound eXchange, the Swiss Army knife of audio manipulation

```
sox [global-options] [format-options] infile1
    [[format-options] infile2] ... [format-options] outfile
    [effect [effect-options]] ...

play [global-options] [format-options] infile1
    [[format-options] infile2] ... [format-options]
    [effect [effect-options]] ...

rec [global-options] [format-options] outfile
    [effect [effect-options]] ...
```

SoX reads and writes audio files in most popular formats and can optionally apply effects to them; 
it can combine multiple input sources, synthesise audio, and, on many systems, act as a general purpose 
audio player or a multi-track audio recorder. It also has limited ability to split the input in to multiple output files.

Almost all SoX functionality is available using just the `sox` command, however, to simplify playing and 
recording audio, if SoX is invoked as `play` the output file is automatically set to be the default sound 
device and if invoked as `rec` the default sound device is used as an input source. Additionally, the `soxi`(1) 
command provides a convenient way to just query audio file header information.

Examples:

```
sox recital.au recital.wav
```
translates an audio file in Sun AU format to a Microsoft WAV file, whilst
```
sox recital.au -r 12k -b 8 -c 1 recital.wav vol 0.7 dither
```
performs the same format translation, but also changes the audio sampling rate & sample size, down-mixes to mono, and applies the vol and dither effects.
```
sox -r 8k -u -b 8 -c 1 voice-memo.raw voice-memo.wav
```
converts 'raw' (a.k.a. 'headerless') audio to a self-descibing file format,
```
sox slow.aiff fixed.aiff speed 1.027
```
adjusts audio speed,
```
sox short.au long.au longer.au
```
concatenates two audio files, and
```
sox -m music.mp3 voice.wav mixed.flac
```
mixes together two audio files.
```
play "The Moonbeams/Greatest/*.ogg" bass +3
```
plays a collection of audio files whilst applying a bass boosting effect,
```
play -n -c1 synth sin %-12 sin %-9 sin %-5 sin %-2 fade q 0.1 1 0.1
```
plays a synthesised 'A minor seventh' chord with a pipe-organ sound,
```
rec -c 2 test.aiff trim 0 10
```
records 10 seconds of stereo audio, and
```
rec -M take1.aiff take1-dub.aiff
```
records a new track in a multi-track recording.
```
rec -r 44100 -2 -s -p silence 1 0.50 0.1% 1 10:00 0.1% | \
     sox -p song.ogg silence 1 0.50 0.1% 1 2.0 0.1% : \
     newfile : restart
```
records a stream of audio such as LP/cassette and splits in to multiple audio files at points with 2 seconds of silence. Also does not start recording until it detects audio is playing and stops after it sees 10 minutes of silence.

> http://linux.die.net/man/1/sox

### `mpg123`

`mpg123` - play audio MPEG 1.0/2.0/2.5 stream (layers 1, 2 and 3)

```
mpg123 [ options ] file ... | URL ... | -
```

`mpg123` reads one or more files (or standard input if ''-'' is specified) or URLs and plays them on the audio device (default) or outputs them to stdout. file/URL is assumed to be an MPEG audio bit stream.

> http://linux.die.net/man/1/mpg123

### `madplay`

`madplay` - decode and play MPEG audio stream(s)

```
madplay [options] file ...
madplay [options] -o [type:]path file ...
```

`madplay` is a command-line MPEG audio decoder and player based on the MAD library (libmad).

> http://manpages.ubuntu.com/manpages/precise/man1/madplay.1.html

### `fluidsynth`

![fluidsynth](http://tedfelix.com/linux/linux-midi.png)

> http://tedfelix.com/linux/linux-midi.html

## Awesome

### Noise Generator

**Using SoX (Sound eXchange)**

On Mac OS:
```
$ brew install sox
$ cat /dev/urandom | sox -traw -r44100 -b16 -e unsigned-integer - -tcoreaudio
```

On Linux:
```
sudo apt-get install sox
sox -traw -r44100 -b16 -e unsigned-integer /dev/urandom -d
```

Explanation:

First, we take the contents of the random generator by doing `cat /dev/urandom`, after which we pipe them to `sox`.

Sox takes a couple of options to tell it what kind of data it’s receiving:
* `-traw` Tells sox to consider incoming data as raw, uncompressed audio.
* `-r44100` Sets the input sampling rate to 44.1 Kiloherz.
* `-b16` Tells sox the bit-depth for this audio stream is 16 bits.
* `-e` unsigned-integer Says that the raw data should be considere an unsigned integer. (Which are the kind of numbers /dev/urandom outputs.)
* `-` This stray minus sign is very **IMPORTANT**! It tells sox to take the stream that was piped to it as a source.
* `-tcoreaudio` tells sox to use the ‘coreaudio’ device for output.

```
# rain-like sound
play -t sl -r48000 -c2 - synth -1 pinknoise tremolo .1 40 < /dev/zero
```

> https://blogs.fsfe.org/marklindhout/2013/02/need-to-play-high-fidelity-white-noise-listen-to-devurandom-on-osx/



**Noise in Different Colors**

  ```
  $ play -n synth 60:00 whitenoise
  ```

> http://dirk.net/2009/07/14/white-noise-generator-for-linux/


**Imitates the Soft Murmur of the Sea**

  ```
  play -n synth brownnoise synth pinknoise mix synth sine amod 0.3 10
  ```

Explanation:

This command first generates and mixes brown noise and pink noise, which I find to be the most comfortable and natural noise.
Then it generates a sine wave of 0.3 Hz with an offset of 10% and uses this to modulate the amplitude of our mixed noises to
produce the sound of ocean waves.

Modifications:
- Timer:
  You can add a timer and limit the playback duration by specifying the number of seconds, the number of minutes and
  seconds (mm:ss) or the number of hours, minutes and seconds (hh:mm:ss) right before brownnoise. Here's an example for one hour:

  ```
  play -n synth 1:0:0 brownnoise synth pinknoise mix synth sine amod 0.3 10
  ```

- Wave frequency:
  If you want the waves to hit the beach more or less frequent, simply change the frequency of the sine wave used for amplitude
  modification (0.3 in the above command). The number represents the amount of waves per second, so a frequency of 0.1 Hz will
  cause 0.1 waves per second and therefore make one wave last for 10 seconds:

  ```
  play -n synth brownnoise synth pinknoise mix synth sine amod 0.1 10
  ```

- Minimum background noise volume:
  The sine that is used for amplitude modulation got shifted to an offset of 10%, so the brown-pink noise will always be played
  with at least 10% volume. If you want a stronger or weaker background noise, increase or decrease this offset to your needs.
  Here's an example with 20% background noise:

  ```
  play -n synth brownnoise synth pinknoise mix synth sine amod 0.3 20
  ```

**Using `aplay`**

  ```
  aplay --channels=2 --format=S16_LE --rate=44100 --duration=3600 /dev/urandom
  ```

**Using `ffmpeg`**

FFMpeg has an audio noise source filter. You can play it using ffplay:
  ```
  ffplay -f lavfi -showmode 0 -i 'anoisesrc=color=brown'
  ```
The arg to -i is interpreted as a lavfi filter graph, because of -f lavfi. -showmode 0 disables ffplay's default audio
visualizer window, which it shows by default for audio-only inputs.

As you can see from the output of ffmpeg -h filter=anoisesrc, you get a choice of brown/pink/white noise at whatever
amplitude and samplerate you like, optionally with a finite duration.

> http://askubuntu.com/questions/789465/generate-white-noise-to-calm-a-baby

**Star Trek Background Noise**

  ```
  play -n -c1 synth whitenoise lowpass -1 120 lowpass -1 120 lowpass -1 120 gain +14
  ```

  ```
  #!/bin/sh
  # Engage Warp Drive

  # Requires package 'sox'
  # http://www.reddit.com/r/linux/comments/n8a2k/commandline_star_trek_engine_noise_comment_from/

  # odokemono
  # http://www.reddit.com/r/scifi/comments/n7q5x/want_to_pretend_you_are_aboard_the_enterprise_for/c36xkjx
  # original
  # play -n -c1 synth whitenoise band -n 100 20 band -n 50 20 gain +25  fade h 1 864000 1

  # noname-_-
  # http://www.reddit.com/r/scifi/comments/n7q5x/want_to_pretend_you_are_aboard_the_enterprise_for/c373gpa
  # stereo
  # play -c2 -n synth whitenoise band -n 100 24 band -n 300 100 gain +20

  # braclayrab
  # http://www.reddit.com/r/scifi/comments/n7q5x/want_to_pretend_you_are_aboard_the_enterprise_for/c372pyy
  # TNG
  # play -n -c1 synth whitenoise band 100 20 compand .3,.8 -1,-10 gain +20

  play -n -c1 synth whitenoise lowpass -1 120 lowpass -1 120 lowpass -1 120 gain +14
  ```

> http://askubuntu.com/questions/789465/generate-white-noise-to-calm-a-baby

### Tips

To list the available sound cards and PCMs for playback:

```
aplay -l
```

To list the available sound cards and PCMs for capture:

```
arecord -l
```

In most cases `-Dplughw:0,0` is the device we want to use for audio but in case we have several audio devices (onboard + USB for example) one need to specify which device to use for audio: `-Dplughw:omap5uevm,0` will use the onboard audio on OMAP5-uEVM board.

To play audio on card0's PCM0 and let ALSA to decide if resampling is needed:

```
aplay -Dplughw:0,0 <path to wav file>
```

To record audio to a file:

```
arecord -Dplughw:0,0 -t wav <path to wav file>
```

To test full duplex audio (play back the recorded audio w/o intermediate file):

```
arecord -Dplughw:0,0 | aplay -Dplughw:0,0
```

To request specific format to be used for playback/capture take a look at the help of aplay/arecord and specify the format with `-f -r -c` and open the hw device not the plughw `-Dhw:0,0`

For example, record 48KHz, stereo 16bit audio:

```
arecord -Dhw:0,0 -fdat -t wav record_48K_stereo_16bit.wav
```

Or to record record 96KHz, stereo 24bit audio:

```
arecord -Dhw:0,0 -fS24_LE -c2 -r96000 -t wav record_96K_stereo_24bit.wav
```

It is a good practice to save the mixer settings found to be good and reload them after every boot (if your distribution is not doing this already)

Set the mixers for the board with amixer, alsamixer
```
alsactl -f board.aconf store
```

After booting up the board it can be restored with a single command:

```
alsactl -f board.aconf restore
```

> http://processors.wiki.ti.com/index.php/Linux_Core_Audio_User's_Guide

## Hardware

### Interface

**Analogue**

![Audio Interface](http://blog.dubspot.com/files/2011/06/komplete_audio_6_web_setups-02.jpg)

> https://en.wikipedia.org/wiki/Audio_and_video_interfaces_and_connectors

> http://blog.dubspot.com/audio-interface-breakdown-apogee-duet-2-native-instruments-komplete-audio-6/

> http://blog.dubspot.com/understanding-audio-interfaces/

**Digital**

- AC'97 (Audio Codec 1997) interface between integrated circuits on PC motherboards
- Intel High Definition Audio - modern replacement for AC'97
- S/PDIF - either over coaxial cable or TOSLINK, common in consumer audio equipment and derived from AES3
- I2S (Inter-IC sound) interface between integrated circuits in consumer electronics
- MIDI - low-bandwidth interconnect for carrying instrument data; cannot carry sound but can carry digital sample data in non-realtime
- MADI (Multichannel Audio Digital Interface)
- ADAT interface
- AES3 interface with XLR connectors, common in professional audio equipment
- AES47 - professional AES3-style digital audio over Asynchronous Transfer Mode networks
- TDIF, TASCAM proprietary format with D-sub cable
- A2DP via Bluetooth

> https://en.wikipedia.org/wiki/Digital_audio#Digital_audio_interfaces

### I2S

**I2S**, also known as **Inter-IC Sound**, **Integrated Interchip Sound**, or **IIS**, is an electrical serial
bus interface standard used for connecting digital audio devices together. It is used to communicate PCM audio
data between integrated circuits in an electronic device.

The bus consists of at least three lines:

- Bit clock line (SCK)
- Word clock line - also called word select (WS) or left right clock (LRCLK)
- At least one multiplexed data line (SD)

It may also include the following lines:

- Master clock (typical 256 x LRCLK)
- A multiplexed data line for upload

> https://web.archive.org/web/20060702004954/http://www.semiconductors.philips.com/acrobat_download/various/I2SBUS.pdf

![I2S Connection](images/diginterf_i2s.jpg)

> https://www.tnt-audio.com/clinica/diginterf2_e.html

![I2S Connection](images/i2s-connection.png)

> https://embeddeddesignblog.blogspot.com/2017/07/understanding-i2s-interface-part-1.html

> http://m.eet.com/media/1167789/common_inter_ic_digital_interfaces_for_audio_data_transfer_fig3.jpg

The Philips standard for these signals uses the names WS for word select, SCK for the clock, and SD for the data,
although IC manufacturers seem to rarely use these names in their IC datasheets. Word select is also commonly
called LRCLK, for "left/right clock", and SCK may be called BCLK, for "bit clock" or SCLK for "serial clock".

The name of an IC's serial data pin varies most from one IC vendor to another, and even within a single vendor's
different products. A quick survey of audio IC datasheet shows that the SD signal may also be called SDATA, SDIN,
SDOUT, DACDAT, ADCDAT, or other variations on these, depending on whether the data pin is an input or output.

> http://www.edn.com/design/consumer/4390981/Common-inter-IC-digital-interfaces-for-audio-data-transfer-

> https://web.archive.org/web/20140223115501/http://www.eng.auburn.edu/~nelson/courses/elec5260_6260/Inter-IC%20Sound%20(I2S)%20Bus2.pdf

![I2S Timing](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a7/I2S_Timing.svg/605px-I2S_Timing.svg.png)

<!--
![I2S Timing and Controller](http://audio6.qiniudn.com/wp-content/uploads/2014/04/e38b9e3b7440e23a7c5f1d6b28058ef3.gif)
-->

> http://www.audio6.com/i2s/

### Reference Solution

**Audio Solution in Smartphone**

![Cirrus Logic Audio Solution for Flagship Smartphone](images/CirrusLogic_Smartphone_Solution.png)

> http://www.cirrus.com/en/images/applications/static/APP17.png

**Voice Command Remote Control**

<!--
![Cirrus Logic Audio Solution for Voice Command Remote Control](http://www.cirrus.com/en/images/applications/static/APP105.png)
-->

> http://www.cirrus.com/en/images/applications/static/APP105.png

**Smart Home Automation**

<!--
![Cirrus Logic Audio Solution for Smart Home Automation](http://www.cirrus.com/en/images/applications/static/APP104.png)
-->

> http://www.cirrus.com/en/applications/app/category/APPC3.html

> http://www.cirrus.com/en/applications/app/category/APPC2.html

### I2S vs PCM vs AC97

I2S is a common 4 wire DAI used in HiFi, STB and portable devices. The Tx and
Rx lines are used for audio transmission, whilst the bit clock (BCLK) and
left/right clock (LRC) synchronise the link.

![I2S Interface](http://www.interfacebus.com/I2S-interface-output-serial-data-transfer-format.jpg)

PCM is another 4 wire interface, very similar to I2S, which can support a more
flexible protocol. It has bit clock (BCLK) and sync (SYNC) lines that are used
to synchronise the link whilst the Tx and Rx lines are used to transmit and
receive the audio data. Bit clock usually varies depending on sample rate
whilst sync runs at the sample rate. PCM also supports Time Division
Multiplexing (TDM) in that several devices can use the bus simultaneously (this
is sometimes referred to as network mode).

![PCM Interface](https://www.maximintegrated.com/en/images/appnotes/376/376Fig02.gif)

AC97 is a five wire interface commonly found on many PC sound cards.

![AC97 Interface](https://jreeblog.files.wordpress.com/2012/02/screen-shot-2012-02-16-at-8-41-18-pm.png?w=449&h=160)

> https://www.kernel.org/doc/Documentation/sound/alsa/soc/DAI.txt

> http://prog3.com/sbdm/blog/xushx_bigbear/article/details/48802875

### Codec IC Sample

**WM8960**

![Codec IC WM8960](images/codec_ic_wm8960.png)

> https://www.cirrus.com/en/pubs/proDatasheet/WM8960_v4.2.pdf

**TLV320**

![Codec IC TLV320](images/codec_ic_tlv320.png)

> http://www.ti.com/product/tlv320aic3101/description

> http://www.ti.com.cn/cn/lit/ds/symlink/tlv320aic31.pdf

**AK4951**

![Codec IC AK4951](images/codec_ic_ak4951.png)

> https://www.akm.com/akm/en/file/datasheet/AK4951EN.pdf

**Single-Chip MP3 Decoder - AT83SND2CMP3A1**

![Codec IC AT83SND2CMP3A1](images/codec_ic_at83snd2cmp3.png)

> https://www.pjrc.com/mp3/sta013.html

> http://www.atmel.com/Images/doc7524.pdf

> http://www.atmel.com/Images/doc7525.pdf

**Single-Chip Versatile Decoder - VS1053**

VS1053 is a versatile "MP3 decoder chip" belonging to VLSI Solution's extensive slave audio processor family.

Decodes multiple formats:
- Ogg Vorbis
- MP3 = MPEG 1 & 2 audio layer III (CBR+VBR+ABR)
- MP1 & MP2 = MPEG 1 & 2 audio layers I & II optional
- MPEG4 / 2 AAC-LC(+PNS), HE-AAC v2 (Level 3) (SBR + PS)
- WMA4.0/4.1/7/8/9 all profiles (5-384 kbps)
- FLAC lossless audio with software plugin (upto 24 bits, 48 kHz)
- WAV (PCM + IMA ADPCM)
- General MIDI 1 / SP-MIDI format 0

Encodes three different formats from mic/line in mono or stereo:
- Ogg Vorbis with software plugin
- IMA ADPCM
- 16-bit PCM

![Codec IC VS1053](images/codec_ic_vs1053b.png)

> http://www.vlsi.fi/en/products/vs1053.html

**More Single-Chip MP3/.. Encoder and Decoder**

- http://www.vlsi.fi/en/products.html
- http://www.nxp.com/documents/data_sheet/SAF784X.pdf
- http://rohmfs.rohm.com/en/products/databook/datasheet/ic/audio_video/audio_processor/bu94601kv-e.pdf
- http://www.keil.com/dd/docs/datashts/atmel/at85c51snd3.pdf

## Misc

### Color of Noise

- White
- Pink
- Red (Brownian)
- Grey

![Color of Noise](https://upload.wikimedia.org/wikipedia/commons/6/6c/The_Colors_of_Noise.png)

In audio engineering, electronics, physics, and many other fields, the color of a noise signal (a signal produced by a stochastic process) is generally understood to be some broad characteristic of its power spectrum. Different colors of noise have significantly different properties: for example, as audio signals they will sound different to human ears, and as images they will have a visibly different texture. Therefore, each application typically requires noise of a specific color. This sense of 'color' for noise signals is similar to the concept of timbre in music (which is also called "tone color"); however the latter is almost always used for sound, and may consider very detailed features of the spectrum.

> https://en.wikipedia.org/wiki/Colors_of_noise

### Mixing

**Hardware Mixing**

"Hardware mixing" is a feature that some soundcards support that allows the card
to receive multiple audio streams and play them all at the same time. This is
done entirely in hardware, and does not adversely affect system performance.

> http://www.alsa-project.org/~jfulmer/alsa-faq.html

Most discrete sound cards support hardware mixing, which is enabled by default
if available. Integrated motherboard sound cards (such as Intel HD Audio), usually
do not support hardware mixing. On such cards, software mixing is done by an ALSA
plugin called `dmix`. This feature is enabled automatically if hardware mixing is unavailable.

> https://wiki.archlinux.org/index.php/Advanced_Linux_Sound_Architecture

**Software Mixing**

"Software mixing" is the ability to play multiple sound files or applications
at the same time through the same device.

There are many ways to have software mixing in the Linux environment.
Usually it requires a server application such as ARTSD, ESD, JACK...
The list is large and the apps can often be confusing to use.

> http://www.alsa-project.org/main/index.php/Asoundrc#Software_mixing

> https://wiki.archlinux.org/index.php/Advanced_Linux_Sound_Architecture

## Reference
- https://jan.newmarch.name/LinuxSound/index.html
- http://www.embeddedlinux.org.cn/essentiallinuxdevicedrivers/
- http://processors.wiki.ti.com/index.php/TI-Android-GingerBread-2.3.4-DevKit-2.1_PortingGuides
- http://tedfelix.com/linux/linux-midi.html

# PipeWire

PipeWire is a modern low-level multimedia framework and daemon that aims to both capture and playback minimal latency audio and video. It aims to be the successor to PulseAudio. It supports PulseAudio, JACK, ALSA and GStreamer-based applications.

PipeWire is a complete re-implementation of a PulseAudio and JACK server but still uses `ALSA` for audio and `bluez5` for bluetooth as well as `PulseAudio` for the client API.

PipeWire supports application containers like Flatpak.

## Breakdown:

- **JACK Audio Connection Kit (JACK):** Professional audio server and API for low-latency audio applications and MIDI, usually found in professional audio production environments like recording studios.
- **Advanced Linux Sound Architecture (ALSA):** Kernel-level sound subsystem that provides audio and MIDI functionality on Linux with sound devices. ALSA wholly controls an audio device and does not provide any multiplexing for applications.  Can be managed directly with `alsa-utils`. 
- **PulseAudio:** PulseAudio is a sound server that handles audio routing between audio devices and applications. It supports ALSA and JACK as well as providing routing for multiplexing audio streams. This is now considered legacy and replaced by PipeWire.
- **PipeWire:** PipeWire is a drop-in replacement for PulseAudio that supports both audio and video. It supports ALSA, JACK and PulseAudio applications as well as multiplexing. PipeWire is set to replace both JACK and PuleAudio by providing compatibility APIs, it is still managed by the PulseAudio API client API (`libpulse`).

## PipeWire:

On most distributions, PipeWire is installed with the `pipewire` package. To use the audio server daemon install the `pipewire-audio` package. To support ALSA clients install the `pipewire-alsa` package and to replace the PulseAudio server install the `pipewire-pulse` package. 

### 

systemctl --user enable --now pipewire-pulse.socket

## WirePlumber:

WirePlumber is a session and policy manager for for both PipeWire and PulseAudio. It is used for managing streams to and from their respective devices and applications.


# PipeWire

PipeWire is a modern low-level multimedia framework and daemon that aims to both capture and playback minimal latency audio and video. It aims to be the successor to PulseAudio. It supports PulseAudio, JACK, ALSA and GStreamer-based applications.

PipeWire is a complete re-implementation of a PulseAudio and JACK server but still uses `ALSA` for audio and `bluez5` for bluetooth as well as `PulseAudio` for the client API.

PipeWire supports application containers like Flatpak.

## Breakdown:

- **JACK Audio Connection Kit (JACK):** Professional audio server and API for low-latency audio applications and MIDI, usually found in professional audio production environments like recording studios.
- **Advanced Linux Sound Architecture (ALSA):** Kernel-level sound subsystem that provides audio and MIDI functionality on Linux with sound devices. ALSA wholly controls an audio device and does not provide any multiplexing for applications.  Can be managed directly with `alsa-utils`. 
- **PulseAudio:** PulseAudio is a sound server that handles audio routing between audio devices and applications. It supports ALSA and JACK as well as providing routing for multiplexing audio streams. This is now considered legacy and replaced by PipeWire.
- **PipeWire:** PipeWire is a drop-in replacement for PulseAudio that supports both audio and video and MIDI. It supports ALSA, JACK and PulseAudio applications as well as multiplexing. PipeWire is set to replace both JACK and PuleAudio by providing compatibility APIs, it is still managed by the PulseAudio API client API (`libpulse`).

## PipeWire:

On most distributions, PipeWire is installed with the `pipewire` package. To use the audio server daemon install the `pipewire-audio` package. To support ALSA clients install the `pipewire-alsa` package and to replace the PulseAudio server install the `pipewire-pulse` package. 

### Terminology:

- **Clients:** Clients are open connections to PipeWire to produce or consume audio and video data.
- **Nodes:** A node is an audio or video processor such as a speaker, microphone, camera, or application that produces or consumes data.
- **Modules:** Loadable components that extend PipeWire's functionality, i.e bluetooth or JACK support.
- **Ports:** Ports are the input and output interfaces for a node. A port can either be used for input or output (but not both).
- **Links:** Links are the connection between nodes and their ports. 
- **Devices:** Devices are what nodes are created from as per their respective API.
- **Sinks:** Sinks are the output nodes that consume audio data, such as speakers or headphones.
- **Sources:** Sources are the input nodes that produce audio data, such as microphones.
- **Samples:** Built-in data types that represent audio and video data in PipeWire for testing, like an audio clip.
- **Cards:** Cards are physical or virtual devices that provide audio/video capabilities, such as PCI sound cards, USB interfaces, HDMI devices or bluetooth devices. 
- **Profiles:** Profiles are pre-configured card capabilities used to define how a card can be used, such as stereo output, mono output, hdmi-surround sound etc.
- **Source-outputs:** Source-inputs are the audio streams that are being captured by applications like media players or browsers.
- **Sink-inputs:** Sink-inputs are the audio streams that are being played by applications to a sink.

To interact with PipeWire both `pipewire.service` & `pipewire-pulse.service` user services must be started.

## WirePlumber:

WirePlumber is a session and policy manager for for both PipeWire and PulseAudio. It is used for managing streams to and from their respective devices and applications.

On most distributions the `wireplumber` package can be installed to provide this session manager. 

### Configuration:

For backup purposes the default configuration files are located in `/usr/share/wireplumber/` so do not edit these files directly. For system-wide create configuration files in `/etc/wireplumber/` and for user-specific configuration create files in `~/.config/wireplumber/`.
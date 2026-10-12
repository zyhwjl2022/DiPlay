# Bluetooth and audio coordination

<!-- carlito | Public routing contract; OEM implementations remain in the separate private bridge. -->

DiPlay uses one audio focus coordinator per live sink. Calls, ringtones and Siri take priority over media. Navigation overlays media at reduced volume and requests transient ducking when it is the only DiPlay output. Focus denial or loss mutes DiPlay tracks. A permanent loss does not repeatedly reclaim media focus from the newly selected native source.

When navigation guidance arrives on its own track, the music level is the **Music level during navigation** setting (0, 10, 20, 30, 50, 70 or 100 percent of the original level). 100 percent, shown as *Off (no ducking)*, leaves music untouched; the default 20 percent matches the previous fixed behaviour. The setting is read when the session starts, so a change applies on reconnect. This applies only to the navigation overlay; calls, Siri and focus loss still mute media through their own paths.

The default is enabled; an explicitly saved preference is retained. Standard Android focus cannot force a vendor player that ignores focus to stop.

For a confirmed, bonded CarPlay peer, an available A2DP sink profile can yield Bluetooth music without disconnecting HFP, RFCOMM, the Bluetooth adapter or the pairing. Generic handoff follows granted audio ownership; factory handoff and explicit phone commands remain supported. Only an app-disconnected connection is eligible for restoration. Shared peer leases cover session replacement and a bounded retry handles an in-progress disconnect. Restricted APIs fall back to normal focus routing.

Each output role stores a device identity. Calls and Siri also store separate microphone identities. Hotplug resolves identities again, using system routing for absent or rejected devices. Legacy factory stream mapping and numeric device controls remain available.

Communication mode has a process-wide lease. An active native or other application call is never replaced. A ringing mode may transition to CarPlay communication and is restored on completion. Android 12+ uses communication-device selection; older versions request SCO only for an explicitly selected Bluetooth voice device and wait for connection before capture. A five-second timeout returns to system routing. DiPlay releases only its own SCO, speaker and communication requests.

Microphone capture yields on focus loss, resumes with the same negotiated RTP counters and uses echo cancellation/noise suppression when supported. Failed capture retries are delayed. Background-session adoption rebinds the audio ownership listener to the live controller.

Platform references: [audio focus](https://developer.android.com/media/optimize/audio-focus), [AudioManager communication devices and SCO](https://developer.android.com/reference/android/media/AudioManager).

The release workflow supplies the established signing identity and MFi credentials. No private credentials or closed bridge implementation are part of this source tree. Vehicle-specific behavior requires physical verification.

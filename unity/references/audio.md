# Unity - Audio

**Pages:** 1

---

## Speech-to-text and audio injection

**URL:** https://docs.unity.com/ugs/en-us/manual/vivox-unity/manual/Unity/speech-to-text/stt-audio-injection

**Contents:**
- Speech-to-text and audio injection#
  - IVivoxService.StartAudioInjection#
  - IVivoxService.StopAudioInjection#
  - IVivoxService.IsInjectingAudio#

You can have speech-to-text transcribe injected audio by using the following API methods.

Use this method to start audio injection inside a channel. Injected audio is played only into the channels you're transmitting into. This method takes the following parameter:

audioFilePath: The full pathname for the .wav file for audio injection. The file must be single channel, 16-bit PCM, with the same sample rate as the negotiated audio codec. This file is required.

Use this method to stop audio injection inside a channel.

Use this getter to check whether audio is being injected.

**Examples:**

Example 1 (unknown):
```unknown
IVivoxService.StartAudioInjection
```

Example 2 (unknown):
```unknown
audioFilePath
```

Example 3 (unknown):
```unknown
IVivoxService.StopAudioInjection
```

Example 4 (unknown):
```unknown
VivoxService.Instance.StartAudioInjection(path);
await Task.Delay(10000);
VivoxService.Instance.StopAudioInjection();
await Task.Delay(10000);
```

---

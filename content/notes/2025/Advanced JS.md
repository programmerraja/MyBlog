---
title : Advanced JS
date : 2025-01-31T05:36:15.1515+05:30
draft : true
tags : 
---

## Blob

In JavaScript, a **Blob** (Binary Large Object) represents raw binary data, such as images, audio, or text files. It provides a way to store and manipulate large amounts of data efficiently.

```js
const blob = new Blob(["Hello, Blob!"], { type: "text/plain" });
console.log(blob); // Blob {size: 12, type: "text/plain"}
blob.text().then(console.log); // Output: "Hello, Blob!"

```

To Read
```js
const blob = new Blob(["Hello, Blob!"], { type: "text/plain" });

const reader = new FileReader();
reader.onload = function (event) {
  console.log(event.target.result); // Output: "Hello, Blob!"
};
reader.readAsText(blob);


```

### ArrayBuffer

Raw data 

Typed Array raw data in 8bit,16 bit ,32 bit etc which unit8Array ,,

## AudioContext

 is the central interface of the **Web Audio API** that enables developers to generate, manipulate, and control audio in a web application. It acts as the **audio processing environment**, managing the creation and connection of **audio nodes** to form an **audio graph**.

```js
const audioCtx = new (window.AudioContext || window.webkitAudioContext ();

```

The `AudioContext` manages various aspects of an audio graph, including:

|Component|Description|
|---|---|
|**`createOscillator()`**|Generates sound (waveforms)|
|**`createBufferSource()`**|Plays audio files|
|**`createGain()`**|Controls volume|
|**`createBiquadFilter()`**|Applies audio filtering effects|
|**`createAnalyser()`**|Analyzes frequency and waveform data|
|**`createPanner()`**|Controls 3D spatial audio|
|**`destination`**|The final output (e.g., speakers)|
The `AudioContext` can have different states:

|State|Description|
|---|---|
|`suspended`|The context is created but not running (default in some browsers)|
|`running`|The context is active and processing audio|
|`closed`|The context is permanently stopped|

Audio Graph

The structure of interconnected audio nodes that process and route sound using the **Web Audio API**. It allows developers to create, modify, and control audio streams dynamically in a web application.

Each **node** represents an audio processing unit that can generate, modify, or analyze audio data.

- **Source Nodes**: Generate audio (`AudioBufferSourceNode`, `OscillatorNode`)
- **Processing Nodes**: Modify or analyze audio (e.g., `GainNode`, `BiquadFilterNode`, `AnalyserNode`)
- **Destination Node**: Represents the final output (usually the speakers)

`[Source Node] --> [Processing Nodes] --> [Destination Node]`

```js
// Create an AudioContext
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

// Create an Oscillator Node (Sound Source)
const oscillator = audioCtx.createOscillator();
oscillator.type = 'sine'; // Sine wave
oscillator.frequency.setValueAtTime(440, audioCtx.currentTime); // 440 Hz (A4 note)

// Create a Gain Node (Volume Control)
const gainNode = audioCtx.createGain();
gainNode.gain.setValueAtTime(0.5, audioCtx.currentTime); // Set volume to 50%

// Connect nodes together to form the audio graph
oscillator.connect(gainNode);
gainNode.connect(audioCtx.destination); // Connect to speakers

// Start the oscillator
oscillator.start();


//to stop

oscillator.stop(10); // stop after 10 sec

oscillator.disconnect(); // garabage collect else memory leak
```


### Basic of sound

```
        Amplitude (Loudness)
          ↑
          │      ---      ---      ---
          │     /   \    /   \    /   \
          │----/-----\--/-----\--/-----\-----
          │  /        \/       \/        \
          │----------------------------------> Time
                        ↑
                 Wavelength (Pitch)

```
**Frequency**

**Frequency** refers to how many sound wave cycles occur in **one second** and is measured in **Hertz (Hz)**. It determines the **pitch** of the sound.

| Frequency (Hz)     | Type of Sound                       |
| ------------------ | ----------------------------------- |
| **20 Hz - 60 Hz**  | Deep Bass (Subwoofer sounds)        |
| **60 Hz - 250 Hz** | Bass (Kick drum, male voices)       |
| **250 Hz - 2 kHz** | Midrange (Guitar, piano, speech)    |
| **2 kHz - 6 kHz**  | Upper Midrange (Clarity, presence)  |
| **6 kHz - 20 kHz** | Treble (Hi-hats, cymbals, airiness) |
**sample rate**
The **sample rate** decides how frequently we "take a snapshot" of the continuous wave.

**Bit rate**

**Bit Rate** refers to how much audio data is processed per second in a digital audio file. It is measured in **kilobits per second (kbps)**.

**Bit rate** refers to how much **data** is used to store each sample, and it’s directly related to **audio quality**.

|Bit Rate|Quality|Usage|
|---|---|---|
|**32 kbps**|Very Low|Voice recordings, podcasts|
|**96 kbps**|Low|FM Radio quality|
|**128 kbps**|Standard|Common for MP3 audio|
|**192 kbps**|Good|Decent music quality|
|**256 kbps**|High|Used in online streaming|
|**320 kbps**|Very High|Studio-quality MP3|
**Bit Depth (Bits)**

**Bit Depth** defines the number of bits used to store each audio sample. 

bit depth represent the Y axis in the sound where it go from 0 to 65k top

| Bit Depth  | Dynamic Range | Use Case                                |
| ---------- | ------------- | --------------------------------------- |
| **8-bit**  | 48 dB         | Old video games, low-quality audio      |
| **16-bit** | 96 dB         | CD-quality audio                        |
| **24-bit** | 144 dB        | Studio-quality, professional recordings |


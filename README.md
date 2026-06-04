# 🧠 OtosakuStreamingASR-iOS

**OtosakuStreamingASR** is a lightweight **on-device streaming speech recognition engine** for iOS. It performs real-time audio processing using a Conformer-based architecture and CTC decoding.

> [!IMPORTANT]
> ### 🚀 Successor project: VoxRT
> Streaming on-device ASR has moved to **[VoxRT](https://github.com/VoxRT)** — a ground-up
> rewrite on a custom Rust inference runtime. Same idea as `OtosakuStreamingASR`, but faster,
> smaller, and now on **both** platforms:
>
> - **iOS →** [**voxrt-asr-ios**](https://github.com/VoxRT/voxrt-asr-ios) —
>   NEON-accelerated FastConformer (32M), RTF **0.08–0.10** on iPhone 13 Pro Max, fully offline.
> - **Android →** [**voxrt-asr-android**](https://github.com/VoxRT/voxrt-asr-android) —
>   same engine, **~150 ms** latency, no cloud.
>
> This repo stays up for reference, but new development happens in VoxRT. 👉 Start there.

## 🚀 Features

* ✅ Fully offline
* 🎙 Real-time streaming speech recognition
* 🛠 Modular architecture (feature extractor, encoder, decoder)

---
## 🎥 Demo

Watch the model running live on iPhone 13:

![Demo running on iPhone](Media/demo.gif)

---

## 📆 Installation

Add the Swift Package to your Xcode project:

```swift
https://github.com/Otosaku/OtosakuStreamingASR-iOS
```

---

## 🧰 Usage Example

```swift
import OtosakuStreamingASR
                                                                                                
let asr = OtosakuStreamingASR()

try asr.prepareModel(from: modelURL)

asr.subscribe { text in
    print("🗣 Recognized: \(text)")
}

// Raw audio chunk: [Double] in range [-1.0, 1.0], strictly 2559 samples per chunk (80ms at 16kHz)
try asr.predictChunk(rawChunk: yourRawAudioChunk)

try asr.stop() // Finalize and decode remaining buffer

asr.reset() // Reset internal model state
```

---

## 🧠 Model Details

* **Architecture**: Fast Conformer (Cache-Aware Streaming)
* **Language**: 🇷🇺 Russian (fine-tuned from English)
* **Training**: 250 hours of Russian speech (30 epochs)
* **WER (Word Error Rate)**:

  * Russian (fine-tuned): **11%**
  * English (before fine-tuning): **6.5%** on LibriSpeech `test-other`

🔗 **Download Russian model:**
[Link to model](https://drive.google.com/file/d/1Bk1U-mJz1hxd4l_t1VPWv3Z57N0oqUuD/view?usp=sharing)

For other languages or custom domains, contact me:

📧 **[otosaku.dsp@gmail.com](mailto:otosaku.dsp@gmail.com)**

---

## 🧵 OtosakuStreamingASR API

| Method                    | Description                           |
| ------------------------- | ------------------------------------- |
| `prepareModel(from:)`     | Load model from directory             |
| `predictChunk(rawChunk:)` | Submit audio frame (`[Double]`)       |
| `stop()`                  | Finalize and decode remaining buffer  |
| `reset()`                 | Reset model state                     |
| `subscribe { String in }` | Receive transcribed text in real time |

⚠️ Input audio must be sampled at `16kHz` and normalized to `[-1.0, 1.0]`, strictly 2559 samples per chunk (80ms at 16kHz)

---

## 🔒 Privacy First

This package is designed with privacy in mind:

* Runs **entirely on-device**
* No cloud calls or external dependencies

---

## 📩 Contact

If you have any questions, suggestions, or are interested in adapting the model to another language or domain:

**Email:** [otosaku.dsp@gmail.com](mailto:otosaku.dsp@gmail.com)

---

## 📄 License

MIT License

---
title: '🔥 M4 Crushes M2: Apple Silicon VLM Benchmark Shows Up to 5× Speed Boost!'
date: 2025-09-17 20:58:50
tags:
 - Vision Language Models
 - Multimodal AI
 - Apple Silicon
 - Benchmark
 - MPS
---

# 🚀 Image Captioning and Visual Understanding with VLM

Ever wondered how fast cutting-edge vision-language models can run on Apple Silicon? This script puts **Moondream2** to the test with a wide range of visual understanding tasks:

* 📝 **Image Captioning** — short, detailed, streamed, and non-streamed
* ❓ **Visual Queries** — ask natural language questions about what’s inside the image
* 🎯 **Object Detection** — find all instances of a given object
* 📍 **Pointing** — locate objects in the image by name

It also benchmarks **runtime performance** of these tasks to compare different Apple Silicon chips (M2 vs M4) using the **Metal Performance Shaders (MPS)** backend.

<!-- more -->

```python
import time
from transformers import AutoModelForCausalLM
from PIL import Image

# Load the model
model = AutoModelForCausalLM.from_pretrained(
    "vikhyatk/moondream2",
    revision="2025-06-21",
    trust_remote_code=True,
    device_map={"": "mps"}  # Change to "mps" on Apple Silicon if needed
)

# Load the image
image_path = "/tmp/img.png"
image = Image.open(image_path).convert("RGB")

# Track total runtime
global_start = time.time()

# --- Captioning: Short ---
print("Short caption:")
t0 = time.time()
print(model.caption(image, length="short")["caption"])
print(f"[Short caption runtime: {time.time() - t0:.3f} sec]\n")

# --- Captioning: Normal (streamed with benchmarking) ---
print("Normal caption (streamed with timing):")
t0 = time.time()

stream = model.caption(image, length="normal", stream=True)["caption"]

latencies = []
start_time = time.time()
first_token_time = None

for i, token in enumerate(stream):
    now = time.time()
    if i == 0:
        first_token_time = now - start_time
    else:
        latencies.append(now - prev_time)
    prev_time = now

    print(token, end="", flush=True)

total_time = time.time() - start_time
avg_latency = sum(latencies) / len(latencies) if latencies else 0.0

print("\n\n--- Benchmark Results ---")
print(f"First token latency: {first_token_time:.3f} sec")
print(f"Average token latency: {avg_latency:.3f} sec")
print(f"Total generation time: {total_time:.3f} sec")
print(f"[Normal caption runtime: {time.time() - t0:.3f} sec]\n")

# --- Final Caption (non-streaming version) ---
t0 = time.time()
final_caption = model.caption(image, length="normal")["caption"]
print("Full caption (non-streamed):")
print(final_caption)
print(f"[Final caption runtime: {time.time() - t0:.3f} sec]\n")

# --- Visual Querying ---
print("Visual query: 'How many people are in the image?'")
t0 = time.time()
print(model.query(image, "How many people are in the image?")["answer"])
print(f"[Visual query runtime: {time.time() - t0:.3f} sec]\n")

# --- Object Detection ---
print("Object detection: 'face'")
t0 = time.time()
objects = model.detect(image, "face")["objects"]
print(f"Found {len(objects)} face(s)")
print(f"[Object detection runtime: {time.time() - t0:.3f} sec]\n")

# --- Pointing ---
print("Pointing: 'person'")
t0 = time.time()
points = model.point(image, "person")["points"]
print(f"Found {len(points)} person(s)")
print(f"[Pointing runtime: {time.time() - t0:.3f} sec]\n")

# --- Total Runtime ---
global_end = time.time()
print(f"=== Total end-to-end runtime: {global_end - global_start:.3f} sec ===")
```

---

## Code Overview

1. **Load Model & Image**

   * Loads the `vikhyatk/moondream2` model with custom methods (`caption`, `query`, `detect`, `point`).
   * Opens a test image (`/tmp/img.jpg`) for processing.

2. **Captioning**

   * Generates a **short caption** (one sentence).
   * Generates a **normal caption**:

     * In **streaming mode**, printing tokens as they arrive while measuring latency.
     * In **non-streaming mode**, returning the full caption at once.

3. **Visual Query**

   * Answers natural-language questions about the image, e.g. *"How many people are in the image?"*

4. **Object Detection**

   * Detects specified objects, e.g. *"face"*.

5. **Pointing**

   * Finds spatial locations of objects, e.g. *"person"*.

6. **Benchmarking**

   * Tracks runtime for each task.
   * Reports **first token latency, average token latency, total generation time**, and **end-to-end runtime**.

---

## Results: M2 vs M4 (MPS Device)

We going to compare with this two image:
 - Little snowman
 - The public speech

<div style="display: flex; flex-direction: row; justify-content: center;">
    <img src="/2025/09/17/Benchmarking-VLM-on-Apple-Silicon-M2-vs-M4-MPS-Backend/img.jpg" height="200">
    <img src="/2025/09/17/Benchmarking-VLM-on-Apple-Silicon-M2-vs-M4-MPS-Backend/img.png" height="200">
</div>

### Little Showman


| Task                      | M2 Runtime (s) | M4 Runtime (s) | Speedup (≈)     |
| ------------------------- | -------------- | -------------- | --------------- |
| Short caption             | 13.37          | 4.44           | **3× faster**   |
| Normal caption (stream)   | 29.45          | 5.84           | **5× faster**   |
| Full caption (non-stream) | 21.45          | 5.77           | **4× faster**   |
| Visual query              | 15.19          | 3.58           | **4× faster**   |
| Object detection          | 13.21          | 3.38           | **4× faster**   |
| Pointing                  | 12.67          | 3.36           | **4× faster**   |
| **Total runtime**         | 105.34         | 26.36          | **\~4× faster** |

### Public speech


| Task                  | M2 Runtime (s) | M4 Runtime (s) | Speedup |
| --------------------- | -------------- | -------------- | ------- |
| Short Caption         | 13.2           | 4.3            | \~3×    |
| Normal Caption (full) | 20.0           | 6.8            | \~3×    |
| Visual Query          | 13.0           | 3.6            | \~3.5×  |
| Object Detection      | 12.9           | 3.5            | \~3.5×  |
| Pointing              | 14.9           | 3.8            | \~4×    |
| **Total Runtime**     | 95.3           | 28.9           | \~3.3×  |

### Key Takeaways

* **M4 dramatically outperforms M2** across all tasks, often by a factor of 3–5×.
* **First token latency** (important for streaming applications) is much lower on M4 (0.08s vs 0.16s).
* **Average token latency** improves significantly (0.043s vs 0.214s).
* This makes M4 much better suited for **interactive multimodal applications** like real-time captioning, chat, or object recognition.

---

## Example Output (Short Caption)

**M2 & M4 produced nearly identical captions**, but M4 was \~3× faster.

```
Short caption:
A white snowman figurine with green scarf and black band around neck sits on a red and black checkered base, surrounded by winter-themed items on a white surface.
```

---

## How to Run

```bash
pip install transformers pillow
python script.py
```

Replace `device_map={"": "cuda"}` with `"mps"` for Apple Silicon.
Update `image_path` to your own test image.

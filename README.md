# HORUS: American Sign Language Translation and Learning App

HORUS is an interactive application that enables seamless two-way communication between sign language users and non-signers. It also provides an engaging and educational platform for users to learn American Sign Language (ASL).

## Project Overview

**HORUS** is a real-time American Sign Language (ASL) translation and learning system built using deep learning and computer vision. The system enables:

- Accurate, low-latency translation of ASL gestures into text using a transformer-based model trained on MediaPipe-derived 3D keypoints.
- Reverse communication where non-signers can input text and have it displayed in sign language form.
- An interactive ASL learning module that guides users through sign practice with automated feedback on gesture correctness.

This project focuses on building a scalable, efficient pipeline that combines on-device keypoint extraction with server-side neural inference, leveraging transformer attention mechanisms to handle spatial and temporal dynamics of ASL gestures. HORUS is designed to be accessible, educational, and deployable across devices via a React Native frontend and optimized backend inference stack.

---

## Approach

I leveraged [MediaPipe](https://mediapipe.dev/) to track 3D coordinates of a user's hands, face, and upper body. These keypoints are processed by our transformer-based model, which performs the following tasks:

- **Sign-to-Text Translation**: Detect valid keypoints and convert gestures into corresponding words.
- **Text-to-Sign Support**: Allow text input from a non-signing user, convert to sign language, and present the translated output for accessibility.
- **Learning Module**: Users can practice signs from a curated ASL vocabulary. Our model validates the accuracy of the performed sign before advancing.

---

## Model Architecture

We use a transformer-based neural network with 87 million parameters. Each frame (or sequence of frames) is tokenized using 3D MediaPipe keypoints, encoding both spatial and temporal features. Our model handles:

- **Missing or dropped keypoints** via dropout-aware embedding
- **Temporal dynamics** through attention mechanisms
- **Vocabulary** of ~2.3k ASL words

**Top-K accuracy**:

| Metric | Score |
|--------|-------|
| Top-1  | 0.810 |
| Top-3  | 0.939 |
| Top-5  | 0.958 |

---

## System Architecture

HORUS runs inference on a remote compute instance equipped with:

- **Intel Sapphire Rapids** (4 cores, 16GB RAM)
- **On-chip accelerators**
- **XNNPACK optimization** for low-latency inference
- **React Native frontend** for cross-platform mobile support

---

## Performance

I optimized the system to keep end-to-end translation time under 4 seconds:

| Step       | Time (s) |
|------------|----------|
| MP4 Decode | 0.5      |
| MediaPipe  | 1.5      |
| Model      | 0.5      |
| **Total**  | **2.5**  |

---

## Challenges

- **Limited dataset**: Some labels only had 20–30 samples, requiring data augmentation and ensembling.
- **Latency**: Balancing real-time performance with model complexity and accuracy.
- **Gesture variability**: Ensuring robustness across different users, lighting, and camera angles.

---

## Tech Stack

- **Frontend**: React Native & Expo Go
- **Backend**: Python & PyTorch
- **Model**: Transformer architecture
- **Computer Vision**: MediaPipe
- **Hardware Acceleration**: XNNPACK, On-chip acceleration

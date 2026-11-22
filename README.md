# Webcam Gesture Toolkit

**Hand tracking and gesture interaction for Unity using a standard webcam.**

Webcam Gesture Toolkit by **EDC** lets you control interactions in Unity through hand poses, without dedicated tracking hardware. Powered by **MediaPipe**, detection runs locally on the CPU.

## Local processing and privacy
**Hand tracking and gesture recognition run entirely on your computer.** Webcam frames are processed locally on the CPU, without being uploaded to a server.
The toolkit does not send webcam images, hand landmarks or gesture results over the internet. Its tracking and recognition features require no cloud service, online account or internet connection.

## What can you build?

- **Touchless interfaces** — activate buttons, menus and objects with hand gestures.
- **Gesture-controlled games** — use hand poses to trigger gameplay actions.
- **Interactive installations** — create webcam-driven experiences for exhibitions and demonstrations.
- **Educational applications** — build activities that respond to hand poses.
- **Prototypes** — experiment with gesture input using an ordinary webcam.

## Features

- Track up to **two hands** and access hand landmarks.
- Recognize **five built-in poses**: open palm, fist, pointing index finger, index and middle fingers extended and extended thumb.
- Record custom hand pose templates in the Unity Editor and recognize them in your application.
- Access immediate and stabilized gesture results through a **C# API**.
- Connect gesture activation and deactivation to actions using **UnityEvents**.
- Get started with included prefabs and sample scenes.

## Requirements and scope

The initial release targets **Windows x64**, **Unity 6000.3.8f1** and **URP 17.3.0**.

A webcam, Windows camera permissions and a compatible Microsoft Visual C++ x64 runtime are required.
If a compatible version is not already installed, download and install the latest supported **X64** package from
[Microsoft's official download page](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist/).

Custom templates recognize individual hand poses, rather than movement sequences. Recording and saving templates are Editor authoring features.

## Project status

Version 1.0 is being prepared for submission to the Unity Asset Store.

This repository hosts documentation, development progress and third-party license/source materials. The installable Unity plugin will be distributed separately.

---

Developed by **EDC** · Maintained on GitHub by **keimasoft** · Powered by **MediaPipe**

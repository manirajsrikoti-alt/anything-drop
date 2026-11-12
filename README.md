# AnythingDrop 📡

> **Fast, Private, P2P Cross-Device File Sharing Web Application**  
> *Transfer files, photos, videos, and text directly between any device via browser — No setup, no accounts, no file size limits.*

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Node.js](https://img.shields.io/badge/Node.js-18%2B-green.svg)](https://nodejs.org/)
[![WebRTC](https://img.shields.io/badge/Protocol-WebRTC%20P2P-orange.svg)](https://webrtc.org/)
[![PWA Ready](https://img.shields.io/badge/PWA-Installable-purple.svg)](https://web.dev/progressive-web-apps/)

---

## 🌟 Overview & Problem Statement

Sharing files across different operating systems (e.g. from an Android phone to a Mac, or from Windows to iPhone) has always been frustrating:
* **AirDrop** only works within the Apple ecosystem.
* **Cloud Storage** (Google Drive, Dropbox) consumes internet bandwidth, compresses photos, and exposes private data to third-party servers.
* **Messaging Apps** (WhatsApp, Telegram) compress files and require phone numbers.

**AnythingDrop** solves this by establishing **direct peer-to-peer (P2P) connections** between browsers using **WebRTC DataChannels**. Traffic stays inside your local Wi-Fi network and flows directly from sender to receiver at maximum hardware speeds.

---

## 🚀 Key Features

* ⚡ **Peer-to-Peer Transfer:** Uses WebRTC DataChannels to transfer files directly between devices without uploading to any intermediate server.
* 🔒 **End-to-End Encryption:** Zero-knowledge privacy. Data is encrypted peer-to-peer and never stored on any cloud disk.
* 📱 **Universal Cross-Platform:** Works on any device with a modern browser — iOS, Android, macOS, Windows, Linux.
* 📲 **Progressive Web App (PWA):** Can be installed to the home screen on iPhone, iPad, and Android with full standalone app feel.
* 🔗 **Permanent Device Pairing:** Pair your personal devices (e.g., your laptop and phone) with a secure key so they find each other even across different networks.
* 🏷️ **Temporary Public Rooms & QR Codes:** Generate a room link or QR code to share files with classmates or colleagues outside your local Wi-Fi.
* 💬 **Integrated Clipboard & Text Sharing:** Send text snippets, URLs, and clipboard contents instantly.
* 🌐 **WebSocket Relay Fallback:** Automatically falls back to encrypted server relay if symmetric NAT/firewalls block direct UDP WebRTC packets.

---

## 🏗️ System Architecture

```text
       ┌────────────────────────┐
       │     AnythingDrop       │
       │   Signaling Server     │
       │ (Node.js + WebSockets) │
       └───────────┬────────────┘
                   │
         1. SDP & ICE Candidate
            Signaling Exchange
                   │
        ┌──────────┴──────────┐
        ▼                     ▼
 ┌──────────────┐     2. Direct P2P      ┌──────────────┐
 │   Device A   │◄──────────────────────►│   Device B   │
 │   (Laptop)   │   WebRTC DataChannel   │   (Mobile)   │
 └──────────────┘ (Direct File Transfer) └──────────────┘
```

1. **Signaling Phase:** Devices connect to the lightweight Node.js WebSocket server to discover each other and exchange WebRTC session descriptions (SDP) and ICE candidates.
2. **P2P Transfer Phase:** Once the handshake is complete, a direct encrypted `RTCDataChannel` is created between the two browsers. Files are read as `ArrayBuffer` binary chunks and streamed directly between devices.
3. **Zero Storage:** The server never receives, buffers, or inspects the transferred files.

---

## 🎓 College Showcase & Viva Q&A Guide

When presenting this project to professors, evaluators, or in viva exams, highlight these key engineering concepts:

### 1. How does WebRTC work here?
> *"WebRTC (Web Real-Time Communication) provides peer-to-peer audio, video, and arbitrary data transfer. We use WebRTC's `RTCDataChannel` API with SCTP protocol over DTLS/UDP, ensuring high-throughput, low-latency binary file streaming."*

### 2. What is the role of the Signaling Server?
> *"Browsers cannot discover each other on the internet without assistance due to NATs and firewalls. Our Node.js WebSocket signaling server acts as a matchmaker—it tells Device A about Device B's network endpoint, after which the direct peer connection takes over."*

### 3. What happens if direct P2P is blocked by a college firewall?
> *"In strict enterprise or campus networks with symmetric NATs where STUN cannot resolve peer ports, AnythingDrop automatically activates an integrated WebSocket relay fallback so file transfers never fail."*

### 4. How to demonstrate in 30 seconds:
1. Open AnythingDrop on your laptop (`http://localhost:3000`).
2. Ask the professor or friend to scan the QR code on your screen using their mobile phone camera.
3. Instantly, their phone appears as a glowing avatar on your laptop screen.
4. Drag and drop any 50MB video/document on their avatar — watch it transfer in seconds without touching the internet!

---

## 💻 Quick Start (Local Setup)

### Prerequisites
* [Node.js](https://nodejs.org/) (v18 or higher)
* `npm`

### 1. Clone & Install
```bash
git clone https://github.com/manirajsrikoti-alt/anything-drop.git
cd anything-drop
npm install
```

### 2. Run the Server
```bash
npm start
```
The server will start at `http://localhost:3000`.

### 3. Access on Mobile Devices (Same Wi-Fi)
1. Find your machine's local IP address:
   * **macOS / Linux:** Run `ifconfig | grep "inet "`
   * **Windows:** Run `ipconfig`
2. Open `http://<YOUR_LOCAL_IP>:3000` in your phone's browser (Safari/Chrome).

---

## 🐳 Docker Deployment

Run with a single Docker command:

```bash
docker run -d \
  --name anything-drop \
  --restart=always \
  -p 3000:3000 \
  ghcr.io/manirajsrikoti-alt/anything-drop:latest
```

Or build locally:
```bash
docker build -t anything-drop .
docker run -p 3000:3000 anything-drop
```

---

## ☁️ 1-Click Cloud Deployment

### Deploy on Render
Click to deploy using the included `render.yaml`:
1. Fork or push this repository to your GitHub.
2. Link your GitHub account to [Render.com](https://render.com).
3. Create a **New Web Service** and select this repo — Render will automatically detect `render.yaml` and deploy it on the free tier.

### Deploy on Railway
1. Go to [Railway.app](https://railway.app).
2. Choose **Deploy from GitHub repo** and select `anything-drop`.
3. Railway will build using Nixpacks/Dockerfile and provide a live public HTTPS URL with automatic WebSockets support.

---

## 📜 Technology Stack

* **Frontend:** HTML5, CSS3 (Modern Glassmorphism & Responsive Flexbox), JavaScript (ESModules), Web Workers, Service Workers (PWA).
* **Backend:** Node.js, Express.
* **Communication Protocols:** WebRTC (RTCDataChannel), WebSockets (`ws`), STUN (Google STUN: `stun.l.google.com:19302`).
* **Deployment & Containerization:** Docker, Alpine Linux, Render, Railway.

---

## 🤝 Contributing & License

Contributions, issues, and feature requests are welcome!  
This project is open-source under the [GNU General Public License v3.0 (GPL-3.0)](LICENSE), building upon the open-source foundations of PairDrop and Snapdrop.

**Created by [Maniraj Srikoti](https://github.com/manirajsrikoti-alt)**

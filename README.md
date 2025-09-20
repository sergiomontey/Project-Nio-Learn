# Project-Nio-Learn


# Nio Learn — White‑Label Corporate Training App (Mobile‑First, On‑Device AI)

> A miniature, rebrandable corporate training app proving **on‑device inference**, **offline workflows**, and the **AI kernel** concept.

---

## TL;DR
- **Platforms:** iOS + Android (React Native UI)  
- **Native AI Modules:** Swift (Core ML) • Kotlin (TensorFlow Lite) bridged to RN  
- **Key Proofs:** Offline Q&A chatbot, biometric auth, push notifications, mock LMS integration, one‑file theming  
- **White‑Labeling:** `theme.json` controls logo, colors, and app name

---

## Screens & Badges
> (Add screenshots/GIFs here: onboarding, course list, training detail, AI Assistant Q&A, completion modal)
<!-- Example build/status badges -->
![CI](https://img.shields.io/badge/CI-GitHub_Actions-informational)
![Platforms](https://img.shields.io/badge/iOS-16%2B-blue)
![Platforms](https://img.shields.io/badge/Android-8%2B-green)

---

## Why This Project Exists
Recruiters and buyers ask: *“Can you prove mobile‑first AI that runs offline and is governance‑ready?”*  
**Aura Learn** answers with a working app that’s:
- **Rebrandable** (white‑label)
- **Cross‑platform** (React Native + native modules)
- **Edge‑aware** (Core ML/TFLite on device)
- **Enterprise‑savvy** (biometrics, push, audit logs, mock LMS API)

---

## Architecture (High Level)

```
React Native UI
  ├─ Auth (Biometric gate) → Keychain/Keystore
  ├─ Course List / Detail → Mock LMS API (HTTPS)
  ├─ AI Training Assistant (RN bridge)
  │     ├─ iOS Swift Module → Core ML (distilled BERT/miniLM)
  │     └─ Android Kotlin Module → TensorFlow Lite
  └─ Audit Log (local encrypted store → optional export)
```

**Cross‑Platform Strategy**
- RN for fast UI iteration.
- Native modules for performance‑critical inference.
- Clean RN bridges for JS consumption.

---

## White‑Labeling & Theming

All brand knobs live in `config/theme.json`:

```json
{
  "appName": "Nio Learn",
  "logo": "./assets/logo.png",
  "colors": {
    "primary": "#2D6AE3",
    "secondary": "#101828",
    "accent": "#00C2A8"
  },
  "lms": {
    "apiBaseUrl": "https://localhost:3001",
    "cdnBaseUrl": "https://localhost:3001/cdn"
  },
  "notifications": {
    "reminderCadenceDays": 7
  }
}
```

**Tip:** Create branches like `client-acme` and `client-borealis` to show two branded builds from the same codebase.

---

## AI “Training Assistant” (On‑Device, Offline)

**Model options:** distilled BERT / MiniLM from HF → convert to **Core ML** (iOS) + **TFLite** (Android).  
**Packaging:**  
- iOS: place `.mlmodelc` under `ios/Models/` (add to Xcode target)  
- Android: place `.tflite` under `android/app/src/main/assets/models/`

**Swift (conceptual bridge):**
```swift
@objc(AuraAi)
class AuraAi: NSObject {
  @objc
  func answer(_ question: String,
              resolver resolve: RCTPromiseResolveBlock,
              rejecter reject: RCTPromiseRejectBlock) {
    // 1) tokenize question  2) run Core ML  3) return best span/string
    // (Use a compact tokenizer + quantized model for speed)
    let result = "Company policy limits sharing PII to approved vendors under NDA."
    resolve(result)
  }
}
```

**Kotlin (conceptual bridge):**
```kotlin
class AuraAiModule(reactContext: ReactApplicationContext) :
  ReactContextBaseJavaModule(reactContext) {

  override fun getName() = "AuraAi"

  @ReactMethod
  fun answer(question: String, promise: Promise) {
    // 1) tokenize  2) TFLite Interpreter run  3) best answer
    promise.resolve("Company policy limits sharing PII to approved vendors under NDA.")
  }
}
```

**RN Usage:**
```ts
import { NativeModules } from 'react-native';
const { AuraAi } = NativeModules;
const response = await AuraAi.answer("What's our PII vendor policy?");
```

> ✅ **Validation:** proves **on‑device inference**, **offline capability**, and an **edge‑aware AI kernel** exposed to RN.

---

## LMS Integration & Security

**Biometric Auth:** Face ID / Touch ID / Android Biometrics for gatekeeping app unlock.  
**Mock LMS API:** Node/Express simulating course list + completion updates.

```js
// server/index.js (excerpt)
app.get("/api/courses", (_, res) => res.json([{ id: "sec101", title: "Data Security" }]));
app.post("/api/courses/:id/complete", (req, res) => res.status(200).json({ ok: true }));
```

**Push Notifications:** training reminders based on `reminderCadenceDays`.  
**Encrypted Storage:** use Keychain/Keystore for tokens; consider SQLCipher/EncryptedFile for local audit logs.

---

## Auditability (Enterprise‑Ready)

Every sensitive action creates a **structured local log**:
```
[timestamp] action=ai_answer  model=miniLM-v3  input_hash=0x3f...  result_tokens=12
[timestamp] action=course_complete user=abc123 course=sec101 method=biometric_confirmed
```
Export logs as JSON for compliance reviews.

---

## Getting Started

### Prereqs
- Node 20+, Yarn/PNPM, Watchman
- Xcode 15+, CocoaPods; Android Studio + NDK/SDK
- Ruby + Bundler + Fastlane (optional for releases)

### Install & Run
```bash
git clone <this-repo>
cd aura-learn
yarn install
cd ios && pod install && cd ..
yarn ios   # or: yarn android
```

### Start Mock LMS
```bash
cd server && yarn install && yarn start
```

---

## Folder Layout
```
/app
  /components /screens /hooks
/native-modules
  /ios (Swift)   /android (Kotlin)
/config/theme.json
/server (mock LMS)
```

---

## Roadmap
- [ ] Compress tokenizer/model further (8‑bit quant).
- [ ] Add doc embeddings to boost retrieval for Q&A.
- [ ] Add screenshot test suite + detox e2e.
- [ ] Ship two branded branches to demo white‑labeling.

---

## License
Copyright (c) 2025 MonteyAI LLC. All rights reserved.

This software and associated documentation files (the "Software") are proprietary 
to MonteyAI LLC. and are protected by copyright law.

VIEWING PERMITTED: This code is available for viewing, educational, and 
evaluation purposes only.

RESTRICTIONS:
- No commercial use without explicit written permission from MONTEYcodes
- No redistribution, modification, or derivative works
- No reverse engineering or decompilation
- No use in production environments without valid license



# Kauppalista & Muistiinpanot  
**Shopping List & Notes — Android Production Showcase**

A privacy-first productivity application built for modern Android devices.  
This repository presents a **sanitized architectural showcase** of a real-world production app currently undergoing final Google Play verification.

---

## Overview

**Kauppalista & Muistiinpanot** is a fully offline Android application designed with a strong focus on user privacy, data integrity, and long-term maintainability.

- 📱 **Offline-first by design**
- 🔒 **No accounts, no tracking, no external servers**
- 🌍 **Localized for 10+ languages**
- 🚀 **Built with modern Android best practices**

---

## Privacy-First Philosophy

User data ownership is the core principle of this project.

- **100% Offline Storage** – All data remains on the user’s device  
- **No Tracking or Analytics** – No data collection of any kind  
- **Full User Control** – Manual export and import via encrypted backup files  

---

## Technical Highlights

### Robust Data Management (Room)

- **Schema migrations** demonstrating safe evolution (v16 → v17)  
- **Data integrity** using foreign keys, indices, and transactional operations  
- **Reactive DAOs** powered by Kotlin Flow for real-time UI updates  

### Modern UI & State Management

- **Jetpack Compose + Material 3**
- **Dynamic theming** (Light / Dark / System)
- **Jetpack DataStore** for thread-safe, reactive preferences

### Production-Ready Features

- 🎙 **Google Voice-to-Text** input
- 🌐 **Multi-language support** (Finnish, English, Swedish, German, and more)
- 📊 **Android Vitals monitoring** (currently reporting zero critical issues)

---

## Developer Focus

This project demonstrates:

- Designing a scalable Room database for long-term feature growth  
- Managing schema migrations safely in production  
- Applying modern Android architecture patterns  
- Building privacy-respecting applications without compromising UX  

---

## Project Status

- ✅ **12/12 closed-test milestone completed**
- ⏳ **14-day Google Play production verification in progress**
- 📈 **Stability monitored via Android Vitals**

---

## Repository Notice

This repository contains a **sanitized subset** of the application intended to showcase architectural decisions and technical competence.  
The full production source code and business logic remain private.

---

**Developed by Jan Sarivuo**  

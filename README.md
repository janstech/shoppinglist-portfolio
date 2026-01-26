Shopping List & Notes (Kauppalista & Muistiinpanot)

Android Production Architecture Showcase

This repository documents the architecture and production design decisions behind Shopping List & Notes, a privacy-first Android application released to the Google Play Store.

This is a portfolio and architecture showcase, not a source-code release.

🎯 Project Goals

Offline-first productivity application (shopping lists + notes)

Privacy-first by design: no accounts, no analytics, no ads

Local-first data storage with reliable Room migrations

Modern Android architecture using Jetpack Compose and Material 3

Optional AI-assisted meal and meal-plan suggestions with strict cost and abuse control

🧱 Architecture Overview

The application follows a layered, production-ready architecture:

UI Layer – Jetpack Compose screens and Material 3 components

State Layer – ViewModels with Kotlin Flow / StateFlow for predictable state management

Domain Layer – Use-cases and repository interfaces (business logic isolated from UI)

Data Layer – Room database, DAOs, mappers, and DataStore for settings

System Integration – Biometric authentication, backup/export/import, and multi-language support (14 languages)

Backend Integration – Dedicated FastAPI backend for AI features and security-critical logic

🔐 Privacy & Security

Core features work fully offline

No personal accounts and no tracking SDKs

All user data (lists, favorites, notes, birthdays, settings) is stored locally on the device

Optional biometric locking for sensitive sections (notes and settings)

Backup and restore are fully user-controlled via export/import (share sheet)

Network-based features are strictly isolated and optional

🤖 AI Features (Meal & Meal-Plan Suggestions)

AI functionality is implemented as an optional assistant, not a core dependency.

Client side

User provides short constraints (quick choices + free text + optional servings)

Prompt length is intentionally constrained to control cost and predictability

Clear UI separation between:

single meal suggestions

multi-day meal plans

Backend side (production-grade)

Dedicated FastAPI backend handles all AI interactions

Requests are protected with:

Firebase App Check

Google Play Integrity

AI usage is governed by scope-specific rate limits, enforced server-side:

meal and mealplan are treated as independent features

Each feature has its own daily quota and enforcement logic

Limits are identity-based and deterministic, ensuring:

predictable user experience

protection against abuse

cost control without client-side trust

Optional server-side whitelisting supports internal testing and future premium tiers

This design ensures that AI features remain:

optional

secure

cost-aware

production-safe

📦 Production Status

✅ Live on Google Play (Production track)

✅ Multi-language UI (14 languages)

✅ Lightweight and fast Compose UI

✅ Stable release workflow with signed App Bundles (AAB)

✅ Backend-enforced security and rate limiting for AI features

ℹ️ Source Code Notice

This repository intentionally does not contain application source code.
The full implementation remains private to protect intellectual property and business logic.

Architecture descriptions and selective pseudocode snippets may be provided for technical evaluation purposes.

© 2026 Janstech

--------

Kauppalista & Muistiinpanot

Android-tuotantosovelluksen arkkitehtuuriesittely

Tämä repositorio dokumentoi Kauppalista & Muistiinpanot -Android-sovelluksen
arkkitehtuurin ja tuotantotason suunnitteluratkaisut.

Tämä on portfolio- ja arkkitehtuuriesittely, ei sovelluksen lähdekoodijulkaisu.

🎯 Projektin tavoitteet

Offline-first-tuottavuussovellus (kauppalistat + muistiinpanot)

Tietosuoja lähtökohtana: ei käyttäjätilejä, ei analytiikkaa, ei mainoksia

Paikallinen tiedontallennus ja luotettavat Room-migraatiot

Moderni Android-arkkitehtuuri (Jetpack Compose + Material 3)

Vapaaehtoiset AI-avusteiset ateria- ja ateriaviikkotoiminnot, hallitulla kustannusmallilla

🧱 Arkkitehtuurin yleiskuva

Sovellus noudattaa kerroksellista tuotantoarkkitehtuuria:

UI-kerros – Jetpack Compose -näkymät ja Material 3 -komponentit

Tilanhallinta – ViewModelit + Kotlin Flow / StateFlow

Domain-kerros – Käyttötapaukset ja repositorio-rajapinnat

Data-kerros – Room-tietokanta, DAO:t, mapperit ja DataStore

Järjestelmäintegraatiot – Biometrinen lukitus, varmuuskopiointi ja monikielisyys (14 kieltä)

Backend-integraatiot – AI-toiminnot ja tietoturvakriittinen logiikka eriytetty palvelinpuolelle

🔐 Tietosuoja ja turvallisuus

Sovelluksen perustoiminnot toimivat täysin offline-tilassa

Ei käyttäjätilejä eikä seuranta-SDK:ita

Kaikki käyttäjädatan osat tallennetaan paikallisesti laitteelle

Valinnainen biometrinen lukitus herkille osioille

Varmuuskopiointi ja palautus täysin käyttäjän hallinnassa

Verkkotoiminnot rajattu ja eristetty

🤖 AI-toiminnot (ateriat ja ateriaviikot)

AI-toiminnot on toteutettu vapaaehtoisena apuna:

Käyttäjä antaa rajatut syötteet (pikavalinnat + vapaa teksti)

Sovellus kutsuu erillistä FastAPI-backendia

Pyynnöt suojataan:

Firebase App Check -mekanismilla

Google Play Integrity -tarkistuksilla

Käyttömäärät rajoitetaan toimintokohtaisesti:

yksittäinen ateria

monipäiväinen ateriaviikko

Rajoitukset ja valvonta toteutetaan palvelinpuolella

Ratkaisu mahdollistaa tulevaisuudessa:

premium-tasot

lisä-AI-toiminnot

kustannusten hallinnan ilman client-luottamusta

📦 Tuotantotilanne

✅ Julkaistu Google Play Kaupassa (Production)

✅ Monikielinen käyttöliittymä (14 kieltä)

✅ Kevyt ja nopea käyttöliittymä

✅ Vakaa julkaisuputki (AAB)

✅ Tuotantotason backend-turvallisuus AI-toiminnoille

ℹ️ Lähdekoodihuomautus

Tämä repositorio ei sisällä sovelluksen lähdekoodia.
Koko toteutus pidetään yksityisenä immateriaalioikeuksien ja liiketoimintalogiikan suojaamiseksi.

© 2026 Janstech

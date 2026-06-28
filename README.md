# Shopping List & Notes (Kauppalista & Muistiinpanot)

## Android Production Architecture Showcase

This repository documents the architecture and production design decisions behind
Shopping List & Notes, a privacy-first Android productivity application released
to the Google Play Store.

This is a portfolio and architecture showcase. The full production implementation
and business logic remain private.

## Project Goals

- Offline-first everyday productivity: shopping lists, tasks, notes, recipes,
  meal plans, birthdays, reminders, and personal health tracking.
- Privacy-first core use: no account required for local features, no ads, and
  no sale of user data.
- Local-first persistence with Room, DataStore, schema migrations, and
  user-controlled backup/export/import.
- Modern Android architecture with Kotlin, Jetpack Compose, Material 3,
  ViewModels, Kotlin Flow, and repository boundaries.
- Optional online layers for AI assistance and real-time shared lists, isolated
  from the local core experience.

## Architecture Overview

The application follows a layered production architecture:

- UI layer: Jetpack Compose screens, dialogs, bottom navigation, Material 3
  components, and Android 8+ compatibility considerations.
- State layer: ViewModels with Kotlin Flow / StateFlow for predictable,
  reactive UI state.
- Domain/data boundaries: repositories, mappers, Room DAOs, backup models, and
  DataStore-backed preferences.
- Local persistence: Room database for lists, favorites, notes, folders,
  birthdays, todos, health entries, and local reminders.
- System integration: biometric locking, local notifications, backup/export,
  import, Play In-App Review fallback, Google Play flexible updates, and
  multi-language support.
- Backend integration: FastAPI backend for AI features, Firebase App Check,
  Google Play Integrity, OpenAI calls, and MariaDB-backed rate limiting.
- Firebase collaboration layer: Google Sign-In, Firestore, Cloud Functions, FCM,
  invite links, App Links, roles, and real-time shared-list synchronization.

Detailed documentation:

- [Architecture Overview](architecture-overview.md)
- [Data Layer Design](architecture-data-layer.md)
- [UI & State Management](architecture-ui-layer.md)
- [Privacy & Security](architecture-privacy.md)
- [Pseudocode Samples](architecture-pseudocode.md)

## Privacy & Security

- Core features work without an account and store user content locally on the
  device.
- Google Sign-In is required only for shared-list collaboration.
- AI and shared-list features require network access and are optional.
- Local shopping lists, notes, recipes, birthdays, todos, reminders, and health
  entries are not automatically moved to the cloud.
- Optional biometric locking protects sensitive sections such as notes, recipes,
  meal plans, settings, and health tracking.
- Backup and restore are user-controlled through export/import.
- Shared-list cloud data is scoped to collaboration: list metadata, items,
  membership, roles, invites, notification preferences, and FCM token metadata.
- Member avatars use Google/Firebase profile `photoUrl` values only; the app does
  not upload or store custom avatar files.
- Firebase Analytics is used as a privacy-first, opt-out usage layer only through
  an allowlisted wrapper. Events must not contain list contents, notes, recipes,
  searches, health data, Firebase UID values, or other personal identifiers.

## AI Features

AI functionality is implemented as an optional assistant, not a core dependency.

Client-side AI flows include:

- single meal suggestions
- multi-day meal plans
- ingredient scanning
- recipe options from scanned ingredients
- saving generated recipes and meal plans into notes

Backend-side controls include:

- dedicated FastAPI backend
- Firebase App Check verification
- Google Play Integrity verification with nonce flow
- `X-Client-Id`-based abuse protection
- scope-specific daily and burst rate limits in MariaDB
- whitelist support for internal testing and future premium tiers
- server-side validation and sanitization of AI responses before returning them
  to Android

This keeps AI functionality optional, cost-aware, and protected from
client-side trust assumptions.

## Shared Lists

Shared Lists are a separate real-time collaboration layer for shopping and task
lists. They do not replace the local offline-first Room lists.

Implemented collaboration features include:

- optional Firebase Auth / Google Sign-In identity
- shared shopping and task lists
- invite links with HTTPS App Links and GitHub Pages fallback
- Firestore real-time item synchronization
- owner/editor/viewer roles
- owner-only invite, rename, delete, cleanup, archive, and restore flows
- editor support for adding, editing, toggling, and reordering active rows
- viewer read-only access
- shared-list overview with member count, avatar previews, item count, and
  user-specific ordering
- FCM push notifications with user notification preferences and debounce
- backup schema v5 cloud references without exporting full shared-list content

## Current Feature Set

- Local shopping lists, favorites, tasks, notes, note folders, recipes,
  meal plans, birthdays, reminders, and health entries.
- Room database schema version 23 and backup schema version 5.
- Personal health tracking for blood pressure, weight, height, pulse, and custom
  entries without medical interpretation or health advice.
- Multi-language UI for 14 locales: Finnish, English, German, Spanish, French,
  Italian, Japanese, Swedish, Norwegian, Danish, Hungarian, Estonian, Ukrainian,
  and Chinese.
- Google Play production release workflow with signed Android App Bundles.
- App-controlled review prompt with Play In-App Review and Play Store fallback.
- Google Play flexible In-App Updates.
- FastAPI AI backend with App Check, Play Integrity, OpenAI integration, and
  MariaDB rate limiting.
- Firebase-based shared-list collaboration with Firestore, Cloud Functions, FCM,
  Firestore rules, and App Links.

## Production Status

- Live on Google Play production track.
- Privacy-first local core with optional online AI and collaboration layers.
- 14-language UI.
- Lightweight Compose UI with older Android and small-screen compatibility
  considered.
- Production backend security and server-side AI rate limiting.
- Real-time shared-list architecture implemented with Firebase.

## Source Code Notice

This repository is maintained as a portfolio and architecture showcase. It does
not publish the full private production source code, proprietary business logic,
release credentials, or production secrets.

Architecture descriptions and selective pseudocode snippets may be provided for
technical evaluation purposes.

© 2026 Janstech

---

# Kauppalista & Muistiinpanot

## Android-tuotantosovelluksen arkkitehtuuriesittely

Tämä repositorio dokumentoi Kauppalista & Muistiinpanot -Android-sovelluksen
arkkitehtuurin ja tuotantotason suunnitteluratkaisut.

Tämä on portfolio- ja arkkitehtuuriesittely. Koko tuotantototeutus ja
liiketoimintalogiikka pidetään yksityisenä.

## Projektin tavoitteet

- Offline-first-tuottavuussovellus arjen käyttöön: kauppalistat, tehtävät,
  muistiinpanot, reseptit, viikkoruokalistat, syntymäpäivät, muistutukset ja
  henkilökohtaiset terveystiedot.
- Tietosuoja lähtökohtana: paikalliset perustoiminnot eivät vaadi tiliä,
  sovelluksessa ei ole mainoksia eikä käyttäjädataa myydä.
- Paikallinen tiedontallennus Roomilla, DataStorella, hallituilla
  skeemamigraatioilla sekä käyttäjän hallitsemalla varmuuskopioinnilla,
  viennillä ja tuonnilla.
- Moderni Android-arkkitehtuuri: Kotlin, Jetpack Compose, Material 3,
  ViewModelit, Kotlin Flow ja selkeät repository-rajat.
- Valinnaiset verkkokerrokset AI-avustukselle ja reaaliaikaisille jaetuille
  listoille paikallisesta ydinkäytöstä erillisinä toimintoina.

## Arkkitehtuurin yleiskuva

Sovellus noudattaa kerroksellista tuotantoarkkitehtuuria:

- UI-kerros: Jetpack Compose -näkymät, dialogit, alapalkkinavigaatio,
  Material 3 -komponentit ja Android 8+ -yhteensopivuuden huomiointi.
- Tilanhallinta: ViewModelit, Kotlin Flow ja StateFlow ennustettavaan
  reaktiiviseen UI-tilaan.
- Domain- ja datarajat: repositoryt, mapperit, Room DAO:t, backup-mallit ja
  DataStore-pohjaiset asetukset.
- Paikallinen tallennus: Room-tietokanta listoille, suosikeille,
  muistiinpanoille, kansioille, syntymäpäiville, tehtäville, terveystiedoille ja
  paikallisille muistutuksille.
- Järjestelmäintegraatiot: biometrinen lukitus, paikalliset ilmoitukset,
  backup/export, import, Play In-App Review -fallback, Google Play flexible
  updates ja monikielisyys.
- Backend-integraatio: FastAPI-backend AI-toiminnoille, Firebase App Check,
  Google Play Integrity, OpenAI-kutsut ja MariaDB-pohjainen rate limiting.
- Firebase-yhteistyökerros: Google Sign-In, Firestore, Cloud Functions, FCM,
  kutsulinkit, App Links, roolit ja reaaliaikainen jaettujen listojen synkronointi.

Tarkemmat dokumentit:

- [Architecture Overview](architecture-overview.md)
- [Data Layer Design](architecture-data-layer.md)
- [UI & State Management](architecture-ui-layer.md)
- [Privacy & Security](architecture-privacy.md)
- [Pseudocode Samples](architecture-pseudocode.md)

## Tietosuoja ja turvallisuus

- Perustoiminnot toimivat ilman tiliä ja tallentavat käyttäjän sisällön
  paikallisesti laitteelle.
- Google Sign-In vaaditaan vain jaettujen listojen yhteismuokkaukseen.
- AI- ja shared-list-toiminnot vaativat verkkoyhteyden ja ovat valinnaisia.
- Paikallisia kauppalistoja, muistiinpanoja, reseptejä, syntymäpäiviä,
  tehtäviä, muistutuksia tai terveystietoja ei siirretä automaattisesti pilveen.
- Valinnainen biometrinen lukitus suojaa herkkiä osioita, kuten muistiinpanoja,
  reseptejä, viikkoruokalistoja, asetuksia ja terveystietoja.
- Varmuuskopiointi ja palautus ovat käyttäjän hallinnassa export/import-polulla.
- Jaettujen listojen pilvidata rajautuu yhteistyöhön: listametadataan, riveihin,
  jäsenyyksiin, rooleihin, kutsuihin, ilmoitusasetuksiin ja FCM-tokenmetadataan.
- Jäsenavatarit käyttävät vain Google/Firebase-profiilin `photoUrl`-arvoa;
  sovellus ei lataa tai tallenna omia avatar-kuvatiedostoja.
- Firebase Analytics toimii privacy-first-periaatteella opt-out-käyttötilastointina
  vain sallitun wrapperin kautta. Eventit eivät saa sisältää listojen,
  muistiinpanojen, reseptien, hakujen, terveystietojen, Firebase UID -arvojen tai
  muiden henkilötunnisteiden sisältöä.

## AI-toiminnot

AI-toiminnot on toteutettu vapaaehtoisena apuna, ei sovelluksen
ydinriippuvuutena.

Client-puolen AI-polkuja ovat:

- yksittäiset ateriaehdotukset
- monipäiväiset viikkoruokalistat
- aineskannaus
- skannatuista aineksista muodostetut reseptivaihtoehdot
- generoitujen reseptien ja viikkoruokalistojen tallennus muistiinpanoihin

Palvelinpuolen kontrollit:

- erillinen FastAPI-backend
- Firebase App Check -tarkistus
- Google Play Integrity -tarkistus nonce-polulla
- `X-Client-Id`-pohjainen väärinkäytön torjunta
- toimintokohtaiset päivä- ja burst-rajat MariaDB:ssä
- whitelist-tuki sisäiseen testaukseen ja tuleviin premium-tasoihin
- AI-vastausten palvelinpuolinen validointi ja siistiminen ennen Androidille
  palauttamista

Ratkaisu pitää AI-toiminnot valinnaisina, kustannustietoisina ja suojattuina
ilman client-puolen luottamusoletusta.

## Jaetut listat

Jaetut listat ovat erillinen reaaliaikainen yhteistyökerros ostos- ja
tehtävälistoille. Ne eivät korvaa paikallisia offline-first Room-listoja.

Toteutettuja yhteistyöominaisuuksia ovat:

- valinnainen Firebase Auth / Google Sign-In -identiteetti
- jaetut ostos- ja tehtävälistat
- kutsulinkit HTTPS App Links -polulla ja GitHub Pages -fallbackilla
- Firestore-pohjainen reaaliaikainen rivisynkronointi
- owner/editor/viewer-roolit
- owner-only-kutsut, uudelleennimeäminen, poisto, cleanup, arkistointi ja palautus
- editorin lisäys-, muokkaus-, toggle- ja aktiivisten rivien järjestystoiminnot
- viewer-roolin read-only-käyttö
- jaettujen listojen yleisnäkymä jäsenmäärällä, avatar-previewllä, rivimäärällä
  ja käyttäjäkohtaisella järjestyksellä
- FCM-push-ilmoitukset käyttäjäkohtaisilla ilmoitusasetuksilla ja debounce-logiikalla
- backup schema v5:n pilviviitteet ilman jaettujen listojen koko sisällön vientiä

## Nykyinen ominaisuustaso

- Paikalliset kauppalistat, suosikit, tehtävät, muistiinpanot,
  muistiinpanokansiot, reseptit, viikkoruokalistat, syntymäpäivät, muistutukset
  ja terveystiedot.
- Room-tietokannan skeemaversio 23 ja backup schema v5.
- Henkilökohtainen terveystietojen seuranta verenpaineelle, painolle,
  pituudelle, pulssille ja omille mittareille ilman lääketieteellistä tulkintaa
  tai terveysneuvontaa.
- Monikielinen käyttöliittymä 14 lokaalille: suomi, englanti, saksa, espanja,
  ranska, italia, japani, ruotsi, norja, tanska, unkari, viro, ukraina ja kiina.
- Google Play -tuotantojulkaisuprosessi allekirjoitetuilla Android App Bundleilla.
- Sovelluksen oma arvostelukehote, Play In-App Review ja Play Store -fallback.
- Google Play flexible In-App Updates.
- FastAPI AI -backend App Check-, Play Integrity-, OpenAI- ja MariaDB rate
  limiting -integraatioilla.
- Firebase-pohjainen shared-list-yhteistyö Firestorella, Cloud Functionsilla,
  FCM:llä, Firestore rules -mallilla ja App Linkseillä.

## Tuotantotilanne

- Julkaistu Google Playn tuotantokanavassa.
- Privacy-first paikallinen ydinkäyttö sekä valinnaiset online-AI- ja
  yhteistyökerrokset.
- 14-kielinen käyttöliittymä.
- Kevyt Compose-käyttöliittymä, jossa vanhemmat Android-versiot ja pienet näytöt
  on huomioitu.
- Tuotantotason backend-turvallisuus ja palvelinpuolinen AI-käytön rajoitus.
- Reaaliaikainen jaettujen listojen arkkitehtuuri toteutettu Firebasella.

## Lähdekoodihuomautus

Tämä repositorio ylläpidetään portfolio- ja arkkitehtuuriesittelynä. Se ei
julkaise koko yksityistä tuotantolähdekoodia, omistettua liiketoimintalogiikkaa,
julkaisuavaimia tai tuotantosalaisuuksia.

Arkkitehtuurikuvauksia ja valikoituja pseudokoodiesimerkkejä voidaan tarjota
teknistä arviointia varten.

© 2026 Janstech

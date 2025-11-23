# Instalacja i konfiguracja Material Design 3 Dynamic Mobile Dashboard

Poniższy poradnik przeprowadzi Cię przez proces instalacji oraz konfiguracji repozytorium **Material Design 3 Dynamic Mobile Dashboard** dla Home Assistant. Możesz go wkleić bezpośrednio na GitHub jako plik README.md.

---

## 📌 Wymagania

Aby dashboard działał poprawnie, potrzebujesz Home Assistant z dostępem do **HACS (Home Assistant Community Store)**. W tym poradniku instalujemy wszystko tylko przez **HACS** — bez ręcznego kopiowania plików!

### 📦 Czy potrzebne jest **Material You Utilities?**

Tak — większość dashboardów MD3 (Material Design 3) wymaga **Material You Utilities**, ponieważ zawiera ono:

* szablony kolorów
* zmienne Material You
* funkcje dynamicznych motywów
* style używane przez wszystkie karty MD3

Dlatego **MUSISZ** je zainstalować z HACS, zanim dashboard zadziała poprawnie.

### Jak zainstalować Material You Utilities przez HACS

1. Wejdź w **HACS → Frontend → Explore & Download**
2. Wpisz:

```
Material You Utilities
```

3. Kliknij **Download** → **Install**
4. Po instalacji wykonaj restart Home Assistant

Po tym możesz instalować sam dashboard.
Aby dashboard działał poprawnie, potrzebujesz Home Assistant z dostępem do **HACS (Home Assistant Community Store)**. W tym poradniku instalujemy wszystko tylko przez **HACS** — bez ręcznego kopiowania plików!
Aby dashboard działał poprawnie, potrzebujesz:

### ✅ 1. Home Assistant

* Wersja **2023.5 lub nowsza**.
* Może być instalacja: Supervised, Container (Docker), TrueNAS, Proxmox, HAOS.

### ✅ 2. HACS (Home Assistant Community Store)

Potrzebny do instalacji dodatkowych kart i integracji.

Jeżeli nie masz HACS — zainstaluj go:

1. Wejdź na: [https://hacs.xyz](https://hacs.xyz)
2. Pobierz instalator i wykonaj instrukcję podaną na stronie.

### ✅ 3. Custom Cards wymagane przez dashboard

Zainstaluj w HACS → Frontend:

* **button-card**
* **layout-card**
* **card-mod**
* **stack-in-card** (jeśli wymaga tego konkretna sekcja dashboardu)

---

## 📌 Instalacja Dashboardu przez HACS

W tym poradniku instalujemy dashboard **wyłącznie przez HACS**, bez ręcznego kopiowania plików.

### 1. Otwórz HACS

W Home Assistant wejdź w:
**HACS → Frontend → Explore & Download Repositories**

### 2. Wyszukaj repozytorium

Wpisz:

```
Material Design 3 Dynamic Mobile Dashboard
```

Jeśli repo nie jest widoczne — dodaj je ręcznie jako repozytorium niestandardowe.

### 3. Dodaj repo jako „Custom Repository”

1. W HACS kliknij w prawym górnym rogu **⋮ (menu)**
2. Wybierz **Custom repositories**
3. Wpisz adres repozytorium:

```
https://github.com/ElementZoom/Material-Design-3-Dynamic-Mobile-Dashboard
```

4. Wybierz kategorię: **Dashboard**
5. Kliknij **Add**

### 4. Zainstaluj repozytorium

Po dodaniu repo widzisz przycisk **Download → Instaluj**.

### 5. Włącz dashboard w HA

Po instalacji pojawi się komunikat typu:

* „This dashboard contains a Lovelace resource” → kliknij **Add resource**

Następnie panel dodasz w:
**Ustawienia → Panele → Dodaj panel → Dashboard → wybierz MD3 Dashboard**

---

## 📌 Dodanie dashboardu do Home Assistant

1. W Home Assistant wejdź w:
   **Ustawienia → Panele → Dodaj panel → Dashboard od pliku YAML**

2. Wskaż plik:

```
/config/dashboards/Material-Design-3-Dynamic-Mobile-Dashboard/dashboard.yaml
```

3. Nadaj nazwę, np.:

```
Material You Mobile
```

4. Zapisz i odśwież stronę.

---

## 📌 Dodanie motywów Material You

Dashboard MD3 obsługuje motywy Material You, ale zanim się pojawią w ustawieniach, musisz je dodać do Home Assistant.

### 📁 1. Instalacja motywów przez HACS

1. Wejdź w **HACS → Frontend → Explore & Download**
2. Wyszukaj:

```
Material You Themes
```

3. Kliknij **Download** i zainstaluj.

Po instalacji w folderze pojawią się różne motywy, np.:

* Material You Light
* Material You Dark
* Material You Dynamic
* Material You AMOLED

### ⚙️ 2. Włączenie obsługi motywów w configuration.yaml

Aby motywy pojawiły się na liście do wyboru, dodaj w `configuration.yaml`:

```yaml
frontend:
  themes: !include_dir_merge_named themes
```

Jeśli masz już sekcję `frontend:`, dodaj tylko linię `themes:`.

Po zapisaniu zmian **zrestartuj Home Assistant**.

### 🎨 3. Aktywacja motywu w Home Assistant

Po restarcie:

1. Wejdź w **Ustawienia → Osobiste → Motyw**
2. Wybierz motyw, np.:

   * Material You Dynamic
   * Material You Light
   * Material You Dark
   * Material You AMOLED

### 🖌️ 4. Ustawienia automatycznego trybu dzień/noc (opcjonalnie)

Dodaj do `configuration.yaml`, aby motyw zmieniał się automatycznie:

```yaml
frontend:
  themes: !include_dir_merge_named themes
  default_theme: Material You Light
  dark_mode: true
  dark_theme: Material You Dark
```

### 📌 Tip

Jeśli zainstalujesz **Material You Utilities**, dashboard automatycznie dopasuje kolory kart i elementów UI do wybranego motywu.

Repozytorium zawiera motywy dynamiczne. Skopiuj folder:

```
themes/material-you-dynamic
```

do:

```
/config/themes/material-you-dynamic
```

W `configuration.yaml` dodaj:

```yaml
theme: !include_dir_merge_named themes
```

Po restarcie systemu wybierz motyw:

* Ustawienia → Osobiste → Motyw → Material You Dynamic

---

## 📌 Ważne! Konfiguracja animowanego tła

Aby działały animowane tła, w `dashboard.yaml` musi być sekcja:

```yaml
animated_background:
  default_url: https://cdn.flixel.com/flixel/ypy8bw9fgw1zv2b4htp2.hd.mp4
  entity: weather.forecast_dom
```

Jeśli tło jest czarne — sprawdź:

* czy `entity:` istnieje u Ciebie
* czy Flixel (lub inne źródło wideo) nie blokuje linku

Możesz użyć dowolnego mp4 z HTTPS.

---

## 📌 Przykład użycia button-card (sekcja Lights)

Poniżej przykładowy kod, który możesz wykorzystać w swoim dashboardzie:

```yaml
type: button
entity: light.lsc_moodlight
icon: mdi:lightbulb
show_name: true
template: lights_rgb
color: primary
theme: Material You
show_state: true
```

---

## 📌 Aktualizacja dashboardu

Aby zaktualizować do nowszej wersji:

```
git pull
```

albo usuń stary folder i wgraj nowy.

---

## 📌 Pełna konfiguracja Material You Utilities i motywów

### 🔧 1. Czy trzeba używać `panel_custom`?

W większości przypadków **NIE** — Material You Utilities instalowane z HACS same dodają panel boczny.

Użyj `panel_custom` tylko wtedy, gdy panel się NIE pojawia.

#### Konfiguracja (opcjonalna):

```yaml
panel_custom:
  - name: material-you-panel
    url_path: material-you-configuration
    sidebar_title: Material You Utilities
    sidebar_icon: mdi:material-design
    module_url: /hacsfiles/material-you-utilities/material-you-utilities.min.js
```

---

### 🎨 2. Włączenie obsługi motywów — wymagane

W `configuration.yaml` dodaj:

```yaml
frontend:
  themes: !include_dir_merge_named themes
```

To sprawia, że wszystkie motywy z HACS będą dostępne.

Upewnij się, że folder istnieje:

```
/config/themes
```

Jeśli nie — utwórz go.

---

### 🖌️ 3. Instalacja motywów Material You przez HACS

1. Wejdź w **HACS → Frontend → Explore & Download**
2. Wyszukaj:

```
Material You Themes
```

3. Zainstaluj → Restartuj Home Assistant

Motywy pojawią się w:
**Ustawienia → Osobiste → Motyw**

---

### 🌈 4. Dodawanie własnych motywów (przykład happy/sad)

Jeśli chcesz dodać swoje motywy:

Stwórz plik:

```
/config/themes/custom/happy.yaml
```

Dodaj do niego:

```yaml
happy:
  primary-color: pink
  accent-color: orange
```

Drugi motyw:

```
/config/themes/custom/sad.yaml
```

```yaml
sad:
  primary-color: steelblue
  accent-color: darkred
```

Po restarcie oba motywy pojawią się na liście.

---

### 🌙 5. Automatyczna zmiana motywu dzień/noc

Jeśli chcesz, by motyw zmieniał się automatycznie:

```yaml
frontend:
  themes: !include_dir_merge_named themes
  default_theme: Material You Light
  dark_mode: true
  dark_theme: Material You Dark
```

---


## 📌 Licencja

Dashboard udostępniony zgodnie z licencją repozytorium autora.

link do orginalnego posta autora : https://github.com/ElementZoom/Material-Design-3-Dynamic-Mobile-Dashboard
---



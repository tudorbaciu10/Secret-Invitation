# Dayt — Invitație Romantică Digitală

O aplicație web single-page pentru a trimite o invitație la o cină romantică, cu animații, confetti, muzică ambientală și confirmare prin email.

---

## Flux utilizator

```
[Loading] → [Welcome] → [Întrebare haioasă] → [Mesaj romantic] → [Propunere] → [Formular dată] → [Ticket digital] → [Mesaj final + email]
```

7 ecrane conectate prin tranziții fade. Progress dots în header indică poziția curentă.

---

## Funcționalități

| Funcție | Detalii |
|---|---|
| **Loading screen** | Animație heartbeat + bară de progres, 2.9s |
| **Ecran 2 – Întrebare** | Ambele butoane conduc la DA (nu există opțiune de refuz) |
| **Ecran 3 – Typing** | Text scris caracter cu caracter (~42ms/char + jitter) |
| **Ecran 4 – Propunere** | Butonul „Nu" fuge la hover/touch, se micșorează treptat, dispare după 9 încercări |
| **Ecran 5 – Formular** | Data + ora (required) + notă (opțional) |
| **Ecran 6 – Ticket** | Cod unic `DATE-XXXXXX`, QR code colorat (roz) via qrcodejs |
| **Ecran 7 – Final** | Trimite email cu datele întâlnirii via EmailJS |
| **Inimi flotante** | Background animat, densitate adaptată mobile/desktop |
| **Inimi la click** | Spawn pe orice click în afara butoanelor |
| **Confetti** | Canvas animat la confirmarea datei și la trimiterea emailului |
| **Muzică** | Melodie ambientală sinusoidală via Web Audio API, loop |
| **Dark mode** | Toggle manual + detectare automată `prefers-color-scheme` |
| **Easter egg** | 5 clickuri pe titlul de pe ecranul 1 → modal secret |

---

## Configurare EmailJS

### Pași de setup

1. Cont gratuit pe [emailjs.com](https://www.emailjs.com)
2. **Email Services** → Add New Service → Gmail/Outlook → copiază `Service ID`
3. **Email Templates** → Create New Template cu structura:

```
Subiect: 🎉 Răspuns invitație - {{cod}}

Data:  {{data}}
Ora:   {{ora}}
Notă:  {{nota}}
Cod:   {{cod}}
To Email: {{to_email}}
```

4. **Account → General** → copiază `Public Key`
5. Înlocuiește în `index.html` (secțiunea CONFIG, sus în `<script>`):

```js
const EJS_PUBLIC_KEY  = 'cheia_ta_publica';
const EJS_SERVICE_ID  = 'service_xxxxxxx';
const EJS_TEMPLATE_ID = 'template_xxxxxxx';
const RECIPIENT_EMAIL = 'emailul_tau@gmail.com';
```

### Valori curente (commit inițial)

```js
EJS_PUBLIC_KEY  = 'CbnYGvZz_5AWYSaj1'
EJS_SERVICE_ID  = 'service_52vcnsl'
EJS_TEMPLATE_ID = 'template_iwqddre'
RECIPIENT_EMAIL = 'your@email.com'   // ← de înlocuit
```

---

## Structură tehnică

```
index.html          (1156 linii — tot proiectul într-un singur fișier)
├── <head>
│   ├── Google Fonts: Dancing Script + Poppins
│   ├── qrcodejs (CDN) — generare QR code
│   ├── EmailJS browser SDK v4 (CDN)
│   └── <style> (~400 linii CSS + animații)
└── <body>
    ├── #bg          — gradient background fix
    ├── #hearts      — container inimi flotante
    ├── #confetti    — canvas animație
    ├── #dots        — progress indicator (7 puncte)
    ├── .fab × 2     — butoane muzică + dark mode
    ├── #easter      — modal easter egg
    ├── #s0 – #s7    — cele 8 ecrane
    └── <script>     — toată logica JS
```

### Dependențe externe (CDN)

| Librărie | Versiune | Scop |
|---|---|---|
| qrcodejs | 1.0.0 | Generare QR code pe ticket |
| @emailjs/browser | 4.x | Trimitere email fără backend |
| Google Fonts | — | Dancing Script, Poppins |

---

## Design system

```css
--lp:   #FFB6C1   /* light pink */
--rp:   #FF6B81   /* rose pink (accent principal) */
--spbg: #FCE4EC   /* background light */
--lav:  #E6E6FA   /* lavender */
--td:   #4a2040   /* text dark */
--tm:   #7a4060   /* text medium */
--cbg:  rgba(255,255,255,0.82)  /* card background (glassmorphism) */
```

Card-urile folosesc `backdrop-filter: blur(20px)` + border semi-transparent (glassmorphism).

---

## Responsive

| Breakpoint | Ajustări |
|---|---|
| `≤ 520px` | Padding card redus, gif mai mic, grid form pe o coloană |
| `≤ 380px` | Padding minim, font mai mic (Galaxy A, iPhone SE) |
| `height ≤ 680px` | Landscape phones — padding și mărimi reduse |

Input-urile au `font-size: 1rem` explicit pentru a preveni zoom-ul automat iOS Safari.

---

## Git

```bash
# Repository inițializat și commit inițial creat cu:
cd "c:\Users\btudo\Documents\GitHub\dayt"
git init
git add index.html
git commit -m "add emailjs integration with correct template ID"
# commit: 818ab23
```

---

## Deploy

Proiectul este un singur fișier HTML static — poate fi găzduit pe:

- **GitHub Pages** — push pe branch `gh-pages` sau din `main`
- **Netlify / Vercel** — drag & drop al folderului
- **Orice hosting static** — upload direct `index.html`

Nu necesită build step, server, sau bază de date.

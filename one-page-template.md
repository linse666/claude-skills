# One-Page-Website Vorlage — Anweisung für Claude Code

> Diese Datei wird von Claude Code per Slash Command `/one-page` geladen.
> Sie enthält alle Anweisungen und Vorlagen für die Erstellung einer
> professionellen, statischen One-Page-Website.

---

## 1. Erst fragen, dann bauen

Bevor du irgendeine Datei erstellst, stelle dem Nutzer folgende Fragen
und warte auf die Antworten:

```
Ich erstelle dir eine One-Page-Website. Dazu benötige ich ein paar Angaben:

1. **Projektname / Domain** — Wie heißt das Projekt und die geplante Domain?
2. **Unternehmen / Person** — Name, Ort, Branche, kurze Beschreibung
3. **Zielgruppe** — Für wen ist die Seite? (z.B. Privatkunden, Unternehmen, Entwickler)
4. **Sektionen** — Welche brauchst du?
   - [ ] Hero (Pflicht)
   - [ ] Über uns / About
   - [ ] Leistungen / Services
   - [ ] Preise / Pricing
   - [ ] Referenzen / Portfolio
   - [ ] Blog-Vorschau (benötigt PHP-Backend)
   - [ ] Kontaktformular (benötigt PHP-Backend)
5. **Theme** — Dark, Hell, oder beide (mit Umschalter)?
6. **Primärfarbe** — Welche Akzentfarbe? (z.B. Blau, Grün, Lila — oder Hex-Code)
7. **Kontaktdaten** — E-Mail, Telefon, Adresse, Öffnungszeiten (für Schema.org)
```

Nutze die Antworten um alle Platzhalter in den folgenden Vorlagen
zu befüllen.

---

## 2. Dateistruktur erstellen

Lege folgende Struktur an (nur Sektionen die der Nutzer gewählt hat):

```
projektname/
├── index.html
├── impressum.html          # Pflicht in Deutschland
├── datenschutz.html        # Pflicht in Deutschland
├── mail.php                # Nur wenn Kontaktformular gewählt
├── .htaccess
├── robots.txt
├── sitemap.xml
│
├── css/
│   ├── style.css
│   └── fonts/
│       └── fonts.css
│
├── js/
│   └── main.js
│
└── assets/
    └── images/
        ├── hero/
        ├── sections/
        └── favicon/
```

---

## 3. Vorlage: `index.html`

```html
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- SEO -->
  <title>[PROJEKTNAME] — [KURZBESCHREIBUNG]</title>
  <meta name="description" content="[BESCHREIBUNG max. 160 Zeichen]">
  <meta name="keywords" content="[KEYWORD1], [KEYWORD2], [ORT], [BRANCHE]">
  <meta name="author" content="[NAME]">
  <link rel="canonical" href="https://[DOMAIN]/">

  <!-- Open Graph -->
  <meta property="og:type"        content="website">
  <meta property="og:url"         content="https://[DOMAIN]/">
  <meta property="og:title"       content="[PROJEKTNAME] — [KURZBESCHREIBUNG]">
  <meta property="og:description" content="[BESCHREIBUNG]">
  <meta property="og:image"       content="https://[DOMAIN]/assets/images/og-image.jpg">
  <meta property="og:locale"      content="de_DE">

  <!-- Twitter Card -->
  <meta name="twitter:card"        content="summary_large_image">
  <meta name="twitter:title"       content="[PROJEKTNAME] — [KURZBESCHREIBUNG]">
  <meta name="twitter:description" content="[BESCHREIBUNG]">
  <meta name="twitter:image"       content="https://[DOMAIN]/assets/images/og-image.jpg">

  <!-- Schema.org LocalBusiness -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "LocalBusiness",
    "name": "[UNTERNEHMENSNAME]",
    "url": "https://[DOMAIN]",
    "telephone": "[TELEFON]",
    "email": "[EMAIL-ROT13-DECODED]",
    "address": {
      "@type": "PostalAddress",
      "streetAddress": "[STRASSE]",
      "addressLocality": "[ORT]",
      "postalCode": "[PLZ]",
      "addressCountry": "DE"
    },
    "geo": {
      "@type": "GeoCoordinates",
      "latitude": [LAT],
      "longitude": [LNG]
    },
    "openingHoursSpecification": [{
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Monday","Tuesday","Wednesday","Thursday","Friday"],
      "opens": "09:00",
      "closes": "17:00"
    }],
    "areaServed": "[REGION]"
  }
  </script>

  <!-- Favicon -->
  <link rel="icon"             href="/assets/images/favicon/favicon.ico">
  <link rel="icon"             href="/assets/images/favicon/favicon.svg" type="image/svg+xml">
  <link rel="apple-touch-icon" href="/assets/images/favicon/apple-touch-icon.png">

  <!-- Preload: LCP-Bild + primärer Font -->
  <link rel="preload" as="image" href="/assets/images/hero/hero.webp">
  <link rel="preload" as="font"  href="/css/fonts/[FONT].woff2" type="font/woff2" crossorigin>

  <!-- CSS non-blocking -->
  <link rel="preload" as="style" href="/css/style.css"
        onload="this.rel='stylesheet'">
  <noscript><link rel="stylesheet" href="/css/style.css"></noscript>
</head>

<body data-theme="[dark|light]">

  <!-- NAVIGATION -->
  <header id="site-header">
    <nav class="nav container" aria-label="Hauptnavigation">
      <a href="/" class="nav__logo" aria-label="Startseite">
        <img src="/assets/images/favicon/logo.webp" width="48" height="48"
             alt="Logo [PROJEKTNAME]" loading="eager">
        <span>[PROJEKTNAME]</span>
      </a>
      <ul class="nav__links" role="list">
        <!-- Nur Sektionen einbauen die der Nutzer gewählt hat -->
        <li><a href="#about">Über uns</a></li>
        <li><a href="#services">Leistungen</a></li>
        <li><a href="#pricing">Preise</a></li>
        <li><a href="#referenzen">Referenzen</a></li>
        <li><a href="#contact" class="btn btn--primary">Kontakt</a></li>
      </ul>
      <!-- Theme-Toggle nur wenn "beide Themes" gewählt -->
      <button class="nav__theme-toggle" id="theme-toggle"
              aria-label="Theme wechseln" aria-pressed="false">
        <i data-lucide="sun"  class="icon-light" aria-hidden="true"></i>
        <i data-lucide="moon" class="icon-dark"  aria-hidden="true"></i>
      </button>
      <button class="nav__hamburger" id="hamburger"
              aria-label="Menü öffnen" aria-expanded="false">
        <i data-lucide="menu" aria-hidden="true"></i>
      </button>
    </nav>
    <div id="mobile-menu" class="mobile-menu" hidden>
      <ul role="list">
        <li><a href="#about">Über uns</a></li>
        <li><a href="#services">Leistungen</a></li>
        <li><a href="#pricing">Preise</a></li>
        <li><a href="#referenzen">Referenzen</a></li>
        <li><a href="#contact">Kontakt</a></li>
      </ul>
    </div>
  </header>

  <main>

    <!-- HERO (Pflicht) -->
    <section id="hero" class="hero">
      <div class="container hero__inner">
        <div class="hero__text">
          <div class="hero__badges">
            <span class="badge">[TAG1]</span>
            <span class="badge">[TAG2]</span>
          </div>
          <h1 class="hero__headline">
            [HAUPTÜBERSCHRIFT]<br>
            <span class="highlight">[HIGHLIGHT-WORT]</span>
          </h1>
          <p class="hero__sub">[SUBLINE — 1-2 Sätze]</p>
          <div class="hero__ctas">
            <a href="#contact"  class="btn btn--primary">Jetzt anfragen</a>
            <a href="#services" class="btn btn--ghost">Mehr erfahren</a>
          </div>
        </div>
        <div class="hero__image">
          <img src="/assets/images/hero/hero.webp"
               srcset="/assets/images/hero/hero-640.webp  640w,
                       /assets/images/hero/hero-960.webp  960w,
                       /assets/images/hero/hero-1280.webp 1280w"
               sizes="(max-width: 768px) 100vw, 50vw"
               width="640" height="480"
               alt="[BILDBESCHREIBUNG]"
               loading="eager" decoding="async">
        </div>
      </div>
    </section>

    <!-- ABOUT (optional) -->
    <section id="about" class="about section">
      <div class="container">
        <div class="section__header reveal">
          <h2>Über uns</h2>
          <p class="section__sub">[UNTERZEILE]</p>
        </div>
        <div class="about__inner">
          <div class="about__image reveal">
            <img src="/assets/images/sections/portrait.webp"
                 srcset="/assets/images/sections/portrait-320.webp 320w,
                         /assets/images/sections/portrait-640.webp 640w"
                 sizes="(max-width: 768px) 280px, 360px"
                 width="360" height="360"
                 alt="Portrait [NAME]" loading="lazy">
          </div>
          <div class="about__text reveal">
            <h3>[NAME]</h3>
            <p>[ABOUT-TEXT ABSATZ 1]</p>
            <p>[ABOUT-TEXT ABSATZ 2]</p>
            <div class="pillars">
              <div class="pillar">
                <i data-lucide="[ICON]" aria-hidden="true"></i>
                <strong>[WERT1]</strong>
                <span>[KURZERKLÄRUNG]</span>
              </div>
              <div class="pillar">
                <i data-lucide="[ICON]" aria-hidden="true"></i>
                <strong>[WERT2]</strong>
                <span>[KURZERKLÄRUNG]</span>
              </div>
              <div class="pillar">
                <i data-lucide="[ICON]" aria-hidden="true"></i>
                <strong>[WERT3]</strong>
                <span>[KURZERKLÄRUNG]</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- SERVICES (optional) -->
    <section id="services" class="services section">
      <div class="container">
        <div class="section__header reveal">
          <h2>Leistungen</h2>
          <p class="section__sub">[UNTERZEILE]</p>
        </div>
        <div class="services__grid">
          <!-- Für jede Leistung eine Card — Anzahl mit Nutzer abstimmen -->
          <article class="card reveal" data-modal="service-1">
            <div class="card__icon">
              <i data-lucide="[ICON]" aria-hidden="true"></i>
            </div>
            <h3 class="card__title">[LEISTUNG 1]</h3>
            <p class="card__text">[KURZBESCHREIBUNG]</p>
            <button class="card__more">
              Details <i data-lucide="arrow-right" aria-hidden="true"></i>
            </button>
          </article>
          <!-- Weitere Cards nach gleichem Muster -->
        </div>
      </div>
    </section>

    <!-- Modal für Services -->
    <div id="service-modal" class="modal" hidden role="dialog" aria-modal="true">
      <div class="modal__backdrop"></div>
      <div class="modal__box">
        <button class="modal__close" aria-label="Schließen">
          <i data-lucide="x" aria-hidden="true"></i>
        </button>
        <div class="modal__content"></div>
      </div>
    </div>

    <!-- Modal-Daten als Templates -->
    <template id="modal-data-service-1">
      <h2>[LEISTUNG 1 — AUSFÜHRLICHER TITEL]</h2>
      <p>[AUSFÜHRLICHE BESCHREIBUNG]</p>
      <ul>
        <li>[DETAIL A]</li>
        <li>[DETAIL B]</li>
      </ul>
      <a href="#contact" class="btn btn--primary">Jetzt anfragen</a>
    </template>

    <!-- PRICING (optional) -->
    <section id="pricing" class="pricing section">
      <div class="container">
        <div class="section__header reveal">
          <h2>Preise</h2>
          <p class="section__sub">Transparent und fair.</p>
        </div>
        <div class="pricing__toggle reveal">
          <span>Monatlich</span>
          <button class="toggle" id="pricing-toggle"
                  role="switch" aria-checked="false">
            <span class="toggle__knob"></span>
          </button>
          <span>Jährlich <em class="badge badge--green">2 Monate gratis</em></span>
        </div>
        <div class="pricing__grid">
          <!-- Für jedes Paket eine Card -->
          <div class="pricing-card reveal">
            <div class="pricing-card__header">
              <h3>[PAKETNAME]</h3>
              <div class="pricing-card__price">
                <span class="price"
                      data-monthly="[PREIS]"
                      data-yearly="[JAHRESPREIS]">[PREIS]</span>
                <span class="price__unit">€/Monat</span>
              </div>
            </div>
            <ul class="pricing-card__features" role="list">
              <li><i data-lucide="check" aria-hidden="true"></i> [FEATURE]</li>
            </ul>
            <a href="#contact" class="btn btn--ghost btn--full">Anfragen</a>
          </div>
          <!-- Empfohlene Card mit pricing-card--featured Klasse -->
        </div>
      </div>
    </section>

    <!-- REFERENZEN (optional) -->
    <section id="referenzen" class="referenzen section">
      <div class="container">
        <div class="section__header reveal">
          <h2>Referenzen</h2>
        </div>
        <div class="referenzen__grid">
          <article class="ref-card reveal">
            <a href="[URL]" target="_blank" rel="noopener noreferrer">
              <img src="/assets/images/sections/ref-1.webp"
                   width="600" height="400"
                   alt="Screenshot [PROJEKTNAME]" loading="lazy">
              <div class="ref-card__overlay">
                <h3>[PROJEKTNAME]</h3>
                <p>[KURZBESCHREIBUNG]</p>
                <span class="ref-card__link">
                  Besuchen <i data-lucide="external-link" aria-hidden="true"></i>
                </span>
              </div>
            </a>
          </article>
        </div>
      </div>
    </section>

    <!-- CTA-SECTION -->
    <section id="cta-section" class="cta-section section">
      <canvas id="particle-canvas" aria-hidden="true"></canvas>
      <div class="container cta-section__inner reveal">
        <h2>[CTA-HEADLINE]</h2>
        <p>[CTA-SUBLINE]</p>
        <a href="#contact" class="btn btn--primary btn--large">
          Jetzt Kontakt aufnehmen
        </a>
      </div>
    </section>

    <!-- KONTAKT (optional, benötigt mail.php) -->
    <section id="contact" class="contact section">
      <div class="container">
        <div class="section__header reveal">
          <h2>Kontakt</h2>
          <p class="section__sub">[UNTERZEILE]</p>
        </div>
        <div class="contact__inner">
          <div class="contact__info reveal">
            <p><i data-lucide="mail"     aria-hidden="true"></i>
               <a href="mailto:[EMAIL]" id="contact-email">[EMAIL-ROT13]</a></p>
            <p><i data-lucide="phone"    aria-hidden="true"></i> [TELEFON]</p>
            <p><i data-lucide="map-pin"  aria-hidden="true"></i> [ORT]</p>
          </div>
          <form class="contact__form reveal" id="contact-form" novalidate>
            <!-- Honeypot -->
            <input type="text" name="website" tabindex="-1"
                   autocomplete="off" aria-hidden="true">
            <div class="form__group">
              <label for="name">Name *</label>
              <input type="text" id="name" name="name"
                     required minlength="2" autocomplete="name">
            </div>
            <div class="form__group">
              <label for="email">E-Mail *</label>
              <input type="email" id="email" name="email"
                     required autocomplete="email">
            </div>
            <div class="form__group">
              <label for="message">Nachricht *</label>
              <textarea id="message" name="message"
                        required minlength="10" rows="5"></textarea>
            </div>
            <button type="submit" class="btn btn--primary">
              Nachricht senden
              <i data-lucide="send" aria-hidden="true"></i>
            </button>
            <div id="form-status" role="alert" aria-live="polite"></div>
          </form>
        </div>
      </div>
    </section>

  </main>

  <!-- FOOTER -->
  <footer class="footer">
    <div class="container footer__inner">
      <p>&copy; <span id="footer-year"></span> [NAME]. Alle Rechte vorbehalten.</p>
      <nav aria-label="Footer-Navigation">
        <a href="/impressum.html">Impressum</a>
        <a href="/datenschutz.html">Datenschutz</a>
      </nav>
    </div>
  </footer>

  <!-- Lucide Icons (self-hosted: js/lucide.min.js herunterladen) -->
  <!-- Download: https://unpkg.com/lucide@latest/dist/umd/lucide.min.js -->
  <script src="/js/lucide.min.js"></script>
  <script src="/js/main.js" defer></script>

</body>
</html>
```

---

## 4. Vorlage: `css/style.css`

```css
/* ═══════════════════════════════════════════════════════
   DESIGN-SYSTEM — CSS Custom Properties
   Alle Farben, Abstände und Radien hier definieren.
   Nie Farbwerte direkt in Komponenten schreiben.
════════════════════════════════════════════════════════ */

/* Dark Theme (Standard) */
[data-theme="dark"] {
  --primary:      [PRIMÄRFARBE];         /* z.B. #6366f1 */
  --primary-dark: [PRIMÄRFARBE DUNKLER]; /* z.B. #4f46e5 */

  --bg:           #0f1117;  /* Seitenhintergrund */
  --bg2:          #1a1d27;  /* Cards, Panels */
  --bg3:          #242736;  /* Hover, Input-Hintergrund */

  --text:         #c8cfe0;  /* Fließtext */
  --text-dim:     #7a8099;  /* Metadaten, Untertitel */
  --head:         #eef4ff;  /* Überschriften */

  --border:       rgba(255,255,255,0.08);
  --shadow:       0 4px 24px rgba(0,0,0,0.4);
}

/* Light Theme */
[data-theme="light"] {
  --primary:      [PRIMÄRFARBE];
  --primary-dark: [PRIMÄRFARBE DUNKLER];

  --bg:           #f8f9fc;
  --bg2:          #ffffff;
  --bg3:          #f0f2f7;

  --text:         #374151;
  --text-dim:     #6b7280;
  --head:         #111827;

  --border:       rgba(0,0,0,0.08);
  --shadow:       0 4px 24px rgba(0,0,0,0.08);
}

/* Gemeinsame Tokens (theme-unabhängig) */
:root {
  --radius:    0.75rem;
  --radius-sm: 0.375rem;
  --radius-lg: 1.25rem;

  --space-xs:  0.5rem;
  --space-sm:  1rem;
  --space-md:  2rem;
  --space-lg:  4rem;
  --space-xl:  8rem;

  --font-body:    '[SCHRIFT]', system-ui, sans-serif;
  --font-heading: '[SCHRIFT]', system-ui, sans-serif;
  --font-mono:    '[MONO-SCHRIFT]', monospace;

  --transition: 0.25s ease;
  --max-width:  1200px;
}

/* ═══════════════════════════════════════════════════════
   RESET & BASIS
════════════════════════════════════════════════════════ */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html {
  scroll-behavior: smooth;
  scroll-padding-top: 80px; /* Höhe der fixed Navigation */
}

body {
  font-family:      var(--font-body);
  background-color: var(--bg);
  color:            var(--text);
  line-height:      1.7;
  transition:       background-color var(--transition), color var(--transition);
}

img { display: block; max-width: 100%; height: auto; }
a   { color: var(--primary); text-decoration: none; }
ul  { list-style: none; }

/* ═══════════════════════════════════════════════════════
   FONTS — @font-face in css/fonts/fonts.css auslagern
════════════════════════════════════════════════════════ */
/* @import url('/css/fonts/fonts.css'); */

/* ═══════════════════════════════════════════════════════
   LAYOUT-HELFER
════════════════════════════════════════════════════════ */
.container {
  width: min(var(--max-width), 100% - 2 * var(--space-md));
  margin-inline: auto;
}

.section { padding-block: var(--space-xl); }

.section__header {
  text-align: center;
  margin-bottom: var(--space-lg);
}

.section__header h2 {
  font-size: clamp(1.75rem, 4vw, 2.5rem);
  color: var(--head);
  margin-bottom: var(--space-xs);
}

.section__sub {
  color: var(--text-dim);
  font-size: 1.1rem;
}

/* ═══════════════════════════════════════════════════════
   TYPOGRAFIE
════════════════════════════════════════════════════════ */
h1, h2, h3, h4 {
  font-family: var(--font-heading);
  color: var(--head);
  line-height: 1.2;
}

h1 { font-size: clamp(2rem,   5vw, 3.5rem); }
h2 { font-size: clamp(1.75rem,4vw, 2.5rem); }
h3 { font-size: clamp(1.25rem,3vw, 1.75rem); }

.highlight { color: var(--primary); }

/* ═══════════════════════════════════════════════════════
   BUTTONS
════════════════════════════════════════════════════════ */
.btn {
  display:         inline-flex;
  align-items:     center;
  gap:             0.5rem;
  padding:         0.75rem 1.5rem;
  border-radius:   var(--radius);
  font-weight:     600;
  font-size:       1rem;
  cursor:          pointer;
  border:          2px solid transparent;
  transition:      all var(--transition);
  text-decoration: none;
}

.btn--primary {
  background: var(--primary);
  color: #fff;
}
.btn--primary:hover {
  background: var(--primary-dark);
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(0,0,0,0.2);
}

.btn--ghost {
  background: transparent;
  border-color: var(--border);
  color: var(--text);
}
.btn--ghost:hover {
  border-color: var(--primary);
  color: var(--primary);
}

.btn--large { padding: 1rem 2rem; font-size: 1.125rem; }
.btn--full  { width: 100%; justify-content: center; }

/* ═══════════════════════════════════════════════════════
   BADGES
════════════════════════════════════════════════════════ */
.badge {
  display:       inline-block;
  padding:       0.25rem 0.75rem;
  border-radius: 999px;
  font-size:     0.8rem;
  font-weight:   600;
  background:    var(--bg3);
  color:         var(--text-dim);
  border:        1px solid var(--border);
}
.badge--green { background: rgba(34,197,94,0.1); color: #22c55e; }

/* ═══════════════════════════════════════════════════════
   NAVIGATION
════════════════════════════════════════════════════════ */
#site-header {
  position:   fixed;
  top:        0;
  left:       0;
  right:      0;
  z-index:    100;
  transition: background var(--transition), box-shadow var(--transition);
}

#site-header.scrolled {
  background: var(--bg);
  box-shadow: var(--shadow);
}

.nav {
  display:     flex;
  align-items: center;
  gap:         var(--space-md);
  height:      72px;
}

.nav__logo {
  display:     flex;
  align-items: center;
  gap:         0.75rem;
  font-weight: 700;
  font-size:   1.125rem;
  color:       var(--head);
}

.nav__links {
  display:     flex;
  align-items: center;
  gap:         var(--space-md);
  margin-left: auto;
}

.nav__links a {
  color:       var(--text);
  font-weight: 500;
  transition:  color var(--transition);
}
.nav__links a:hover,
.nav__links a.active { color: var(--primary); }

.nav__theme-toggle,
.nav__hamburger {
  background: none;
  border:     none;
  cursor:     pointer;
  color:      var(--text);
  padding:    0.5rem;
  border-radius: var(--radius-sm);
  transition: color var(--transition);
}
.nav__theme-toggle:hover,
.nav__hamburger:hover { color: var(--primary); }

/* Theme-Toggle Icons */
[data-theme="dark"]  .icon-light { display: none; }
[data-theme="light"] .icon-dark  { display: none; }

/* Hamburger nur auf Mobile sichtbar */
.nav__hamburger { display: none; }

/* Mobile Menü */
.mobile-menu {
  background: var(--bg2);
  border-top: 1px solid var(--border);
  padding: var(--space-sm);
}
.mobile-menu ul { display: flex; flex-direction: column; gap: var(--space-xs); }
.mobile-menu a  { display: block; padding: 0.75rem; color: var(--text); }

/* ═══════════════════════════════════════════════════════
   HERO
════════════════════════════════════════════════════════ */
.hero {
  min-height: 100vh;
  display:    flex;
  align-items: center;
  padding-top: 72px; /* Nav-Höhe */
}

.hero__inner {
  display:         grid;
  grid-template-columns: 1fr 1fr;
  gap:             var(--space-lg);
  align-items:     center;
  padding-block:   var(--space-xl);
}

.hero__badges {
  display:   flex;
  gap:       0.5rem;
  flex-wrap: wrap;
  margin-bottom: var(--space-sm);
}

.hero__headline {
  margin-bottom: var(--space-sm);
}

.hero__sub {
  font-size:     1.1rem;
  color:         var(--text-dim);
  margin-bottom: var(--space-md);
  max-width:     50ch;
}

.hero__ctas {
  display:   flex;
  gap:       var(--space-sm);
  flex-wrap: wrap;
}

.hero__image img {
  border-radius: var(--radius-lg);
  box-shadow:    var(--shadow);
}

/* ═══════════════════════════════════════════════════════
   ABOUT
════════════════════════════════════════════════════════ */
.about__inner {
  display:         grid;
  grid-template-columns: 1fr 2fr;
  gap:             var(--space-lg);
  align-items:     start;
}

.about__image img {
  border-radius: var(--radius-lg);
  width:         100%;
}

.about__text h3 { margin-bottom: var(--space-sm); }
.about__text p  { margin-bottom: var(--space-sm); color: var(--text-dim); }

.pillars {
  display:               grid;
  grid-template-columns: repeat(3, 1fr);
  gap:                   var(--space-sm);
  margin-top:            var(--space-md);
}

.pillar {
  display:       flex;
  flex-direction: column;
  gap:           0.25rem;
  padding:       var(--space-sm);
  background:    var(--bg2);
  border-radius: var(--radius);
  border:        1px solid var(--border);
}

.pillar svg   { color: var(--primary); width: 1.5rem; height: 1.5rem; }
.pillar strong { color: var(--head); font-size: 0.9rem; }
.pillar span   { color: var(--text-dim); font-size: 0.8rem; }

/* ═══════════════════════════════════════════════════════
   SERVICES
════════════════════════════════════════════════════════ */
.services__grid {
  display:               grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap:                   var(--space-md);
}

.card {
  background:    var(--bg2);
  border:        1px solid var(--border);
  border-radius: var(--radius-lg);
  padding:       var(--space-md);
  cursor:        pointer;
  transition:    transform var(--transition), box-shadow var(--transition),
                 border-color var(--transition);
}

.card:hover {
  transform:    translateY(-4px);
  box-shadow:   var(--shadow);
  border-color: var(--primary);
}

.card__icon {
  width:         3rem;
  height:        3rem;
  border-radius: var(--radius);
  background:    rgba(var(--primary-rgb, 99,102,241), 0.1);
  display:       flex;
  align-items:   center;
  justify-content: center;
  margin-bottom: var(--space-sm);
  color:         var(--primary);
}

.card__title { margin-bottom: 0.5rem; font-size: 1.125rem; }
.card__text  { color: var(--text-dim); margin-bottom: var(--space-sm); }

.card__more {
  background: none;
  border:     none;
  color:      var(--primary);
  cursor:     pointer;
  font-weight: 600;
  display:    flex;
  align-items: center;
  gap:        0.25rem;
  padding:    0;
  transition: gap var(--transition);
}
.card__more:hover { gap: 0.5rem; }

/* ═══════════════════════════════════════════════════════
   MODAL
════════════════════════════════════════════════════════ */
.modal {
  position: fixed;
  inset:    0;
  z-index:  200;
  display:  flex;
  align-items: center;
  justify-content: center;
  padding:  var(--space-md);
}

.modal[hidden] { display: none; }

.modal__backdrop {
  position:   absolute;
  inset:      0;
  background: rgba(0,0,0,0.6);
  backdrop-filter: blur(4px);
}

.modal__box {
  position:      relative;
  background:    var(--bg2);
  border:        1px solid var(--border);
  border-radius: var(--radius-lg);
  padding:       var(--space-md);
  max-width:     640px;
  width:         100%;
  max-height:    80vh;
  overflow-y:    auto;
}

.modal__close {
  position: absolute;
  top:      1rem;
  right:    1rem;
  background: none;
  border:   none;
  cursor:   pointer;
  color:    var(--text-dim);
  transition: color var(--transition);
}
.modal__close:hover { color: var(--primary); }

.modal__content h2  { margin-bottom: var(--space-sm); }
.modal__content p   { color: var(--text-dim); margin-bottom: var(--space-sm); }
.modal__content ul  { margin-bottom: var(--space-md); padding-left: 1.25rem; list-style: disc; }
.modal__content li  { color: var(--text-dim); margin-bottom: 0.25rem; }

/* ═══════════════════════════════════════════════════════
   PRICING
════════════════════════════════════════════════════════ */
.pricing__toggle {
  display:     flex;
  align-items: center;
  justify-content: center;
  gap:         var(--space-sm);
  margin-bottom: var(--space-lg);
}

.toggle {
  position:      relative;
  width:         3rem;
  height:        1.5rem;
  border-radius: 999px;
  background:    var(--bg3);
  border:        none;
  cursor:        pointer;
  transition:    background var(--transition);
}
.toggle[aria-checked="true"] { background: var(--primary); }

.toggle__knob {
  position:      absolute;
  top:           0.125rem;
  left:          0.125rem;
  width:         1.25rem;
  height:        1.25rem;
  border-radius: 50%;
  background:    #fff;
  transition:    transform var(--transition);
}
.toggle[aria-checked="true"] .toggle__knob { transform: translateX(1.5rem); }

.pricing__grid {
  display:               grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap:                   var(--space-md);
  align-items:           start;
}

.pricing-card {
  background:    var(--bg2);
  border:        1px solid var(--border);
  border-radius: var(--radius-lg);
  padding:       var(--space-md);
  position:      relative;
}

.pricing-card--featured {
  border-color:  var(--primary);
  transform:     scale(1.03);
  box-shadow:    var(--shadow);
}

.pricing-card__badge {
  position:      absolute;
  top:           -0.75rem;
  left:          50%;
  transform:     translateX(-50%);
  background:    var(--primary);
  color:         #fff;
  padding:       0.2rem 1rem;
  border-radius: 999px;
  font-size:     0.8rem;
  font-weight:   700;
  white-space:   nowrap;
}

.pricing-card__header { margin-bottom: var(--space-md); }
.pricing-card__header h3 { color: var(--head); margin-bottom: 0.5rem; }

.pricing-card__price {
  display:     flex;
  align-items: baseline;
  gap:         0.25rem;
}

.price { font-size: 2.5rem; font-weight: 700; color: var(--primary); }
.price__unit { color: var(--text-dim); }

.pricing-card__features {
  margin-bottom: var(--space-md);
  display:       flex;
  flex-direction: column;
  gap:           0.5rem;
}

.pricing-card__features li {
  display:     flex;
  align-items: center;
  gap:         0.5rem;
  color:       var(--text);
}

.icon--muted { color: var(--text-dim); }

/* ═══════════════════════════════════════════════════════
   REFERENZEN
════════════════════════════════════════════════════════ */
.referenzen__grid {
  display:               grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap:                   var(--space-md);
}

.ref-card {
  border-radius: var(--radius-lg);
  overflow:      hidden;
  position:      relative;
}

.ref-card a      { display: block; }
.ref-card img    { width: 100%; aspect-ratio: 3/2; object-fit: cover;
                   transition: transform 0.4s ease; }
.ref-card:hover img { transform: scale(1.05); }

.ref-card__overlay {
  position:   absolute;
  inset:      0;
  background: linear-gradient(to top, rgba(0,0,0,0.85), transparent);
  padding:    var(--space-md);
  display:    flex;
  flex-direction: column;
  justify-content: flex-end;
  opacity:    0;
  transition: opacity var(--transition);
}

.ref-card:hover .ref-card__overlay { opacity: 1; }

.ref-card__overlay h3 { color: #fff; margin-bottom: 0.25rem; }
.ref-card__overlay p  { color: rgba(255,255,255,0.8); font-size: 0.875rem;
                         margin-bottom: 0.5rem; }
.ref-card__link       { color: #fff; font-weight: 600; display: flex;
                         align-items: center; gap: 0.25rem; font-size: 0.875rem; }

/* ═══════════════════════════════════════════════════════
   CTA-SECTION
════════════════════════════════════════════════════════ */
.cta-section {
  position:   relative;
  overflow:   hidden;
  text-align: center;
  background: var(--bg2);
}

#particle-canvas {
  position: absolute;
  inset:    0;
  width:    100%;
  height:   100%;
}

.cta-section__inner {
  position:       relative;
  z-index:        1;
}

.cta-section h2 { margin-bottom: var(--space-sm); }
.cta-section p  { color: var(--text-dim); margin-bottom: var(--space-md); }

/* ═══════════════════════════════════════════════════════
   KONTAKT
════════════════════════════════════════════════════════ */
.contact__inner {
  display:               grid;
  grid-template-columns: 1fr 2fr;
  gap:                   var(--space-lg);
  align-items:           start;
}

.contact__info { display: flex; flex-direction: column; gap: var(--space-sm); }
.contact__info p {
  display:     flex;
  align-items: center;
  gap:         0.75rem;
  color:       var(--text-dim);
}

.form__group { display: flex; flex-direction: column; gap: 0.5rem; margin-bottom: var(--space-sm); }

.form__group label { font-weight: 500; color: var(--head); }

.form__group input,
.form__group textarea {
  background:    var(--bg3);
  border:        1px solid var(--border);
  border-radius: var(--radius);
  padding:       0.75rem 1rem;
  color:         var(--text);
  font-family:   var(--font-body);
  font-size:     1rem;
  transition:    border-color var(--transition);
  width:         100%;
}

.form__group input:focus,
.form__group textarea:focus {
  outline:      none;
  border-color: var(--primary);
}

/* Honeypot verstecken */
input[name="website"] {
  position: absolute;
  opacity:  0;
  height:   0;
  width:    0;
}

#form-status { margin-top: var(--space-sm); font-weight: 500; }
#form-status.success { color: #22c55e; }
#form-status.error   { color: #ef4444; }

/* ═══════════════════════════════════════════════════════
   FOOTER
════════════════════════════════════════════════════════ */
.footer {
  background:  var(--bg2);
  border-top:  1px solid var(--border);
  padding-block: var(--space-md);
}

.footer__inner {
  display:     flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap:   wrap;
  gap:         var(--space-sm);
}

.footer p { color: var(--text-dim); font-size: 0.875rem; }

.footer nav { display: flex; gap: var(--space-md); }
.footer nav a { color: var(--text-dim); font-size: 0.875rem;
                transition: color var(--transition); }
.footer nav a:hover { color: var(--primary); }

/* ═══════════════════════════════════════════════════════
   SCROLL-REVEAL ANIMATIONEN
════════════════════════════════════════════════════════ */
.reveal {
  opacity:   0;
  transform: translateY(24px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.reveal.visible {
  opacity:   1;
  transform: translateY(0);
}

/* ═══════════════════════════════════════════════════════
   RESPONSIVE — Mobile First Breakpoints
════════════════════════════════════════════════════════ */
@media (max-width: 1024px) {
  .hero__inner    { grid-template-columns: 1fr; text-align: center; }
  .hero__image    { display: none; }
  .hero__ctas     { justify-content: center; }
  .hero__sub      { margin-inline: auto; }
  .about__inner   { grid-template-columns: 1fr; }
  .pillars        { grid-template-columns: 1fr 1fr; }
  .contact__inner { grid-template-columns: 1fr; }
}

@media (max-width: 768px) {
  .nav__links     { display: none; }
  .nav__hamburger { display: flex; }
  .pillars        { grid-template-columns: 1fr; }
  .pricing-card--featured { transform: none; }
}
```

---

## 5. Vorlage: `js/main.js`

```javascript
'use strict';

/* ═══════════════════════════════════════════════════════
   LUCIDE ICONS
════════════════════════════════════════════════════════ */
document.addEventListener('DOMContentLoaded', () => {
  if (typeof lucide !== 'undefined') lucide.createIcons();
});

/* ═══════════════════════════════════════════════════════
   NAVIGATION — Scroll-Effekt + Aktiver Link
════════════════════════════════════════════════════════ */
const header = document.getElementById('site-header');

window.addEventListener('scroll', () => {
  header.classList.toggle('scrolled', window.scrollY > 20);
}, { passive: true });

// Aktiven Nav-Link per IntersectionObserver setzen
const sections = document.querySelectorAll('section[id]');
const navLinks  = document.querySelectorAll('.nav__links a[href^="#"]');

const sectionObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (!entry.isIntersecting) return;
    navLinks.forEach(link => link.classList.remove('active'));
    const active = document.querySelector(`.nav__links a[href="#${entry.target.id}"]`);
    if (active) active.classList.add('active');
  });
}, { threshold: 0.4 });

sections.forEach(s => sectionObserver.observe(s));

/* ═══════════════════════════════════════════════════════
   HAMBURGER — Mobile Menü
════════════════════════════════════════════════════════ */
const hamburger  = document.getElementById('hamburger');
const mobileMenu = document.getElementById('mobile-menu');

if (hamburger && mobileMenu) {
  hamburger.addEventListener('click', () => {
    const open = hamburger.getAttribute('aria-expanded') === 'true';
    hamburger.setAttribute('aria-expanded', String(!open));
    mobileMenu.hidden = open;
  });

  // Menü schließen wenn Link geklickt
  mobileMenu.querySelectorAll('a').forEach(link => {
    link.addEventListener('click', () => {
      hamburger.setAttribute('aria-expanded', 'false');
      mobileMenu.hidden = true;
    });
  });
}

/* ═══════════════════════════════════════════════════════
   THEME-TOGGLE — Dark / Light
   Nur einbauen wenn "beide Themes" gewählt wurde.
════════════════════════════════════════════════════════ */
const themeToggle = document.getElementById('theme-toggle');

if (themeToggle) {
  // Gespeichertes Theme laden
  const saved = localStorage.getItem('theme') || 'dark';
  document.body.setAttribute('data-theme', saved);

  themeToggle.addEventListener('click', () => {
    const current = document.body.getAttribute('data-theme');
    const next    = current === 'dark' ? 'light' : 'dark';
    document.body.setAttribute('data-theme', next);
    localStorage.setItem('theme', next);
    themeToggle.setAttribute('aria-pressed', String(next === 'light'));
  });
}

/* ═══════════════════════════════════════════════════════
   SCROLL-REVEAL — Elemente beim Scrollen einblenden
════════════════════════════════════════════════════════ */
const revealObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      revealObserver.unobserve(entry.target);
    }
  });
}, { threshold: 0.07 });

document.querySelectorAll('.reveal').forEach((el, i) => {
  // Staggered Delay für Cards
  if (el.closest('.services__grid, .pricing__grid, .referenzen__grid')) {
    el.style.transitionDelay = `${i % 4 * 100}ms`;
  }
  revealObserver.observe(el);
});

/* ═══════════════════════════════════════════════════════
   SERVICE-MODAL
════════════════════════════════════════════════════════ */
const modal    = document.getElementById('service-modal');
const modalBox = modal?.querySelector('.modal__content');

function openModal(serviceId) {
  const tpl = document.getElementById(`modal-data-${serviceId}`);
  if (!tpl || !modal) return;
  modalBox.replaceChildren(tpl.content.cloneNode(true));
  modal.hidden = false;
  document.body.style.overflow = 'hidden';
  // Lucide Icons im Modal initialisieren
  if (typeof lucide !== 'undefined') lucide.createIcons();
}

function closeModal() {
  if (!modal) return;
  modal.hidden = true;
  document.body.style.overflow = '';
}

document.querySelectorAll('.card__more').forEach(btn => {
  btn.addEventListener('click', () => {
    const serviceId = btn.closest('[data-modal]')?.dataset.modal;
    if (serviceId) openModal(serviceId);
  });
});

modal?.querySelector('.modal__close')
     ?.addEventListener('click', closeModal);

modal?.querySelector('.modal__backdrop')
     ?.addEventListener('click', closeModal);

document.addEventListener('keydown', e => {
  if (e.key === 'Escape') closeModal();
});

// Links im Modal: Modal schließen
modal?.addEventListener('click', e => {
  if (e.target.tagName === 'A' && e.target.hash === '#contact') closeModal();
});

/* ═══════════════════════════════════════════════════════
   PRICING-TOGGLE — Monatlich / Jährlich
════════════════════════════════════════════════════════ */
const pricingToggle = document.getElementById('pricing-toggle');

if (pricingToggle) {
  pricingToggle.addEventListener('click', () => {
    const yearly = pricingToggle.getAttribute('aria-checked') !== 'true';
    pricingToggle.setAttribute('aria-checked', String(yearly));

    document.querySelectorAll('.price').forEach(el => {
      el.textContent = yearly ? el.dataset.yearly : el.dataset.monthly;
    });
  });
}

/* ═══════════════════════════════════════════════════════
   KONTAKTFORMULAR
════════════════════════════════════════════════════════ */
const contactForm = document.getElementById('contact-form');
const formStatus  = document.getElementById('form-status');

if (contactForm) {
  contactForm.addEventListener('submit', async (e) => {
    e.preventDefault();
    const submitBtn = contactForm.querySelector('[type="submit"]');
    submitBtn.disabled = true;
    formStatus.textContent = 'Wird gesendet …';
    formStatus.className   = '';

    try {
      const res  = await fetch('/mail.php', {
        method: 'POST',
        body:   new FormData(contactForm),
      });
      const data = await res.json();

      if (data.ok) {
        formStatus.textContent = '✓ Nachricht gesendet! Ich melde mich bald.';
        formStatus.className   = 'success';
        contactForm.reset();
      } else {
        throw new Error(data.error || 'Unbekannter Fehler');
      }
    } catch (err) {
      formStatus.textContent = `✗ Fehler: ${err.message}`;
      formStatus.className   = 'error';
    } finally {
      submitBtn.disabled = false;
    }
  });
}

/* ═══════════════════════════════════════════════════════
   CANVAS PARTIKEL-ANIMATION
   Nur in der CTA-Sektion (#cta-section).
════════════════════════════════════════════════════════ */
const canvas = document.getElementById('particle-canvas');

if (canvas) {
  const ctx = canvas.getContext('2d');
  let particles = [];
  const COUNT   = 38;
  const DIST    = 120; // Max. Abstand für Verbindungslinie

  function resize() {
    canvas.width  = canvas.offsetWidth;
    canvas.height = canvas.offsetHeight;
  }

  function initParticles() {
    particles = Array.from({ length: COUNT }, () => ({
      x:  Math.random() * canvas.width,
      y:  Math.random() * canvas.height,
      vx: (Math.random() - 0.5) * 0.5,
      vy: (Math.random() - 0.5) * 0.5,
      r:  Math.random() * 2 + 1,
    }));
  }

  function getColor() {
    return getComputedStyle(document.documentElement)
      .getPropertyValue('--primary').trim() || '#6366f1';
  }

  function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    const color = getColor();

    particles.forEach(p => {
      p.x += p.vx;
      p.y += p.vy;
      if (p.x < 0 || p.x > canvas.width)  p.vx *= -1;
      if (p.y < 0 || p.y > canvas.height) p.vy *= -1;

      ctx.beginPath();
      ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
      ctx.fillStyle = color;
      ctx.globalAlpha = 0.6;
      ctx.fill();
    });

    for (let i = 0; i < particles.length; i++) {
      for (let j = i + 1; j < particles.length; j++) {
        const dx   = particles[i].x - particles[j].x;
        const dy   = particles[i].y - particles[j].y;
        const dist = Math.hypot(dx, dy);
        if (dist < DIST) {
          ctx.beginPath();
          ctx.moveTo(particles[i].x, particles[i].y);
          ctx.lineTo(particles[j].x, particles[j].y);
          ctx.strokeStyle = color;
          ctx.globalAlpha = 1 - dist / DIST;
          ctx.lineWidth   = 0.5;
          ctx.stroke();
        }
      }
    }

    ctx.globalAlpha = 1;
    requestAnimationFrame(draw);
  }

  resize();
  initParticles();
  draw();
  window.addEventListener('resize', () => { resize(); initParticles(); });
}

/* ═══════════════════════════════════════════════════════
   E-MAIL ROT13 DEKODIERUNG
   Schützt E-Mail vor Spam-Bots.
   Im HTML: data-rot13="xbagnxg@..." setzen.
════════════════════════════════════════════════════════ */
function rot13(str) {
  return str.replace(/[a-zA-Z]/g, c =>
    String.fromCharCode(
      c.charCodeAt(0) + (c.toLowerCase() < 'n' ? 13 : -13)
    )
  );
}

document.querySelectorAll('[data-rot13]').forEach(el => {
  const decoded = rot13(el.dataset.rot13);
  el.textContent = decoded;
  if (el.tagName === 'A') el.href = `mailto:${decoded}`;
});

/* ═══════════════════════════════════════════════════════
   FOOTER — Aktuelles Jahr
════════════════════════════════════════════════════════ */
const footerYear = document.getElementById('footer-year');
if (footerYear) footerYear.textContent = new Date().getFullYear();
```

---

## 6. Vorlage: `.htaccess`

```apache
# ═══════════════════════════════════════════════════════
# SICHERHEITS-HEADER
# ═══════════════════════════════════════════════════════
<IfModule mod_headers.c>
  Header always set X-Frame-Options "SAMEORIGIN"
  Header always set X-Content-Type-Options "nosniff"
  Header always set Referrer-Policy "strict-origin-when-cross-origin"
  Header always set Permissions-Policy "camera=(), microphone=(), geolocation=()"
  Header always set Content-Security-Policy "default-src 'self'; style-src 'self' 'unsafe-inline'; script-src 'self' 'unsafe-inline'; img-src 'self' data:;"
  Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
</IfModule>

# ═══════════════════════════════════════════════════════
# HTTPS REDIRECT
# ═══════════════════════════════════════════════════════
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{HTTPS} off
  RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
</IfModule>

# ═══════════════════════════════════════════════════════
# ZUGRIFFSKONTROLLE
# ═══════════════════════════════════════════════════════
Options -Indexes

# PHP-Dateien sperren (Ausnahmen: mail.php)
<FilesMatch "\.php$">
  Require all denied
</FilesMatch>
<Files "mail.php">
  Require all granted
</Files>

# Versteckte Dateien sperren
<FilesMatch "^\.">
  Require all denied
</FilesMatch>

# ═══════════════════════════════════════════════════════
# HTTP-CACHING
# ═══════════════════════════════════════════════════════
<IfModule mod_expires.c>
  ExpiresActive On

  # Bilder + Fonts: 1 Jahr
  ExpiresByType image/webp               "access plus 1 year"
  ExpiresByType image/avif               "access plus 1 year"
  ExpiresByType image/png                "access plus 1 year"
  ExpiresByType image/jpeg               "access plus 1 year"
  ExpiresByType image/svg+xml            "access plus 1 year"
  ExpiresByType font/woff2               "access plus 1 year"

  # CSS + JS: 1 Monat
  ExpiresByType text/css                 "access plus 1 month"
  ExpiresByType application/javascript   "access plus 1 month"
</IfModule>

<IfModule mod_headers.c>
  <FilesMatch "\.(webp|avif|png|jpg|jpeg|svg|woff2)$">
    Header set Cache-Control "public, max-age=31536000, immutable"
  </FilesMatch>
  <FilesMatch "\.(css|js)$">
    Header set Cache-Control "public, max-age=2592000"
  </FilesMatch>
</IfModule>

# ═══════════════════════════════════════════════════════
# KOMPRIMIERUNG
# ═══════════════════════════════════════════════════════
<IfModule mod_deflate.c>
  AddOutputFilterByType DEFLATE text/html text/css application/javascript
  AddOutputFilterByType DEFLATE image/svg+xml application/json
</IfModule>
```

---

## 7. Vorlage: `mail.php`

```php
<?php
// ═══════════════════════════════════════════════════════
// MAIL.PHP — Kontaktformular Backend
// ═══════════════════════════════════════════════════════

header('Content-Type: application/json; charset=utf-8');

// Nur POST erlauben
if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
    http_response_code(405);
    echo json_encode(['ok' => false, 'error' => 'Method not allowed']);
    exit;
}

// CSRF: Origin prüfen
$allowedOrigins = ['https://[DOMAIN]'];
$origin  = $_SERVER['HTTP_ORIGIN']  ?? '';
$referer = $_SERVER['HTTP_REFERER'] ?? '';

$originOk = in_array($origin, $allowedOrigins, true)
         || str_starts_with($referer, 'https://[DOMAIN]');

if (!$originOk) {
    http_response_code(403);
    echo json_encode(['ok' => false, 'error' => 'Forbidden']);
    exit;
}

// Honeypot prüfen
if (!empty($_POST['website'])) {
    // Stillschweigend abweisen (Bot täuschen)
    echo json_encode(['ok' => true]);
    exit;
}

// Input validieren und bereinigen
$name    = trim(filter_input(INPUT_POST, 'name',    FILTER_SANITIZE_SPECIAL_CHARS) ?? '');
$email   = trim(filter_input(INPUT_POST, 'email',   FILTER_VALIDATE_EMAIL)         ?? '');
$message = trim(filter_input(INPUT_POST, 'message', FILTER_SANITIZE_SPECIAL_CHARS) ?? '');

if (strlen($name) < 2 || !$email || strlen($message) < 10) {
    http_response_code(400);
    echo json_encode(['ok' => false, 'error' => 'Bitte alle Pflichtfelder ausfüllen.']);
    exit;
}

// Header-Injection verhindern
$email = str_replace(["\r", "\n"], '', $email);

// E-Mail senden
$to      = '[EMPFÄNGER-EMAIL]';
$subject = "Neue Kontaktanfrage von $name";
$body    = "Name: $name\nE-Mail: $email\n\nNachricht:\n$message";
$headers = implode("\r\n", [
    "From: $name <$email>",
    "Reply-To: $email",
    "X-Mailer: PHP/" . PHP_VERSION,
]);

$sent = mail($to, $subject, $body, $headers);

if ($sent) {
    echo json_encode(['ok' => true]);
} else {
    http_response_code(500);
    echo json_encode(['ok' => false, 'error' => 'E-Mail konnte nicht gesendet werden.']);
}
```

---

## 8. Vorlage: `robots.txt`

```
User-agent: *
Disallow: /mail.php
Disallow: /old/

Sitemap: https://[DOMAIN]/sitemap.xml
```

---

## 9. Vorlage: `sitemap.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://[DOMAIN]/</loc>
    <lastmod>[DATUM YYYY-MM-DD]</lastmod>
    <changefreq>monthly</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://[DOMAIN]/impressum.html</loc>
    <changefreq>yearly</changefreq>
    <priority>0.3</priority>
  </url>
  <url>
    <loc>https://[DOMAIN]/datenschutz.html</loc>
    <changefreq>yearly</changefreq>
    <priority>0.3</priority>
  </url>
</urlset>
```

---

## 10. Checkliste vor Go-Live

Gehe diese Liste mit dem Nutzer durch bevor die Seite live geht:

### Inhalt
- [ ] Alle `[PLATZHALTER]` ersetzt
- [ ] Alle Bilder vorhanden und optimiert (WebP/AVIF)
- [ ] Impressum vollständig (Pflicht in Deutschland)
- [ ] Datenschutzerklärung vollständig (DSGVO)
- [ ] E-Mail-Adresse per ROT13 verschleiert

### SEO
- [ ] `<title>` und `<meta description>` auf allen Seiten
- [ ] Open Graph Bild (1200×630 px) vorhanden
- [ ] Schema.org Daten vollständig und korrekt
- [ ] `sitemap.xml` aktuell
- [ ] Google Search Console einrichten

### Performance
- [ ] Lucide `lucide.min.js` heruntergeladen und selbst gehostet
- [ ] Alle Fonts self-hosted (keine Google Fonts)
- [ ] Bilder mit `sharp` in WebP/AVIF konvertiert
- [ ] `srcset` für alle wichtigen Bilder gesetzt
- [ ] `loading="lazy"` für alle Bilder außerhalb des sichtbaren Bereichs

### Sicherheit
- [ ] `.htaccess` Security-Header gesetzt
- [ ] HTTPS aktiv und Redirect funktioniert
- [ ] `mail.php` CSRF-Origin korrekt konfiguriert
- [ ] Kontaktformular getestet (Senden + Fehlerfälle)

### Technik
- [ ] Alle Links funktionieren (intern + extern)
- [ ] Formular auf echtem Server getestet (PHP läuft nicht lokal ohne Server)
- [ ] Mobile Ansicht auf echtem Gerät geprüft
- [ ] Browser-Test: Chrome, Firefox, Safari

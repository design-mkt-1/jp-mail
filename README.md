# jp-mail

Template-uri de email pentru brandul Jackpot. Construite **email-safe**: tabele + stiluri inline, compatibile Gmail / Outlook / Yahoo.

Preview live: https://design-mkt-1.github.io/jp-mail/

## Conținut

| Fișier | Descriere |
|---|---|
| `welcome-email.html` | Welcome email — banner, intro, 2 carduri bonus, 2 grile de jocuri, footer |
| `images/` | Toate asset-urile, exportate din Figma la 2x (retina) |
| `index.html` | Pagina de index servită de GitHub Pages |

Sursa design: Figma — fișierul *Jackpot*, frame `Welcome Email` (node `124:1952`).

## Zonele editabile per campanie

În `welcome-email.html`, fiecare zonă care se schimbă e marcată cu comentarii:

```
<!-- ═══════════ EDIT: PREHEADER ═══════════ -->
<!-- ═══════════ EDIT: BANNER HERO ═══════════ -->
<!-- ═══════════ EDIT: TITLU + INTRO ═══════════ -->
<!-- ═══════════ EDIT: OFERTA SPORT ═══════════ -->
<!-- ═══════════ EDIT: OFERTA CASINO ═══════════ -->
<!-- ═══════════ EDIT: JOCURI POPULAR ═══════════ -->
<!-- ═══════════ EDIT: JOCURI TOP GAMES ═══════════ -->
```

Textele ofertelor (`375%`, `UP TO £900`, butoanele) sunt text HTML real — se editează direct, fără re-export din Figma. Doar fundalurile cu glow sunt imagini (`card-sports-bg.png`, `card-casino-bg.png`).

## Înainte de un send real

1. **Imagini pe URL absolut.** Un client de email nu poate citi căi relative. Cu Pages activ, imaginile sunt deja publice, deci e suficient un find-replace:

   ```bash
   sed 's|images/|https://design-mkt-1.github.io/jp-mail/images/|g' \
     welcome-email.html > welcome-email.send.html
   ```

   Pentru producție recomandat un CDN propriu (Pages nu are SLA pentru trafic de campanie).

2. **Linkuri reale.** Toate `href` sunt `https://example.com` — de înlocuit: site, terms & conditions, unsubscribe, Telegram.

3. **Test de randare.** Litmus / Email on Acid / Mailtrap. Local se verifică doar în browser; Outlook desktop randează diferit (colțuri drepte în loc de `border-radius`, fără fonturi Google).

## Note tehnice

- Fonturi: Outfit / Space Mono / Inter via Google Fonts, cu fallback Arial / Courier New unde clientul blochează `@import` (Gmail, Outlook).
- Lățime 600px; sub 620px cardurile de bonus se stivuiesc, jocurile trec pe grilă 2×2.
- Fundalurile cardurilor au fost aplatizate din straturile Figma (glow + blend mode `color-dodge`, care nu există în email) într-un PNG per card.

# Claude-minne for notatapp-prosjektet

Dette er en lesbar kopi av minnene Claude bruker mellom samtaler.
Den interne versjonen ligger i `~/.claude/projects/.../memory/`.

---

## Prosjekt: Notatapp

Prosjektet er én enkelt fil: `notatapp.html`. Ingen rammeverk, ren HTML/CSS/JS med localStorage.

### Implementerte funksjoner
- Skriv, lagre, rediger og slett notater — rikt tekstfelt (`contenteditable`) med fet/kursiv/punktliste/H2 og ordteller
- Kilde-felt per notat (URL eller dokumentnavn)
- Etiketter (maks 5 per notat): pilleformede knapper, egendefinert farge per etikett
- Etikett-nedtrekksmeny: de 4 sist brukte vises synlig, resten i "Flere ▾"-dropdown. Bruksrekkefølge lagres i `localStorage` som `etikett-bruk`
- Legg til nye etiketter (lagres i `localStorage` som `etiketter`)
- Favorittmerking, liste-/kortvisning (toggle)
- Multi-select etikettfilter + søk, sorterer notater etter antall treff
- Høyreklikkmeny på notat-kort: Rediger / Slett, lukkes med Escape eller klikk utenfor
- Slett-knapp i skjemaet (vises kun i redigeringsmodus), med confirm()-dialog
- Ulagret-endringer-dialog ved navigering bort fra et påbegynt notat
- **Disposisjonsmodus** (steg 3, nå implementert): dra notatkort inn i seksjoner (Ingress, Åpning, Midtdel, Avslutning)
  - Pool til høyre med søk og etikettfilter, klikk på notat utvider/kollapser
  - Støtte for flere disposisjoner (opprett/bytt/slett), lagres i `localStorage` som `disp_liste`
  - Eksport av én disposisjon til `.txt`-fil
- **Sikkerhetskopi (lagt til 2026-09-02)**: "Eksporter alt" / "Importer"-knapper i toppfeltet
  - Eksporter alt: dumper hele localStorage (notater, etiketter, disposisjoner) til én JSON-fil, `notatapp-backup-ÅÅÅÅ-MM-DD.json`
  - Importer: leser en slik fil, bekreftelsesdialog, overskriver alt og laster siden på nytt
  - Formålet: localStorage flytter seg ikke automatisk ved PC-bytte — denne funksjonen gjør det mulig å ta med seg alle data manuelt

### Viktig om localStorage og `file://`
- Chrome (og trolig andre nettlesere) deler `localStorage` for ALLE `file://`-sider i én felles opprinnelse (`file://` uten host/sti) — altså IKKE partisjonert per mappeplassering. Åpning av `notatapp.html` fra ulike mapper på samme maskin/profil/nettleser gir tilgang til samme lagrede data.
- **PC-bytte 2026-09-30:** Under migrering viste det seg at et eldre notatarkiv (flere disposisjoner) ikke lenger fantes i noen nettleserprofil — bekreftet ved direkte lesing av Chrome/Edge sine LevelDB-lagringsfiler på disk. Dataene var borte fra localStorage før noen eksport/import-handling ble utført den dagen (trolig en tidligere "Slett nettleserdata" e.l., ikke en feil i eksport/import-funksjonen). Viste seg å være testinnhold — ikke reelt tap. Lærdom: eksporter jevnlig som sikkerhetskopi, ikke bare rett før en PC-bytte.

### Designretning
- Jordfarger: bakgrunn `#f5f0eb`, grønn `#7a9e7e`, Georgia serif
- Handlingsknapper: glassmorphism-inspirert — `rgba`-bakgrunn med lav opacity, `backdrop-filter: blur(4px)`, border-radius 8px
- Etikett-knapper: mindre og mer dempet enn handlingsknapper (0.78rem, lys grå)
- Hierarki: handlingsknapper (0.92rem) dominerer over etikett-knapper (0.78rem)
- Ved nye UI-elementer: hold deg til rgba-baserte halvgjennomsiktige stiler og Georgia serif. Unngå solide, opake knapper.

---

## Brukerprofil

- Lærer webutvikling på egenhånd (hobbyist/vibe coding)
- Jobber iterativt og utforskende — vil gjerne se resultatet før endelig beslutning
- Foretrekker at Claude implementerer direkte fremfor lange forklaringer med alternativer
- Norskspråklig — all kommunikasjon på norsk

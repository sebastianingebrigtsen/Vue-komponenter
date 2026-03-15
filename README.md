# Vue Komponenter

Dette er en liten samling av grunnleggende Vue 3-komponenter skrevet med Composition API og TypeScript. Målet med prosjektet er å bygge gjenbrukbare og fleksible komponenter fra bunnen av, uten å bruke eksterne biblioteker.

Stylingen er gjort med ren CSS, og komponentene er bygget for å takle mange ulike situasjoner med minimalt av overflødig kode.

## Komponentene

Prosjektet består av fire hovedkomponenter som kan testes i en samlet visning i `App.vue`:

*   **`BaseButton`**: En fleksibel knapp som støtter ulike varianter (standard, secondary, danger), ikoner (før og etter tekst), og som også kan fungere som en lenke hvis man sender inn en URL.
*   **`BaseInput`**: Et tekstfelt med støtte for ikoner, prefix/suffix, lazy update (oppdatering kun når man trykker Enter) og en integrert søkeknapp.
*   **`DataView`**: En listevisning som kan bytte mellom tabell og kort. Den håndterer søk, sortering alfabetisk og radvalg.
*   **`BaseModal`**: Et popup-vindu som kan brukes til enkle beskjeder, bekreftelse av sletting og redigering.

## Installasjon og kjøring

For å kjøre prosjektet lokalt på din maskin trenger du Node.js. Prosjektet benytter Vite som byggeverktøy for rask oppstart.

1. **Klon repoet ned til maskinen din:**
   ```bash
   git clone https://github.com/sebastianingebrigtsen/Vue-komponenter.git
   cd Vue-komponenter
   ```

2. **Installer alle pakkene:**
   ```bash
   npm install
   ```

3. **Start opp utviklingsserveren:**
   ```bash
   npm run dev
   ```

4. **Åpne prosjektet i nettleseren:**
   Nettleseren din vil normalt være tilgjengelig på `http://localhost:5173`.

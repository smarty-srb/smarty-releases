# smarty-releases

Javni repo **samo za firmware izdanja**. Nema izvornog koda — izvor je u privatnim
repozitorijumima `smarty-gateway`, `smarty-panel-gen2`, `smarty-mobile`.

Javan je namerno: uređaji na terenu moraju da povuku `.bin` bez ikakvog tokena.
Token ugrađen u firmware svako može da izvuče iz flash-a, pa je javni repo bez
tajni bezbedniji izbor od privatnog sa tokenom.

## Šta je gde

- `manifest.json` — uređaji ga čitaju da provere ima li novije verzije
- Releases — `.bin` fajlovi zakačeni kao assets

## manifest.json

```json
{
  "gateway": {
    "version": "2.9.1",
    "url": "https://github.com/smarty-srb/smarty-releases/releases/download/gw-v2.9.1/smarty_gateway.bin",
    "notes": "kratak opis izmena"
  }
}
```

Novi tipovi uređaja se samo dopišu kao novi ključ (`"panel"`, `"node_s"`, ...).
Stariji uređaji ignorišu ključeve koje ne poznaju, pa se ništa ne lomi.

## Objavljivanje nove verzije

1. U `smarty-gateway` repo-u: podigni `FIRMWARE_VERSION`, testiraj na hardveru preko USB-a
2. Arduino IDE → **Sketch → Export Compiled Binary**
   `.bin` završi u `smarty_gateway\build\esp32.esp32.esp32c6\smarty_gateway.ino.bin`
3. Commit + tag u `smarty-gateway` repo-u
4. Ovde: napravi Release sa tagom `gw-v2.9.1`, zakači `.bin` (preimenovan u `smarty_gateway.bin`)
5. Ažuriraj `manifest.json` i pushuj

**Redosled je bitan.** Manifest se ažurira POSLEDNJI — čim se promeni, uređaji
počinju da nude tu verziju. Ako `.bin` još nije gore, javljaju grešku.

## Pravila

- Nikad ne objavljuj build koji nije testiran preko USB-a na stvarnom uređaju
- Ne brisati stara izdanja — uređaj koji je preskočio nekoliko verzija i dalje
  radi, ali stara izdanja su jedini trag šta je tačno bilo na terenu
- Verzije prate `major.minor.patch`; uređaj poredi brojčano, ne po tekstu

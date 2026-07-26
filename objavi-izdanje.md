# Objavljivanje OTA izdanja — postupak

## 1. Testiraj lokalno (obavezno)

Flešuj preko USB-a i potvrdi da radi. **Nema automatskog vraćanja na prethodnu
verziju** — neispravan build objavljen kao OTA znači odlazak na teren sa kablom.

## 2. Izvezi .bin

Arduino IDE → **Sketch → Export Compiled Binary** (Ctrl+Alt+S)

Fajl:
```
C:\Users\jovca\Arduino\smarty_gateway\build\esp32.esp32.esp32c6\smarty_gateway.ino.bin
```

Preimenuj u `smarty_gateway.bin`.

## 3. Commit i tag izvornog koda

```powershell
cd C:\Users\jovca\Arduino\smarty_gateway
git add .
git commit -m "v2.9.1: opis izmena"
git push
git tag -a v2.9.1 -m "opis"
git push --tags
```

## 4. Napravi Release u smarty-releases

Na github.com/smarty-srb/smarty-releases → Releases → Draft a new release
- Tag: `gw-v2.9.1`
- Zakači `smarty_gateway.bin`
- Publish

## 5. Tek onda ažuriraj manifest

```powershell
cd C:\Users\jovca\smarty-releases
# izmeni manifest.json: version i url na novi tag
git add manifest.json
git commit -m "Gateway 2.9.1"
git push
```

Od ovog trenutka uređaji nude novu verziju.

## 6. Provera

Iz app-a pošalji `{"command":"ota_check"}` — Gateway treba da odgovori sa
`{"type":"ota_status","state":"available","version":"2.9.1",...}`.
Zatim `{"command":"ota_update"}` pokreće instalaciju.

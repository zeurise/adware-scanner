# Adware Scanner

Ingyenes, nyílt forráskódú Android-alkalmazás, amely egy nyilvános tiltólista alapján felismeri a reklámokkal teli, kéretlen alkalmazásokat (kamu „frissítők”, „vírusirtók”, „tisztítók”, reklámos kamerák, PDF/QR-olvasó klónok, játékok), és segít eltávolítani őket.

Fejlesztő és üzemeltető: **Csákó Attila** ([csakoattila.hu](https://csakoattila.hu))

Működés és a teljes aktuális tiltólista: **https://csakoattila.hu/adware/**

## Mit csinál?

- Összeveti a telefonon lévő (felhasználó által telepített) alkalmazásokat a tiltólistával.
- Találat esetén megmutatja, miért gyanús az app, és egy gombnyomással eltávolíthatod. **Az eltávolítást mindig a rendszer saját megerősítő ablaka kéri**, csendes törlés nincs.
- Kb. 15 percenként a háttérben megnézi, települt-e új alkalmazás, és ha tiltólistás, értesítést küld „Törlés” gombbal.
- Naponta letölti a friss tiltólistát, és minden appot újraellenőriz.
- Bármelyik appot be lehet jelenteni gyanúsként. A bejelentést a fejlesztő átnézi, és jóváhagyás után az app felkerül a közös listára.
- Rejtett szervizmód a lista karbantartóinak, amellyel bármelyik app közvetlenül felvehető a listára.

## Adatvédelem

Az app **nem gyűjt személyes adatot**, nincs benne reklám, analitika vagy fiók. A telepített appok listája a telefonon marad. Részletek: [PRIVACY.md](PRIVACY.md).

## Felépítés

```
android/          Android Studio projekt (Kotlin, Jetpack Compose, WorkManager)
server/adware/    PHP 8 + SQLite (vagy MySQL) modul a csakoattila.hu/adware/ alá
  api/list.php          nyilvános tiltólista (JSON, ETag)
  api/suggest.php       ügyféljavaslat beküldése
  api/admin_login.php   szervizmód bejelentkezés (token)
  api/admin_add.php     app felvétele a listára (tokennel)
  admin/                webes admin: javaslatok jóváhagyása, lista szerkesztése
  index.php             nyilvános tájékoztató oldal
  tajekoztato.php       nyomtatható ügyféltájékoztató (A4, 2×A5)
  inc/                  konfiguráció, séma, kezdő lista (weben nem elérhető)
  data/                 az SQLite adatbázisfájl helye (weben nem elérhető)
```

## Fordítás (Android)

1. Android Studio (Ladybug vagy újabb) → **Open** → `android` mappa.
2. Az első Gradle-szinkron letölti a függőségeket.
3. `app/build.gradle.kts`-ben állítsd be a `CONTACT_EMAIL`-t és a `GITHUB_URL`-t (és ha más a szerver címe, az `API_BASE`-t).
4. **Build → Generate Signed App Bundle / APK → APK**, saját aláírókulccsal.

Követelmény: Android 8.0 (API 26) vagy újabb.

Az APK a GitHub Releases-be kerül, nem a Play Áruházba, mert az app a `QUERY_ALL_PACKAGES` engedélyt használja (enélkül nem látná a telepített appokat).

## Szerver telepítése

Követelmény: PHP 8.0+ `pdo_sqlite` bővítménnyel (a legtöbb tárhelyen alapból megvan). Adatbázis-szerver nem kell.

1. Másold a `server/adware` mappát a webtárhelyre, hogy `https://csakoattila.hu/adware/` legyen.
2. `inc/config.sample.php` → `inc/config.php`, és írd át az `ip_salt`, a `contact_email` értékét és a linkeket.
3. A `data/` mappa legyen írható a webszerver számára (pl. `chmod 775 data`).
4. Nyisd meg a `https://csakoattila.hu/adware/api/list.php` címet. Az első hívás létrehozza a `data/adware.sqlite` fájlt a kezdő tiltólistával.
5. Nyisd meg a `https://csakoattila.hu/adware/admin/` oldalt, és hozd létre az admin fiókot.

Az `inc/` és a `data/` mappát `.htaccess` védi (Apache). Nginx alatt tiltsd le kézzel:
`location ~ ^/adware/(inc|data)/ { deny all; }`

**Biztonsági mentés:** elég a `data/adware.sqlite` fájlt menteni.

**MySQL-lel is működik:** a `config.php`-ben állítsd át a `db_dsn`-t MySQL-re, és futtasd le az `inc/install.sql`, majd az `inc/seed.sql` fájlt.

## A tiltólista formátuma

`GET /adware/api/list.php`

```json
{
  "ok": true,
  "version": 1794060000,
  "updated": "2026-11-07T14:00:00+00:00",
  "count": 32,
  "categories": { "fake_updater": "Kamu frissítő", "...": "..." },
  "apps": [
    { "package": "com.updatesoftware.updateallapps", "name": "Update All Apps",
      "risk": "high", "category": "fake_updater",
      "reason": "Kamu „szoftverfrissítő”: frissíteni nem tud, csak reklámokat jelenít meg." }
  ]
}
```

A lista szabadon felhasználható más projektekben is.

## Licenc

MIT – lásd [LICENSE](LICENSE).

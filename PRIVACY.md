# Adatkezelési tájékoztató – Adware Scanner

**Adatkezelő:** TELEFONVILÁG Kereskedelmi és Szolgáltató Kft., 4033 Debrecen, Jázmin utca 8., kapcsolat@telefonvilag.hu, +36 70 772 2607

## Mit nem gyűjt az alkalmazás?

- Nem kér fiókot, nevet, telefonszámot vagy e-mail-címet.
- Nem tartalmaz reklámot, analitikát vagy harmadik féltől származó követőkódot.
- A telefonon lévő alkalmazások listáját **nem küldi el**. Az ellenőrzés a telefonon történik.

## Mikor kommunikál a szerverrel?

1. **Tiltólista letöltése** (naponta egyszer, illetve kérésre). Ez egy egyszerű letöltés, mint egy weboldal megnyitása. Azonosítót nem küld.
2. **Gyanús alkalmazás bejelentése**, kizárólag a felhasználó kezdeményezésére. Elküldött adatok:
   - a bejelentett alkalmazás csomagneve és megjelenő neve,
   - a telepítés forrása (pl. „Play Áruház”, „APK-fájlból telepítve”),
   - az Adware Scanner verziószáma,
   - a felhasználó által opcionálisan beírt megjegyzés.

   Az IP-címet a szerver **nem tárolja**. Csak egy sózott, visszafejthetetlen kódot ment belőle (SHA-256), hogy a tömeges, automatizált bejelentéseket ki lehessen szűrni.

A bejelentések célja a közös tiltólista bővítése (jogalap: jogos érdek). Megőrzés: a bejelentés elbírálásáig, legfeljebb 2 évig.

## Szervizmód

A szerviz munkatársai a saját fiókjukkal léphetnek be. A belépéshez tartozó tokent az alkalmazás csak a telefonon, privát tárolóban tartja. A szerver ilyenkor a telefon gyártóját és típusát rögzíti (pl. „Samsung SM-A266B”), hogy az elveszett eszközök belépése visszavonható legyen.

## Engedélyek

| Engedély | Mire kell |
|---|---|
| `QUERY_ALL_PACKAGES` | a telepített alkalmazások listázása az ellenőrzéshez |
| `REQUEST_DELETE_PACKAGES` | az eltávolító ablak megnyitása (a rendszer mindig rákérdez) |
| `POST_NOTIFICATIONS` | értesítés tiltott app telepítésekor |
| `INTERNET` | tiltólista letöltése, bejelentés |
| `REQUEST_IGNORE_BATTERY_OPTIMIZATIONS` | kérés, hogy a rendszer ne állítsa le a háttérellenőrzést |

Kérdés vagy törlési kérelem: kapcsolat@telefonvilag.hu

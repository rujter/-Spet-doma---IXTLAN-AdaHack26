# Spet doma

Link do demo: https://rujter.github.io/Spet-Doma-Fikreti-AdaHack26-/

Enodatotečni predstavitveni primer aplikacije za okrevanje doma, namenjene pacientki, ki je po operaciji ravnokar prišla iz bolnišnice s **katetrom** in **stomo**. V eni strani sta dva pogleda: pregleden **pogled pacientke** z velikimi črkami in **pogled zdravnika**, v katerem zdravnik obravnava pacientkine zahteve in nastavitve.

> **Samo predstavitev.** Vse osebe, zdravila in telefonske številke so izmišljene (pacientka »Marija«, zdravnica »dr. Ana Novak«, telefon `01 XXX XX XX`). To ni medicinska programska oprema in ne nadomešča odpustnih navodil ali nasvetov zdravnika.

**Vmesnik je v celoti v slovenščini** (`lang="sl"`, datumi v obliki `sl-SI`, po želji tudi slovensko govorno branje).

## Zagon

Ničesar ni treba prevajati ali nameščati. Datoteko HTML odprite v sodobnem brskalniku.

- Brez strežnika, uporabniških računov in analitike.
- Stanje se shranjuje v brskalnikov `localStorage` pod ključem `spetDomaIntegrated.v2`, zato podatki ostanejo v tej napravi in tem brskalniku.
- Edini omrežni zahtevki so slogovna datoteka Google Fonts (Atkinson Hyperlegible Next, DM Serif Text) in zunanje povezave na seznamih »Viri«. Brez povezave se uporabita pisavi Arial in Georgia.
- Povezava **»Ponastavi prikaz«** v nogi strani vrne predstavitev na začetne podatke.
- Med pogledoma **Pacientka** in **Zdravnik** preklopite z gumbi na vrhu.

## Pogled pacientke

| Področje | Kaj počne |
|---|---|
| **Jutranja in večerna rutina** | Seznami opravil za posamezen del dneva s števcem napredka (»3 od 4 opravljeno«) in namigom »Zdaj je čas« glede na uro. Opravila zajemajo pregled katetra, vrečke za stomo, zdravila ter menjavo med nočno in dnevno katetrsko vrečko. |
| **Vodnik za menjavo nočne/dnevne vrečke** | Trije koraki s shemo SVG, ki poudari ustrezen del (mehur, ventil ali pipica, povezava cevi, nočna vrečka). Podpira vrečko s pipico in katetrski ventil. |
| **Zdravila** | Zdravila po jutru in večeru, narisana kot tablete glede na obliko in barvo, z namenom, opombo (»po obroku«) in številom preostalih tablet. Nov recept lahko zahteva v aplikaciji. |
| **Težave s katetrom** | Pomoč s sliko pri: urin ne priteka, pušča, vleče ali boli, barva urina. Vsaka ima korake in opozorilo »pokliči zdravnika, če …«. |
| **Menjava vrečke za stomo** | Ilustriran vodnik po korakih za enodelni in dvodelni sistem, po želji z govornim branjem korakov (🔊), če je v napravi nameščen slovenski glas. |
| **»Kaj je normalno danes«** | Besedilo glede na dan okrevanja (»Dan 20 doma · …«), ki opisuje, kaj je običajno in na kaj biti pozoren, v petih obdobjih: prvi dnevi, prvi teden, drugi teden, tretji in četrti teden ter po prvem mesecu. Vsaka trditev ima povezavo na vir. |
| **Stik z zdravnikom** | Vedno vidna kartica zdravnice s povezavo `tel:` ter gumbi za sporočilo, klic in tedenski pregled. Začetni zaslon in zasloni s pomočjo spomnijo, naj ob nevarnosti pokliče **112**. |
| **Sporočilo, dnevno poročilo in tedenski pregled** | Prosta sporočila in prošnje za recept; dnevno poročilo z oceno počutja, petimi področji, neobvezno fotografijo in oznako »prosim, pokličite me«; **tedenski pregled** s šestimi kratkimi vprašanji in oceno zdravja od 1 do 9, na katera lahko odgovori v aplikaciji ali po telefonu. |
| **Zaloga (»Moja zaloga«)** | Spremlja stomske vrečke ter dnevne in nočne katetrske vrečke. Ocenjuje, za koliko dni zadošča zaloga, opozori, kdaj je čas za naročilo, in omogoča prošnjo zdravnici za novo naročilnico. |
| **Kdo je kdo** | Oskrbovalna ekipa (zdravnica, patronažna sestra, stomaterapevtka, kirurg, prostovoljci ILCO, »oseba s ključem«) s fotografijo in telefonsko številko. |

### Predstavitev telefonskega klica

Gumb **»Kliči«** odpre vdelano telefonsko maketo (v `iframe`), ki posnema tedenski pregled po telefonu prek samodejnega glasovnega odzivnika z zvočnimi posnetki in vnosom s tipkovnico. Isti potek obstaja tudi v aplikaciji. Pri telefonskem poteku števke `1`–`9` izberejo odgovor, `*` vklopi ali izklopi »pokličite me nazaj«, `#` pa odgovore pošlje. Zvok je vdelan kot base64, zato je datoteka velika (približno 640 KB).

## Pogled zdravnika

Trije zavihki:

- **Zahteve:** nabiralnik sporočil, dnevnih poročil, tedenskih pregledov, prošenj za recept in prošenj za zalogo z značko neprebranih. Zdravnik lahko odgovori, odobri ali zavrne recept (z opombo) ter odobri naročilo pripomočkov s količino in datumom prevzema. Pri tedenskih pregledih je prikazan trend.
- **Zdravila:** dodajanje, urejanje in ukinjanje zdravil (ime, odmerek, namen, oblika, barva, jutro/večer, zaloga). Isti zavihek nastavlja, kako pogosto pacientka menja vrečko za stomo (vsak 1.–7. dan) in kako pogosto opravi tedenski pregled (7 ali 14 dni), ter vsebuje stanje zaloge pripomočkov z datumi prevzema in števec »dan doma«, ki določa besedilo »Kaj je normalno danes«.
- **Kdo je kdo:** urejanje imen, vlog in številk članov ekipe ter nalaganje fotografij. Fotografija zdravnice se pokaže tudi na pacientkinem začetnem zaslonu.

Odgovori in odločitve se pri pacientki pokažejo takoj, ker oba pogleda uporabljata isto lokalno stanje.

## Zgradba

- Ena samostojna datoteka HTML: vgrajen CSS in čisti JavaScript, brez ogrodij in odvisnosti.
- Vmesnik se izriše iz predlog v obliki nizov, en sam objekt stanja `S` pa se po vsaki spremembi zapiše v `localStorage`.
- Vse ilustracije (tablete, sheme katetra in stome) so vdelani SVG-ji, ustvarjeni v kodi.
- Dostopnost: berljiva pisava, osnovna velikost besedila 18 px, tipke vsaj 48 px, vidni obrobi fokusa, `aria-label` pri shemah in `aria-pressed` pri preklopnikih.
- Vsebina je v navadnih konstantah JavaScripta na vrhu skripte, zato jo je preprosto urejati: `ROUTINES`, `GUIDE`, `ISSUES`, `STOMA`, `PHASES`, `WQ` (tedenska vprašanja) in `SRC` (viri).

## Prilagajanje

| Kaj spremeniti | Kje |
|---|---|
| Ime pacientke ali zdravnice, telefon | `PATIENT` in `DOCTOR` na vrhu skripte |
| Začetna zdravila, zaloga, ekipa, sporočila | `seed()` |
| Opravila in točke na seznamih rutin | `ROUTINES` |
| Besedilo vodnika za nočno/dnevno vrečko | `GUIDE` |
| Odpravljanje težav s katetrom in koraki menjave stome | `ISSUES` in `STOMA` |
| Besedilo »normalno danes« in opozorila »pokliči, če« | `PHASES` |
| Vprašanja tedenskega pregleda | `WQ` |

Če spremenite obliko shranjenega stanja, spremenite tudi niz `KEY` (`spetDomaIntegrated.v2`), da se stari podatki v brskalniku ne zaletijo z novo zgradbo.

## Viri

Besedilo »kaj je normalno« je povzeto po splošnih virih (ACS, NHS, AARP, Harvard Health, UPMC, University of Utah, ZZZS, priročnik Zveze ILCO Slovenije, Onkološki inštitut Ljubljana in drugi). Celoten seznam je v konstanti `SRC` in prikazan pod posameznimi vnosi v aplikaciji. Pri dejanski uporabi bi ga morali pregledati zdravniki, ki pacientko zdravijo, in ga prilagoditi posamezni osebi.

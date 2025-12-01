# 📄 Lab 4 — Dokumentacija projektnog rješenja

## Naziv projekta: Garderobijer – sustav za upravljanje folklornim nošnjama i plesnim izvedbama  
* Autor: Marijan Ević (evian126)
* Akademska godina: 2024./2025.
* Kolegij: Uvod u programsko inženjerstvo

## 1. Uvod

Ovaj dokument opisuje dizajn, funkcionalnosti i arhitekturu informacijskog sustava Garderobijer, aplikacije namijenjene folklornim ansamblima za upravljanje:

* plesnim nošnjama

* pojedinačnim dijelovima nošnje

* plesačima

* koreografijama

* događajima i nastupima

* dostupnosti i inventurom nošnji

Cilj sustava je omogućiti garderobijerima precizno praćenje tko nosi koju nošnju, koliko je kompleta dostupno za pojedinu koreografiju te što nedostaje za određeni nastup. Aplikacija rješava problem kaosa u fizičkim garderobama ansambla i smanjuje pogreške u dodjeli nošnji.

## 2. Opis problema

Folklorni ansambli često imaju stotine dijelova nošnji, višestruke veličine i kompleta, koje se moraju slagati za svaki nastup. Tipični problemi:

* nema jasnog zapisa koliko kompleta ima za određenu koreografiju

* garderobijer ne zna koje dijelove netko trenutno ima

* nedostaje sustav posudbi i povrata

* teško je pratiti inventuru i oštećenja

* ručno vođenje bilješki dovodi do pogrešaka (kriva veličina, krivi komplet itd.)

Projekt garderiba digitalizira cijeli proces i omogućuje pregled u realnom vremenu.

## 3. Funkcionalni zahtjevi
### 3.1. Akteri

* Garderobijer – upravlja nošnjama, dodjeljuje dijelove, vodi inventuru

* Plesač – korisnik kojem se dodjeljuje nošnja

* Administrator – održava sustav

### 3.2. Use case scenariji (sažetak)
#### UC1 – Upravljanje plesačima

* dodavanje plesača

* uređivanje podataka

* deaktivacija plesača

#### UC2 – Upravljanje nošnjama

* unos nošnje

* dodavanje dijelova nošnje

* definiranje broja kompleta

* spajanje nošnje s koreografijama

#### UC3 – Dodjela nošnje plesaču

* izbor plesača

* pregled dostupnih kompleta

* dodjela i povrat

#### UC4 – Inventura

* pregled svih dijelova

* unos stanja

* označavanje oštećenih dijelova

* automatsko generiranje izvješća

#### UC5 – Upravljanje događajima i nastupima

* kreiranje događaja

* odabir koreografija

* pregled potrebne nošnje za nastup

* označavanje što nedostaje

## 4. Nefunkcionalni zahtjevi

* Dostupnost: sustav mora biti dostupan organizatorima i garderobijeru na terenu

* Performanse: lista nošnji <1 sekunde učitavanja

* Sigurnost: plesači i podaci o zalihama dostupni samo autoriziranim korisnicima

* Održivost: backend i baza moraju biti lako proširivi

* Upotrebljivost: sučelje mora biti jednostavno i brzo za korištenje u stresnim uvjetima pripreme nastupa

## 5. Arhitektura sustava
### 5.1. Struktura aplikacije

Aplikacija je predviđena kao višeslojni sustav:

* Frontend: JavaScript / React (moguće alternativno)

* Backend: C# .NET Web API

* Baza: Supabase

### 5.2. Modulni pregled

| Modul       | Opis                                      |
|-------------|-------------------------------------------|
| Plesači     | CRUD operacije plesača                    |
| Nošnje      | upravljanje kompletima, dijelovima, veličinama |
| Koreografije| spajanje nošnji s koreografijama          |
| Događaji    | nastupi + potrebna nošnja                 |
| Inventura   | stanje skladišta i oštećenja              |
| Izvještaji  | automatsko generiranje PDF izvještaja     |

## 6. Model podataka (Baza)
### 6.1. Entiteti
#### Plesac

* ID

* Ime

* Prezime

* Veličina (opcionalno)

* Broj noge

* Status

#### Nosnja

* ID

* Naziv

* Regija 

* Broj kompleta

* Opis

#### DioNosnje

* ID

* NosnjaID

* Naziv (npr. suknja, košulja, kapa)

* Veličina

* Stanje (ispravno/oštećeno)

#### Koreografija

* ID

* Naziv

* Opis

* Potrebna nosnja 

#### Dogadaj

* ID

* Naziv događaja

* Datum

* Koreografije na programu

#### Dodjela

* PlesacID

* DioNosnjeID

* DatumDodjele

* DatumPovrata

### 6.2. Relacijski dijagram (tekstualni opis)

* Plesac 1—N Dodjela

* DioNosnje 1—N Dodjela

* Nosnja 1—N DioNosnje

* Koreografija N—M Nosnja

* Dogadaj N—M Koreografija


## 7. Dijagrami
### 7.1. Use Case dijagram

* Akteri: Garderobijer, Plesač, Koreograf, Admin

* Ključni UC: Upravljanje nošnjama, Upravljanje plesačima, Dodjela nošnje, Inventura, Upravljanje događajima

### 7.2. UML Class Diagram 

Klase:

* Plesac

* Nosnja

* DioNosnje

* Koreografija

* Dogadaj

* Dodjela

Veze: kao u poglavlju modela podataka.


## 8. API dizajn

| Metoda | Ruta                    | Opis                         |
|--------|--------------------------|-------------------------------|
| GET    | /plesaci                | lista plesača                 |
| POST   | /plesaci                | dodavanje novog plesača       |
| GET    | /nosnje                 | lista nošnji                  |
| POST   | /nosnje                 | kreiranje nove nošnje         |
| GET    | /dogadaji               | svi događaji                  |
| POST   | /dogadaji               | dodavanje novog događaja      |
| POST   | /dodjela                | dodjela dijela plesaču        |
| PUT    | /dodjela/{id}/povrat    | označava povrat dodijeljenog dijela |


## 9. Primjer korisničkog tijeka (scenario)

Scenario: Priprema ansambla za nastup u Splitu.

* Garderobijer kreira događaj “Split – koncert”.

* Odabire koreografije: “Stari splitski plesovi”, “Završno kolo iz opere "Ero s onoga svijeta"”.

* Za svaku koreografiju odabire plesače koji će je izvoditi

* Sustav automatski izračunava potrebnu količinu nošnje na temelju:
– broja plesača po koreografiji
– tipa i broja dijelova potrebnih za tu koreografiju
– dostupnog broja kompleta u bazi

* Garderobijer otvara pregled svih plesača odabranih za nastup

* Sustav prikazuje listu potrebnih dijelova nošnje za svakog plesača.

* Garderobijer svakom plesaču dodjeljuje nošnje i komplete za odabrani ples

* Sustav upozorava ako nema dovoljno djelova nošnje ili kompleta.

* Nakon nastupa garderobijer označi povrat.

* Sustav generira inventuru razlika.
– vraćene dijelove
– oštećene dijelove
– dijelove koji nedostaju

## 10. Zaključak

Projekt Garderoba rješava realne potrebe folklornih ansambala i modernizira rad garderobe. Dokument daje pregled ključnih funkcionalnosti, arhitekture i modela podataka, te je temeljen na predlošku iz zahtjeva zadatka. Sustav se može dalje razvijati dodavanjem mobilne aplikacije, QR kod praćenja i automatskog inventara.

## 11. Reference

* PMFST – Uvod u programsko inženjerstvo, 2025.

* Materijali kolegija i predložak iz 2025-intro-swe/labs/lab4.md.

* Analiza folklornih garderoba u praksi (FA Jedinstvo).

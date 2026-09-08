# Taksonomia statusow: stan pomyslu na przestrzeni torow

> Tlumaczenie. Kanonem jest `../STATUS_TAXONOMY.md` w roocie repo. Zsynchronizowane z rootem: 2026-09-09. Gdy root sie zmienia, ta data jest jedynym sygnalem, ze tlumaczenie odstaje: w razie rozbieznosci obowiazuje root.

Po co: uzywanie "PARKED" jako jedynego statusu dla wszystkiego-co-nie-wdrozone to worek bez dna. Nie odroznia "jeszcze niezweryfikowane" od "zweryfikowane, ale czeka na moce przerobowe", od "zablokowane czyms zewnetrznym", od "odrzucone". Skutek: backlog puchnie, a rzeczy faktycznie zrobione wisza blednie opisane jako "plan" (czesty objaw: status zapisany w notatkach rozjezdza sie z realnym stanem produktu). Ta taksonomia daje kazdemu pomyslowi (a) miejsce w przeplywie torow oraz (b) jawna bramke do nastepnego kroku.

Ten plik to wspolny slownik statusow dla playbookow. Uzyte ponizej tokeny rol (PDL, BA, SA, Tech Lead, Backend, Frontend, QA, Security/Sec, PO) sa zdefiniowane w tych plikach: role Discovery (PDL, PS, UXR, MKT, PRC, DATA, EXP, SKEPTIC) i bramki (Outcome, Evidence, Demand, Worth-it, Go/No-Go) opisuje `DISCOVERY_PLAYBOOK.md`, a sekwencje Delivery (BA, SA, Tech Lead, Backend, Frontend, QA, Security) opisuje `DELIVERY_PLAYBOOK.md`. PO oznacza Product Ownera, ktory jest wlascicielem backlogu i metryki north-star w Twoim projekcie. "Slot" ponizej oznacza jedna jednostke mocy dostawczej zespolu: build, ktory zespol realnie moze wziac jako nastepny.

Przeplyw: `IDEA -> DISCOVERY -> (Go) -> READY -> DELIVERY -> STAGING -> SHIPPED`. Stany boczne: PARKED, BLOCKED, KILLED. Flagi kontrolne: PARTIAL, VERIFY. (RAT to status pipeline'u wewnatrz Discovery, patrz sekcja 1.)

---

## 1. Statusy pipeline'u (gdzie w przeplywie)

| Status | Znaczenie | Warunek wejscia |
|---|---|---|
| `IDEA` | surowy pomysl, bez triage'u | trafil do skrzynki okazji |
| `DISCOVERY` | w torze Discovery (pytanie "czy w ogole / co") | PDL przyjal go do toru; przechodzi przez bramki Discovery (Outcome, Evidence, Demand, Worth-it) |
| `RAT` | Discovery wykazalo werdykt WEAK (patrz werdykty ponizej); czeka na tani eksperyment | nazwane najbardziej ryzykowne zalozenie plus zaprojektowany RAT (Riskiest Assumption Test) |
| `READY` | popyt rozstrzygniety (Go z Discovery ALBO wazny powod pominiecia Discovery); Brief istnieje; czeka na slot w Delivery | bramka Go/No-Go zwrocila Go, albo jawny powod pominiecia (patrz Route E) |
| `DELIVERY` | w budowie (sekwencja od BA do Security wg `DELIVERY_PLAYBOOK.md`) | wszedl do Delivery |
| `STAGING` | zbudowany, czeka na QA i promote | wdrozony na srodowisko przedprodukcyjne / staging |
| `SHIPPED` | zyje na produkcji, rezultat potwierdzony | wypromowany na produkcje i docelowy rezultat zostal potwierdzony |

Werdykty Discovery (slownik uzywany powyzej i w maszynie stanow): **Go** (popyt udowodniony, przejscie do READY), **No-Go** (nie budowac, kierowac do PARKED lub KILLED), **WEAK** (kluczowe zalozenie nieudowodnione, kierowac do RAT na tani test przed decyzja). Te trzy to jedyne legalne wyniki Discovery.

## 2. Flagi kontrolne (nadal na pipelinie, ale z flaga)

| Status | Znaczenie | Wymaga |
|---|---|---|
| `PARTIAL` | czesc zyje na produkcji, reszta jest zdefiniowana, ale niezbudowana | jawny podzial: co jest zrobione, a co zostalo (faza X zrobiona / faza Y otwarta) |
| `VERIFY` | rzekomo wdrozone lub zrobione, ale niepotwierdzone (rezultat albo sam fakt wdrozenia) | akcja recon: sprawdzenie wdrozonego zrodla prawdy (np. wydany branch / dzialajacy build / tracker) lub pomiar metryki rezultatu |

## 3. Stany boczne (poza przeplywem, z powodem)

| Status | Znaczenie | WYMAGA (inaczej status jest nielegalny) |
|---|---|---|
| `PARKED` | swiadomie odlozone | wyzwalacz powrotu plus data przegladu plus maksymalny czas odlozenia (patrz regula limitu ponizej). Bez wyzwalacza to nie jest "odlozone", tylko "zgubione" |
| `BLOCKED` | blokada zewnetrzna | kto lub co odblokowuje (przyklady pogladowe: akcept prawny, wklad designu, dane dostepowe API strony trzeciej, podpisana umowa, decyzja biznesowa) |
| `KILLED` | odrzucone lub zastapione | powod. Nie wraca bez NOWEGO sygnalu |

**Maksymalny czas odlozenia (limit PARKED):** odlozenie nie jest bezterminowe. Kazdy element PARKED nosi, poza wyzwalaczem i data przegladu, maksymalny laczny czas odlozenia: domyslnie 90 dni, nadpisywalne per projekt w `PROJECT_PROFILE.md`. W kazdej dacie przegladu sa dokladnie trzy legalne ruchy: (a) wyzwalacz spelniony, element wraca do przeplywu (`DISCOVERY` albo `READY`); (b) wyzwalacz niespelniony, ale nadal prawdopodobny, odlozenie zostaje odnowione z nowa data przegladu (limit dalej liczy laczny czas odlozenia); (c) zadne z powyzszych, element idzie do `KILLED`. Gdy laczny czas odlozenia dobija do limitu, odnowienie przestaje byc domyslne: albo decydent jawnie uzasadnia odlozenie NOWYM, nazwanym powodem (co raz resetuje zegar), albo element idzie do `KILLED`. Odlozenie bez limitu odtwarza dokladnie ten bezdenny worek "PARKED", ktoremu ta taksonomia ma zapobiegac.

---

## 4. Route: ktora sekwencja (odpowiedz na "oba, czy tylko jedna?")

**Intake (przed drzewem).** Wybor trasy sam jest decyzja, wiec potrzebuje wejscia. Zlecenie w jednym zdaniu nie jest wejsciem. Zanim uruchomisz drzewo, wypelnij szesc slotow z tego, co zlecajacy juz napisal: **Ask** (deliverable w jednym zdaniu plus typ artefaktu: plan / architektura / kod / decyzja), **Job** (czyj problem, kiedy wystepuje, co sie psuje bez tego), **Decydent** (kto akceptuje artefakt, kto ma prawo veta), **Twarde ograniczenia** (stack, termin, budzet, rezim prawny lub regulacyjny, reguly organizacji), **Co juz istnieje** (produkt, kod, proces, systemy do integracji), **Sygnal sukcesu** (po czym zlecajacy pozna, ze zadzialalo).

Reguly Intake:

- **Ask jest jedynym slotem blokujacym.** Jesli deliverable jest niejasny, zapytaj i zatrzymaj sie; nie uruchamiaj drzewa. Pozostale piec nigdy nie blokuje.
- **Regula glebokosci:** zuzyj pytanie na slot tylko wtedy, gdy dwie prawdopodobne odpowiedzi zmienilyby artefakt materialnie (trasa, zakres, architektura). Nazwij te dwie odpowiedzi; jesli artefakt wychodzi identyczny przy obu, nie pytaj.
- **Recon przed pytaniem:** slot, ktory da sie ustalic czytajac kod, repo albo dokumentacje, rozstrzyga recon (bramka 0 w `DELIVERY_PLAYBOOK.md`), a nie pytanie. To wlasnie odroznia baze kodu, ktora mozesz przeczytac, od systemu klienta, ktorego nie mozesz.
- **Jedna partia, maksymalnie piec pytan, potem idziesz dalej.** Nie czekaj na odpowiedzi i nie pytaj turami. Intake, ktory blokuje, kosztuje wiecej niz zle przypisana trasa, ktorej zapobiega.
- **Niewypelnione sloty staja sie numerowanymi zalozeniami** na gorze artefaktu: `A-n: <slot> = <wartosc domyslna>; jesli falszywe: <co sie zmienia>`. Niesie je blok default-on-OK (regula przekrojowa 1 w `DELIVERY_PLAYBOOK.md`). Wartosci domyslne: Job = zlecenie wziete doslownie, persona = zlecajacy; Decydent = zlecajacy; Twarde ograniczenia = reguly z `PROJECT_PROFILE.md` i nic poza nimi; Co juz istnieje = to, co znalazl recon; Sygnal sukcesu = "przyjete przez decydenta" (jawnie slabe, oznacz do wymiany).
- **Intake skaluje sie do ryzyka** tak samo jak tory: trywialny element z fast path (Q4 ponizej), ktorego sloty da sie wywnioskowac, nie generuje ani jednego pytania.

| Route | Sciezka | Kiedy |
|---|---|---|
| `D->E` | Discovery, potem Delivery | niezweryfikowany popyt lub zaklad monetyzacyjny; konkuruje o slot |
| `E` | tylko Delivery (pominiecie Discovery) | popyt nie budzi watpliwosci (patrz regula ponizej) |
| `D` | tylko Discovery (bez budowy) | czyste badania lub strategia |
| `--` | to nie jest pomysl | referencja / twarda regula / strategia / ops |

**Drzewo decyzyjne trasy:** przechodz od gory do dolu dla kazdego elementu w triage'u; PIERWSZE pytanie z odpowiedzia TAK przypisuje trase i przejscie sie konczy. Zadnych ocen uznaniowych poza drzewem: jesli nie odpalilo zadne z pytan 1 do 4, odpowiedzia jest pytanie 5.

```
Q1. Czy to w ogole nie jest pomysl (referencja, twarda regula, notatka
    strategiczna, ops)?
      TAK -> Route --
Q2. Czy to czyste badania lub strategia, bez zobowiazania do budowy?
      TAK -> Route D
Q3. Czy popyt jest pewny Z DEFINICJI? To znaczy co najmniej jedno z:
      a. obowiazek prawny / compliance (przepisy o prywatnosci takie jak
         RODO, regulamin, dostepnosc, bezpieczenstwo, rezydencja danych)
      b. parytet table-stakes: bez tego produkt w ogole nie konkuruje
      c. dlug techniczny / infra / refactor (brak pytania o popyt)
      d. dokonczenie czegos, co juz czesciowo zyje na produkcji (PARTIAL)
      e. problem zgloszony I potwierdzony przez PO na podstawie obserwacji
         realnego uzytkownika
      TAK -> Route E (pomin Discovery; zapisz, ktora litera zadzialala)
Q4. Czy to trywialna poprawka ponizej progu fast path opisanego nizej?
      TAK -> Route E (fast path)
Q5. W przeciwnym razie: nowa funkcja user-facing o niepewnym popycie,
    zaklad monetyzacyjny albo duzy pomysl konkurujacy o ograniczony slot.
      -> Route D->E (Discovery wymagane)
```

**Prog fast path (Q4):** "trywialne" to liczba, a nie odczucie: szacowany naklad najwyzej 4 godziny od poczatku do konca (build plus test plus wydanie), ORAZ brak nowego trwalego stanu, ORAZ brak nowej zaleznosci zewnetrznej. Musza zachodzic wszystkie trzy warunki. Jesli estymata przekracza 4 godziny albo poprawka dodaje nowy byt lub nowa integracje, to nie jest trywialne: wroc do drzewa i wyladuj na Q5. Domyslne 4 godziny mieszkaja tutaj; nadpisz per projekt w `PROJECT_PROFILE.md`, ale trzymaj prog w godzinach, nie w dniach: fast path mierzony w dniach to po prostu nieprzejrzany build.

**Wymagaj Discovery (Route D->E, Q5):** Discovery ubija slaby pomysl tanim testem popytu (na przyklad fake-door, smoke test albo zapis na landing page) w godziny, zanim Delivery spali dni lub tygodnie.

**Linia audytowa:** kazdy element na Route E zapisuje, ktora galaz go wpuscila (litera Q3 od a do e, albo Q4 wraz z estymata), a kazdy element z przypisana trasa zapisuje, ktore sloty Intake byly zalozeniami, a nie faktami. Route E bez zapisanego powodu jest nielegalne, dokladnie tak samo jak PARKED bez wyzwalacza.

---

## 5. Wymagane metadane (kazdy zywy element)

- **Outcome**: ktorej dzwigni north-star sluzy. North star to jedyna najwazniejsza metryka sukcesu Twojego projektu, zadeklarowana w `PROJECT_PROFILE.md`. Przykladowy zestaw dzwigni (podmien na wlasny): akwizycja, aktywacja, retencja, przychod, compliance, dlug. Pomysl bez dzwigni nie ma prawa zajmowac slotu.
- **Bramka / Wyzwalacz**: co odblokowuje nastepny krok, albo warunek powrotu (dla PARKED / BLOCKED).
- **Najbardziej ryzykowne zalozenie**: to jedno zalozenie, ktore, jesli falszywe, ubija pomysl (obowiazkowe dla Route D->E; to warunek wejscia do RAT).

---

## 6. Legalne przejscia (maszyna stanow)

```
IDEA -> DISCOVERY
DISCOVERY --WEAK--> RAT --> DISCOVERY            (petla: ponowna ocena po tescie)
RAT --zalozenie obalone--> PARKED | KILLED       (oblany RAT moze zakonczyc pomysl)
DISCOVERY --Go--> READY                          (Route D->E)
DISCOVERY --No-Go--> PARKED | KILLED
(pominiecie) -> READY                            (Route E, popyt pewny)
READY -> DELIVERY -> STAGING -> SHIPPED
STAGING --QA oblane--> DELIVERY                  (poprawki, potem znow STAGING)
SHIPPED -> VERIFY                                (gdy rezultat lub fakt niepotwierdzony)
VERIFY --potwierdzone--> SHIPPED                 (recon dowiodl, ze realne, rezultat OK)
VERIFY --niezrobione / rezultat oblany--> DELIVERY   (ponowne otwarcie, by zbudowac lub naprawic)
DELIVERY | SHIPPED --podzial zakresu--> PARTIAL  (czesc zyje, reszta zdefiniowana)
PARTIAL --pozostaly zakres podjety--> READY      (reszta wraca przez READY)
PARKED --wyzwalacz spelniony--> DISCOVERY | READY
BLOCKED --blokada usunieta--> poprzedni status

Catch-all (nadpisuje lancuch liniowy): DOWOLNY stan -> PARKED (z wyzwalaczem) | BLOCKED (z tym, co odblokowuje) | KILLED (z powodem)
```

Twarda regula: do `DELIVERY` wchodzi tylko `READY`. `READY` powstaje albo z Go w Discovery, albo z jawnego pominiecia wedlug reguly Route E. Nic nie wchodzi do Delivery bokiem. Powyzszy catch-all ma pierwszenstwo nad lancuchem liniowym (dowolny stan moze przejsc do stanu bocznego), ale nigdy nie tworzy nowego bocznego wejscia do DELIVERY: element wychodzacy z BLOCKED wraca do poprzedniego statusu, a jesli tym statusem bylo READY (albo ponownie otwarty PARTIAL poprowadzony przez READY), wejscie do DELIVERY nadal idzie przez READY. Zatem regula "brak wejscia bokiem" i catch-all nie sa ze soba sprzeczne.

---

## 7. Wlasnosc i zapis

- PO jest wlascicielem przejsc statusow elementu i odpowiada za to, by zapisany status byl uczciwy.
- Zapisuj status tam, gdzie Twoj projekt sledzi prace (pole w trackerze, wiersz backlogu albo linia statusu w notatce elementu). Wybierz jedno zrodlo prawdy i trzymaj sie go konsekwentnie: rozjazd miedzy zapisanym statusem a realnym stanem produktu to dokladnie ta awaria, ktorej ta taksonomia ma zapobiegac. Wlasnie to sprawdza akcja recon w VERIFY.

---

## 8. Przyklady przerobione (generyczne)

1. **Funkcja D->E, Go.** Nowa mozliwosc user-facing o niepewnym popycie startuje jako `IDEA`, zostaje przyjeta do `DISCOVERY`, przechodzi tani test popytu, a bramka Go/No-Go zwraca Go. Staje sie `READY` (Brief napisany), czeka na slot, wchodzi do `DELIVERY`, dociera do `STAGING`, przechodzi QA i zostaje wypromowana. Gdy docelowy rezultat zostaje zaobserwowany, jest `SHIPPED`.

2. **WEAK, potem ubicie przez RAT.** Pomysl dociera do `DISCOVERY`, bramka Demand jest niejednoznaczna, wiec werdykt to WEAK. Najbardziej ryzykowne zalozenie zostaje nazwane i element przechodzi do `RAT`. Tani test obala zalozenie, wiec element trafia do `KILLED` z powodem (nie wroci bez nowego sygnalu).

3. **PARKED z wyzwalaczem.** Duzy pomysl jest realny, ale nie teraz. Przechodzi do `PARKED` z wyzwalaczem powrotu ("wroc, gdy [warunek] jest spelniony") i data przegladu. Gdy wyzwalacz zostaje spelniony, wraca do `DISCOVERY` (jesli popyt wciaz otwarty) albo do `READY` (jesli popyt juz rozstrzygniety).

4. **Przypadek VERIFY.** Element jest zapisany jako wdrozony, ale nikt nie potwierdzil rezultatu. Dostaje flage `VERIFY`. Akcja recon sprawdza zywe zrodlo prawdy i mierzy metryke: jesli potwierdzone, wraca do `SHIPPED`; jesli okazuje sie, ze nigdy nie bylo naprawde zrobione, zostaje ponownie otwarty do `DELIVERY`.

---

## Adaptacja do Twojego projektu

Ten plik jest generyczny celowo. Aby go zaadaptowac, trzymaj wszystkie specyfiki projektu w osobnym pliku projektowym (na przyklad `PROJECT_PROFILE.md`), a te taksonomie zostaw czysta i wspolna dla projektow:

- **Dzwignie north-star**: zastap przykladowy zestaw dzwigni (akwizycja, aktywacja, retencja, przychod, compliance, dlug) wlasnym i zadeklaruj swoja jedyna metryke north-star w `PROJECT_PROFILE.md`.
- **Wyzwalacze compliance**: dopasuj przyklady "prawo / compliance" dla Route E do rezimow, ktore faktycznie Cie dotycza (przepisy o prywatnosci, poziom dostepnosci, baza bezpieczenstwa, rezydencja danych, reguly branzowe).
- **Srodowiska**: zmapuj `STAGING` i `SHIPPED` na Twoje realne nazwy srodowisk, jesli sie roznia (pre-prod, canary, produkcja i tak dalej).
- **Mapowanie na tracker**: zmapuj kazdy status na Twoj tracker (etykieta, kolumna albo pole) i zapisz, gdzie status mieszka (patrz sekcja 7).
- **Tani test popytu**: wybierz tani test pasujacy do Twojego kontekstu (fake-door, smoke test, zapis na landing page, concierge MVP) i nazwij go w swoim profilu.
- **Limit PARKED**: nadpisz domyslne 90 dni maksymalnego czasu odlozenia, jesli Twoj rytm planowania wymaga innego, i zapisz nadpisanie w `PROJECT_PROFILE.md`.
- **Prog fast path**: nadpisz domyslny sufit 4 godzin dla trywialnej poprawki w `PROJECT_PROFILE.md`, jesli trzeba, trzymajac go w godzinach (patrz regula w sekcji 4).

Bramki sterujace przejsciami DISCOVERY -> RAT -> READY opisuje `DISCOVERY_PLAYBOOK.md`. Sekwencje budowy stojaca za `DELIVERY` opisuje `DELIVERY_PLAYBOOK.md`.

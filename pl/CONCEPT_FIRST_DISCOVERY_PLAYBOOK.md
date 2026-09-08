# Concept-First Discovery Playbook: zamodeluj domene, zanim zabierzesz sie za problem

> Tlumaczenie. Kanonem jest `../CONCEPT_FIRST_DISCOVERY_PLAYBOOK.md` w roocie repo. Zsynchronizowane z rootem: 2026-09-09. Gdy root sie zmienia, ta data jest jedynym sygnalem, ze tlumaczenie odstaje: w razie rozbieznosci obowiazuje root.

Dokument generyczny, niezalezny od projektu. Uzywaj go do modelowania domeny produktu (jej bytow i ich cykli zycia), zanim padnie pytanie, czy jakikolwiek konkretny ficzer warto budowac. Specyfike projektu (faktyczne byty, kod, core value albo north star) wstrzykuj w sekcji "Adaptacja do twojego projektu" i w PROJECT_PROFILE.md, nie w rdzeniu.

Czym to jest: NAJBARDZIEJ UPSTREAMOWY tor (Tor -1), umieszczony przed Sekwencja Discovery. Discovery bierze problem jako dany i pyta "czy warto to budowac". Ten tor stoi krok wczesniej i pyta "jaki ksztalt ma swiat, w ktorym dzialamy, i czego w nim strukturalnie brakuje". Modeluje domene jako zbior BYTOW i ich CYKLI ZYCIA, mapuje, gdzie doczepiaja sie ARTEFAKTY (notatki, rekordy, podsumowania, logi), a z luk produkuje liste problemow-kandydatow. Ta lista jest wejsciem do Discovery.

Po co istnieje: najdrozszym bledem w produkcie nie jest zly kod, tylko zbudowanie poprawnego rozwiazania dla zle sformulowanego bytu. Kiedy prosba przychodzi jako "scal te dwa pola notatek", "dodaj pole komentarza" albo "zrob lepszy widok historii", odpowiedz inzynierska (scal tabele, dodaj kolumne) leczy objaw i zostawia przyczyne: w domenie brakuje BYTU, do ktorego te artefakty powinny nalezec. Rozproszone notatki to nie problem notatek, to objaw brakujacego bytu, ktory bylby ich wlascicielem. Ten tor znajduje ten brakujacy byt, zanim ktokolwiek napisze linijke kodu albo choc uruchomi Discovery na poziomie ficzera.

Relacja do pozostalych torow: Concept-First Discovery produkuje mape domeny i rejestr luk. Kazda luka, jesli ma byc zbudowana, przechodzi przez Sekwencje Discovery (Go/No-Go, patrz DISCOVERY_PLAYBOOK.md), a po Go przez Sekwencje Delivery (patrz DELIVERY_PLAYBOOK.md). Ten tor nie ma zadnej wladzy, by wyslac cokolwiek prosto do budowy: jego output to material do walidacji, nie kontrakt na budowe. Granica jest twarda: ten tor konczy sie przed pytaniem "czy to sie oplaca" (to Discovery) i na dlugo przed projektowaniem rozwiazania (to Delivery).

Slownik statusow: luka, ktora ten tor oznaczy jako REAL, staje sie wpisem IDEA zasilajacym Sekwencje Discovery (patrz STATUS_TAXONOMY.md). Ustalenia typu konsolidacja i DROP (porzadki) omijaja Discovery i ida prosto do backlogu jako splata dlugu, bo usuwaja zamiast dodawac i nie potrzebuja walidacji popytu.

---

# Rdzen metody: trzy rozroznienia, ktore robia cala robote

## 1. Byt kontra artefakt
- Byt: ma tozsamosc i wlasny cykl zycia; istnieje w czasie niezaleznie od pojedynczej interakcji. Test: "czy ta rzecz zyje i zmienia stan przez wiele interakcji?" Przyklady generyczne: klient, zamowienie, projekt, sprawa, pacjent, konto.
- Artefakt: rekord, ktory powstaje na konkretnym etapie cyklu zycia jakiegos bytu i do niego nalezy; nie ma wlasnego luku. Test: "do czego to nalezy, co jest tego wlascicielem?" Przyklady generyczne: notatka, zalacznik, ocena, podsumowanie, flaga statusu, log komunikacji.
- Konsekwencja: jesli artefakt nie ma jasnego bytu-wlasciciela, to albo brakuje bytu, albo artefakt jest doczepiony w zlym miejscu. Wiekszosc prosb w stylu "ujednolic notatki" albo "dodaj komentarz" to sygnal osieroconego artefaktu.

## 2. Cykl zycia jako szkielet
Kazdy byt ma: narodziny, stany posrednie i przejscia oraz domkniecie (albo kontynuacje bez konca). Modeluj go jako sekwencje stanow, nie worek pol. Warto rozrozniac trzy dlugosci luku:
- Dlugi luk: byt najwyzszego poziomu (na przyklad klient albo czlonek, trwajacy miesiacami).
- Sredni luk: byt posredni (na przyklad projekt albo temat: sekwencja interakcji z poczatkiem i koncem).
- Krotki cykl: pojedyncza interakcja (na przyklad sesja albo transakcja: przed, w trakcie, po).
Najczestsza luka strukturalna to BRAKUJACY SREDNI LUK: produkt ma byt najwyzszego poziomu i pojedyncze interakcje, ale nie ma niczego, co spinaloby serie interakcji w jedna nazwana jednostke z domknieciem. Ten brakujacy srodek to dokladnie powod, dla ktorego artefakty laduja w rozproszeniu i dla ktorego miedzy interakcjami gubi sie ciaglosc.

## 3. Concept-first: abstrakcja przed kodem
Zamodeluj domene idealnie ("jak dziala swiat z punktu widzenia uzytkownika"), ZANIM spojrzysz w kod. Powod: kod koduje zlozonosc przypadkowa (historyczne decyzje, skroty, dlug). Jesli zaczniesz od kodu, zakotwiczysz sie na tym, co JEST, i nigdy nie zobaczysz brakujacego bytu, bo z definicji nie ma go w kodzie. Kolejnosc jest scisla: najpierw zbuduj model abstrakcyjny (soczewki 1 do 5), a dopiero potem skonfrontuj go z kodem (soczewka 6). Zlamanie kolejnosci lamie metode.

---

# Sekwencja: mapa

Orkiestracja przez caly czas: jeden prowadzacy (Domain Lead). Soczewki ida po kolei. Soczewki 1 do 5 sa abstrakcyjne (bez kodu). Soczewka 6 konfrontuje model z kodem. Soczewka 7 atakuje calosc.

1. L1 (Entities): wypisz byty i ich zagniezdzenie; oddziel byty od artefaktow.
2. L2 (Lifecycle): zmapuj kazdy byt od narodzin do domkniecia; znajdz puste etapy i brakujacy sredni luk.
3. L3 (Artifacts): przypisz kazdy artefakt do jednego bytu i jednego etapu cyklu zycia; wydobadz sieroty, duplikaty, martwe zapisy.
4. L4 (Pain): nazwij bol uzytkownika per byt i per przejscie; wskaz, gdzie rwie sie ciaglosc.
5. L5 (Value and Gaps): zmapuj byty na core value; wyprodukuj rejestr luk (brakujace byty, otwarte sekwencje, konsolidacje, DROPy).
6. L6 (As-is reconciliation): dopiero teraz przeczytaj kod; potwierdz albo skoryguj model wzgledem tego, co istnieje.
7. L7 (Skeptic): adwersarz; probuje ubic kazdy proponowany brakujacy byt (przemodelowanie, YAGNI).

Bramki (binarne, blokujace):
- Bramka A (Spojnosc modelu): kazdy element sklasyfikowany jako byt albo artefakt; drzewo zagniezdzenia spojne; kazdy cykl zycia ma poczatek i domkniecie.
- Bramka B (Dom artefaktu): kazdy artefakt ma wlasciciela i etap, albo jawny status sieroty / duplikatu / DROP. Zaden artefakt nie wisi w powietrzu.
- Bramka C (Uzasadnienie luk): kazda luka ma typ, range wedlug core value i przetrwala Sceptyka (REAL, nie OVER-MODEL).
- Bramka D (Concept-first zachowane): model abstrakcyjny powstal PRZED czytaniem kodu (soczewki 1 do 5 przed soczewka 6). Jesli zlamane, przebieg jest niewazny: powtorz.

Rekurencja: ten tor nie jest jednorazowy. Domena ewoluuje, wiec mapa wymaga rewizji, gdy pojawia sie nowy obszar produktu. Przepusc ponownie obszar, ktory sie zmienil, nie cala domene.

---

# Soczewki

## L1 (Entities): byty i zagniezdzenie

Misja: wypisz kazdy byt w domenie i pokaz jego zagniezdzenie lub zawieranie, czysto abstrakcyjnie. Oddziel byty (tozsamosc plus cykl zycia) od artefaktow (rekordow do nich doczepionych). To fundament; reszta na nim stoi.

Specyfikacja wyniku: lista bytow, kazdy z jednozdaniowa definicja (czym jest, co nadaje mu tozsamosc); drzewo zagniezdzenia (ktory byt zawiera ktory, od najwyzszego poziomu przez posredni po interakcje); osobna lista kandydatow na artefakty, zaparkowana dla L3; test na byt zastosowany do kazdego elementu (zyje przez wiele interakcji i zmienia stan = byt, w przeciwnym razie artefakt). Bounded context (warunkowo, nie zawsze): kiedy TA SAMA nazwa bytu znaczy istotnie rozne rzeczy w roznych czesciach biznesu (klasyczny przyklad: byt klienta, ktory co innego znaczy dla billingu, a co innego dla supportu), nazwij bounded context per znaczenie i albo rozdziel byt, albo otaguj go jego kontekstem. Rob to TYLKO wtedy, gdy znaczenie naprawde sie rozni; nie wymyslaj kontekstu dla kazdego bytu.

Definition of Done: kazdy element jest jawnie sklasyfikowany jako byt albo artefakt (zadnego "to zalezy"); istnieje drzewo zagniezdzenia z jednym bytem najwyzszego poziomu (albo jawna deklaracja, ze domena ma kilka rownorzednych korzeni); nazwy bytow to rzeczowniki z jezyka uzytkownika, nie ze schematu bazy; jesli jakas nazwa bytu niesie dwa istotnie rozne znaczenia w roznych czesciach biznesu, konteksty sa nazwane, a byt rozdzielony lub otagowany kontekstem, w przeciwnym razie jest to jawnie oznaczone jako nie-dotyczy.

Anty-wzorce: branie artefaktu za byt (modelowanie "notatki" jako bytu jest niemal zawsze bledem); modelowanie po nazwach tabel zamiast po jezyku domeny uzytkownika; pominiecie bytu, bo nie ma go w kodzie (cala istota tego toru to go znalezc); jeden byt-Bog, ktory co innego znaczy dla roznych zespolow (powinien byc rozdzielony po bounded context); nadkorekta, czyli wymyslanie kontekstow tam, gdzie znaczenie jest faktycznie to samo (eksplozja kontekstow, ktora sama w sobie jest przemodelowaniem do wyciecia przez Sceptyka).

Najmocniejsze dzwignie: test byt-kontra-artefakt (rozpuszcza wiekszosc prosb "scal te pola"); nazywanie w jezyku uzytkownika (odslania byty, ktore schemat ukryl); dopuszczenie bytow, ktorych jeszcze nie ma w kodzie.

## L2 (Lifecycle): cykl zycia per byt

Misja: dla kazdego bytu z L1 narysuj jego cykl zycia jako sekwencje (narodziny, stany i przejscia, domkniecie). Nazwij dlugosc luku (dlugi, sredni, krotki) i wskaz etapy, na ktorych obecnie nic nie jest zamodelowane.

Specyfikacja wyniku: tekstowy diagram stanow per byt (stan -> przejscie -> stan, z domknieciem albo jawnym "open-ended"); dlugosc luku kazdego bytu; puste etapy (etapy cyklu zycia bez zadnej reprezentacji w produkcie); test brakujacego srodka (czy istnieje byt o srednim luku, ktory spina interakcje; jesli nie, czerwona flaga dla L5). Dla kazdego przejscia nazwij TYP WYZWALACZA: akcja uzytkownika / zdarzenie systemowe / uplyw czasu / zaleznosc zewnetrzna. Oflaguj przejscia bramkowane przez cos POZA kontrola uzytkownika (system zewnetrzny albo uplyw czasu), bo to tam ciaglosc sie zacina i uzytkownik utyka miedzy stanami.

Definition of Done: kazdy byt ma cykl zycia z jawnym poczatkiem i jawnym domknieciem (albo "open-ended"); puste etapy sa nazwane (tam, gdzie ciaglosc nie ma nosnika); pytanie "czy brakuje sredniego luku" ma jawna odpowiedz; kazde przejscie ma nazwany typ wyzwalacza; przejscia bramkowane zewnetrznie sa oflagowane jako punkty ryzyka ciaglosci.

Anty-wzorce: cykl zycia zamodelowany jako worek pol zamiast sekwencji stanow; pominiecie domkniecia (byt, ktory nigdy sie nie "konczy", czesto znak brakujacego podsumowania); zalozenie, ze skoro uzytkownik nie prosil o etap "przed", to etap "przed" nie istnieje; modelowanie samych stanow bez tego, co napedza przejscia (Event Storming: zdarzenia napedzaja zmiany stanu); wciaganie szczegolow implementacyjnych (synchronicznie kontra asynchronicznie, konkretne API) do tej abstrakcyjnej soczewki, to sprawa Delivery/architekta, nie Toru -1.

Najmocniejsze dzwignie: domkniecie jako pelnoprawny etap (wydobywa brakujace podsumowanie konca luku); soczewka dlugosci luku (izoluje brakujacy srodek); nazywanie pustych etapow (lokalizuje, gdzie wycieka ciaglosc); soczewka typu wyzwalacza (lokalizuje przejscia bramkowane zewnetrznie, na ktorych utykaja uzytkownicy).

Granica wiarygodnosci: klasyfikacja wyzwalacza jako synchroniczny kontra asynchroniczny to wlasciwosc implementacyjna, ktora nalezy do architekta w Delivery, tutaj poza zakresem. Ta soczewka nazywa, CO napedza przejscie (uzytkownik, system, czas, zewnetrze), a nie JAK jest to okablowane.

## L3 (Artifacts): artefakty i wlasnosc

Misja: wez kazdy artefakt (notatki, oceny, podsumowania, logi, statusy, zalaczniki) i PRZYPISZ go do jednego bytu plus jednego etapu cyklu zycia tego bytu. Wydobadz sieroty (brak wlasciciela) i duplikaty (kilka kanalow na to samo). Kazdy artefakt niesie tez klase retencji/prywatnosci: kto moze go widziec, jak dlugo wolno go trzymac i ktory byt jest wlascicielem jego usuniecia.

Specyfikacja wyniku: tabela przypisan (artefakt, byt-wlasciciel, etap cyklu zycia, kto lub co go czyta, prywatnosc/retencja, status: zywy / martwy / duplikat / sierota); wartosc prywatnosc/retencja to jedno z None / Confidential / Personal-data (z regula retencji lub usuwania); sieroty (artefakty bez jasnego bytu, kandydaci na brakujace byty dla L5); duplikaty (kanaly trzymajace to samo, do konsolidacji); martwe zapisy (artefakty bez czytelnika, ktore nie zasilaja zadnego procesu, kandydaci na DROP).

Definition of Done: kazdy artefakt ma wlasciciela plus etap albo jest jawnie oznaczony jako sierota; duplikaty sa oflagowane parami; kazdy artefakt ma nazwanego czytelnika, a brak czytelnika daje kandydata na DROP z uzasadnieniem; kazdy artefakt ma klase prywatnosci/retencji; kazdy artefakt z danymi osobowymi ma nazwanego wlasciciela usuniecia (swoj byt-wlasciciela); OSIEROCONY artefakt z danymi osobowymi jest oflagowany jako ryzyko compliance (brak wlasciciela oznacza, ze jest trzymany bezterminowo), a nie tylko jako kwestia porzadku.

Anty-wzorce: "to mogloby nalezec do kilku bytow" (wymus jednego glownego wlasciciela, reszta to referencje); trzymanie martwego zapisu, bo "moze sie kiedys przydac" (brak czytelnika = dlug); przeoczenie, ze dwa artefakty to duplikaty, bo maja rozne nazwy; traktowanie osieroconego artefaktu z danymi osobowymi jako tylko sprzatania, podczas gdy to ryzyko retencji/prywatnosci (na przyklad RODO).

Najmocniejsze dzwignie: mapa czytelnikow (obiektywnie obnaza martwe zapisy); jeden glowny wlasciciel per artefakt; skan duplikatow (skleja rownolegle systemy notatek); soczewka wlasciciela usuniecia (to byt-wlasciciel czyni usuwanie/retencje egzekwowalnymi, artefakt bez bytu-wlasciciela nie ma nikogo, kto wyegzekwuje jego usuniecie).

## L4 (Pain): bol per byt i per przejscie

Misja: dla kazdego bytu i kazdego przejscia cyklu zycia nazwij BOL uzytkownika: co trzyma w glowie, co traci, gdzie marnuje czas, gdzie miedzy interakcjami rwie sie ciaglosc. Ta soczewka laczy abstrakcyjny model z ludzkim kosztem.

Specyfikacja wyniku: bol per byt (co boli w prowadzeniu tego bytu); bol per przejscie (gdzie rwie sie ciaglosc, na przyklad "miedzy interakcjami nie ma zadnego podsumowania, zaczynam od zera"); powiazanie wstecz z pustymi etapami z L2 i sierotami z L3 (bol zwykle siedzi dokladnie tam, gdzie etap jest pusty albo artefakt osierocony).

Definition of Done: kazdy byt ma co najmniej jeden konkretny, nazwany bol (albo jawne "tu nie boli"); bol jest ujety jako bol uzytkownika, nie jako brakujacy ficzer; bol jest przypiety do konkretnego etapu albo sieroty, nie generyczny.

Anty-wzorce: bol przebrany za rozwiazanie ("nie ma pola na podsumowanie" zamiast "gubie watek miedzy sesjami"); bol bez lokalizacji w cyklu zycia (generyczne "jest niewygodnie"); wymyslanie bolu zza biurka zamiast przypiecia go do dopiero co znalezionego pustego etapu albo sieroty.

Najmocniejsze dzwignie: wiazanie bolu z pustymi etapami (ugruntowuje rejestr luk); ujecie jako bol, nie ficzer (trzyma opcje otwarte); soczewka przejsc (znajduje bol utraconej ciaglosci, ktorego widoki per byt nie widza).

## L5 (Value and Gaps): brakujace byty i sekwencje

Misja: zmapuj byty i ich cykle zycia na CORE VALUE produktu (jego racje istnienia) i wyprodukuj REJESTR LUK: brakujace byty (przede wszystkim brakujacy sredni luk), otwarte przejscia (sekwencje, ktore powinny byc automatyczne, a nie sa) oraz artefakty do konsolidacji albo DROP. To jest produkt calego toru.

Specyfikacja wyniku: mapa wartosci (core value, ktore byty i przejscia ja niosa, gdzie petla jest otwarta); rejestr luk (glowny artefakt), kazda luka z typem (MISSING ENTITY, OPEN SEQUENCE, CONSOLIDATION, DROP); ranking luk wedlug wplywu na core value; lista downstream-resolved (rzeczy, ktore znikaja same, gdy tylko brakujacy byt powstanie, wiec nie buduje sie ich osobno); kazda luka przeznaczona do budowy ujeta jako PROBLEM-kandydat gotowy wejsc do Discovery (jako bol, nie rozwiazanie). Dla kazdej luki przeznaczonej do budowy podaj tez jej OGRANICZENIE ARCHITEKTONICZNE: inwariant, ktory kazde rozwiazanie musi respektowac. Przyklad dla luki typu MISSING ENTITY: "kazde rozwiazanie musi wprowadzic byt; latka na poziomie pol istniejacego bytu nie rozwiazuje luki strukturalnej i ponownie osieroci artefakt." To jest OGRANICZENIE/inwariant, NIE projekt rozwiazania (projektowanie rozwiazania wciaz jest poza zakresem, to Delivery).

Definition of Done: kazda luka ma typ; luki sa uszeregowane wedlug wplywu na core value; pozycje downstream-resolved sa wypisane (przeciw nadbudowywaniu); kazda luka przeznaczona do budowy jest ujeta jako problem-kandydat, nie rozwiazanie; kazda luka przeznaczona do budowy podaje swoje ograniczenie architektoniczne jako inwariant (nie projekt).

Anty-wzorce: projektowanie rozwiazania ("zbudujmy tabele z takimi polami") zamiast zatrzymania sie na "brakuje bytu"; pchanie luki prosto do budowy z pominieciem walidacji popytu w Discovery; pominiecie downstream-resolved i budowanie osobno tego, co brakujacy byt zalatwia za darmo.

Najmocniejsze dzwignie: rama brakujacego bytu (przeramowuje stos feature requestow w jedna luke architektoniczna); downstream-resolved (zapobiega zbednym budowom); ranking wedlug core value (porzadkuje kandydatow do Discovery).

## L6 (As-is reconciliation): konfrontacja z kodem

Misja: DOPIERO TERAZ spojrz w kod (lista bytow, gdzie artefakty faktycznie sie zapisuja, co jest martwe) i skonfrontuj go z idealnym modelem z L1 do L5. Roznice potwierdzaja luki (albo koryguja model). Tutaj wpina sie fan-out czytania kodu (rownolegli czytelnicy z cytatami file:line).

Specyfikacja wyniku: tabela model-kontra-kod (byt lub artefakt z modelu, reprezentacja w kodzie z file:line albo "brak", werdykt: potwierdzona luka / istnieje / korekta modelu); potwierdzenia (ktore luki z L5 kod twardo potwierdza); korekty modelu (co kod ujawnil, czego modelowi brakowalo); usuniete falszywe alarmy (rzeczy, ktore wygladaly na luke, a istnieja, na przyklad pole odziedziczone z klasy bazowej).

Definition of Done: kazdy byt lub artefakt z modelu ma werdykt ugruntowany w prawdziwym kodzie (file:line), nie w pamieci; sprzecznosci miedzy zrodlami rozstrzyga sie czytaniem zrodla, nie glosowaniem; model jest zaktualizowany o korekty z kodu.

Anty-wzorce: startowanie od kodu (lamie concept-first, patrz Rdzen metody, rozroznienie 3); branie wniosku czytelnika za fakt bez sprawdzenia zrodla przy sprzecznosci; mylenie "kod nie czyta tego artefaktu" (martwy zapis, realny sygnal) z "uzytkownicy tego nie uzywaja" (co wymaga telemetrii, nie kodu).

Najmocniejsze dzwignie: scisla kolejnosc (najpierw model abstrakcyjny sprawia, ze konfrontacja jest potwierdzeniem, nie kotwica); ugruntowanie file:line (zabija sprzecznosci miedzy czytelnikami); granica czytelnik-kontra-uzycie (utrzymuje uczciwosc twierdzen o martwych zapisach).

Granica wiarygodnosci: kod mowi, co ISTNIEJE i co jest CZYTANE przez system. Nie mowi, jak czesto uzytkownik faktycznie czegos uzywa: to wymaga analityki albo telemetrii i lezy poza zakresem tego toru.

## L7 (Skeptic): red-team

Misja: sprobuj UBIC kazdy proponowany brakujacy byt i kazda luke. Najczestszy blad modelowania to over-engineering: wymyslanie struktury, ktorej domena nie potrzebuje. Sceptyk domyslnie mowi "nie dodawaj bytu" i zmusza reszte, zeby to obalila.

Specyfikacja wyniku: atak na kazdy brakujacy byt (czy domena go potrzebuje, czy po prostu chcemy czystej architektury); atak na granice bytow (czy podzial albo scalenie jest poprawne, jeden byt kontra dwa); atak na luki (czy bol uzasadnia strukture, czy wystarczylby mniejszy ruch, artefakt na istniejacym bycie); rekonesans "czy to juz istnieje" (nie wymyslaj luki, ktora jest juz zapelniona); werdykt per luka (REAL = zostaje, OVER-MODEL = skreslic, DOWNGRADE = wystarczy artefakt, nie byt).

Definition of Done: kazdy brakujacy byt dostal konkretny atak (nie generyczny); YAGNI sprawdzone (czy wystarczylby prostszy ruch, artefakt na istniejacym bycie); rekonesans "czy to juz istnieje" wykonany; werdykt REAL / OVER-MODEL / DOWNGRADE per luka.

Anty-wzorce: sceptycyzm na pokaz (przepuszcza kazdy ladny byt); brak ataku YAGNI (model puchnie od bytow, ktorych nikt nie potrzebuje); pominiecie rekonesansu (rejestr zawiera rzeczy juz zbudowane).

Najmocniejsze dzwignie: domyslne "nie dodawaj bytu" (odwraca ciezar dowodu); rekonesans istnienia (usuwa luki juz zbudowane); opcja downgrade (artefakt zamiast bytu czesto wystarcza).

---

# Szablon wyniku: mapa domeny plus rejestr luk (handoff do Discovery)

```
## Mapa domeny: <nazwa domeny lub obszaru>
Data: <YYYY-MM-DD>

BYTY (byt | definicja | dlugosc luku | zawiera):
- <byt najwyzszego poziomu> | <def> | dlugi  | <byty posrednie>
- <posredni>                | <def> | sredni | <interakcje>      [<-- jesli BRAK: brakujacy srodek]
- <interakcja>              | <def> | krotki | -

CYKLE ZYCIA (byt: stan -> ... -> domkniecie | puste etapy):
- <byt>: <narodziny> -> <stan> -> <domkniecie>  | PUSTE: <etap bez nosnika>

ARTEFAKTY (artefakt | wlasciciel | etap | czytelnik | prywatnosc/retencja | status):
- <artefakt> | <byt> | <etap> | <kto to czyta> | <None / Confidential / Personal-data + regula> | zywy/martwy/duplikat/sierota

## Rejestr luk (ranking wedlug core value)
| # | Luka | Typ | Wplyw na core value | Ograniczenie architektoniczne | Werdykt Sceptyka | Kandydat do Discovery? |
|---|------|-----|---------------------|-------------------------------|------------------|------------------------|
| 1 | <brakujacy byt X> | MISSING ENTITY | <jak domyka petle> | <inwariant, ktory Delivery musi uszanowac, np. musi wprowadzic byt, nie latke na poziomie pol> | REAL | TAK, problem: "<bol uzytkownika>" |
| 2 | <auto przejscie Y> | OPEN SEQUENCE | <...> | <inwariant> | REAL | TAK |
| 3 | <duplikat A/B>     | CONSOLIDATION | <...> | -            | REAL | nie (porzadki) |
| 4 | <martwy zapis Z>   | DROP | <...> | -            | REAL | nie (usunac) |

DOWNSTREAM-RESOLVED (znika, gdy tylko brakujacy byt powstanie):
- <rzecz NIE budowana osobno, bo brakujacy byt ja zalatwia>

KOREKTY MODELU Z KODU (L6):
- <co kod ujawnil albo usunal>
```

Kazdy wiersz "Kandydat = TAK" wchodzi do Sekwencji Discovery jako luzny problem (inbox sygnalow lub okazji), gdzie dostaje outcome, sygnal popytu, RAT i Go/No-Go. Konsolidacje i DROPy ida prosto do backlogu porzadkowego (nie potrzebuja walidacji popytu, splacaja dlug). Ograniczenie architektoniczne wedruje RAZEM z problemem-kandydatem do Briefu Discovery i musi byc uszanowane przez Delivery, tak zeby zwalidowany problem nie zostal pozniej rozwiazany latka na poziomie pol, ktora odtwarza luke (taka jest tu intencja, bez edycji DISCOVERY_PLAYBOOK.md ani DELIVERY_PLAYBOOK.md).

---

# Kiedy uruchamiac ten tor (wyzwalacze)

- Prosba jest sformulowana "nisko-inzyniersko": "scal te notatki", "dodaj pole komentarza", "zrob lepszy widok historii". Podejrzewaj osierocony artefakt albo brakujacy byt.
- Artefakty sa rozproszone bez jasnego wlasciciela (kilka rodzajow notatek lub rekordow robiacych podobne rzeczy).
- Przed duza decyzja architektoniczna, ktora dotyka wielu ficzerow naraz (migracja, refactor modelu danych).
- Wejscie w nowy obszar produktu, ktorego domena nie zostala jeszcze zamodelowana.
- Czujesz, ze "petla wartosci domyka sie tylko w polowie": klasyczny objaw brakujacego sredniego luku.

Kiedy NIE uruchamiac: pojedynczy, dobrze zdefiniowany ficzer w juz zamodelowanej domenie (idz prosto do Discovery). Drobna poprawka albo bug (idz prosto do Delivery albo po prostu zrob).

---

# Ciagla petla

Ten tor nie jest jednorazowa bramka. Mapa domeny to zywy artefakt. Przepusc soczewki ponownie, gdy pojawia sie nowy obszar, gdy powracajaca prosba "scal te pola" sygnalizuje osierocony artefakt albo gdy budowa w kolko potyka sie o ten sam brakujacy srodek. Utrzymuj mape tania w utrzymaniu: produktem jest mapa i lista, nie kod, wiec jeden przebieg to godziny do dnia lub dwoch, nie tygodnie.

---

# Anty-wzorce calego toru

- Code-first zamiast concept-first: start od schematu gwarantuje, ze nie zobaczysz brakujacego bytu.
- Przemodelowanie: dodawanie bytow dla elegancji architektury, nie dla bolu domeny (Sceptyk istnieje po to, zeby to wycinac).
- Pomieszanie torow: projektowanie rozwiazania albo walidacja popytu w tym torze (to Discovery i Delivery). Ten tor konczy sie na "brakuje bytu / istnieje luka".
- Artefakt jako byt: modelowanie notatki albo komentarza jako pelnoprawnego bytu zamiast rekordu doczepionego do bytu.
- Pchanie do budowy z pominieciem Discovery: rejestr luk to kandydaci do walidacji, nie zlecenie budowy.
- Pominiecie downstream-resolved: budowanie osobno tego, co brakujacy byt zalatwia za darmo (na przyklad "scalenie notatek" jako osobny projekt).

---

# Adaptacja do twojego projektu

Utrzymuj ten playbook generycznym. Specyfike projektu trzymaj w PROJECT_PROFILE.md i stamtad ja tu przywoluj:
- Faktyczne byty i ich nazwy w jezyku uzytkownika (rzeczowniki domeny).
- Core value albo north star, wzgledem ktorych rankuje sie rejestr luk.
- Lokalizacje kodu dla L6 (katalog bytow, warstwa persystencji, historia migracji) oraz czy dostepny jest fan-out czytania kodu.
- Twarde reguly i rejestr pulapek, ktore twoj stack narzuca, gdy luka trafi do Delivery (trzymane w PROJECT_PROFILE.md, nie tutaj).
- Granica wiarygodnosci twoich dowodow: kod mowi, co istnieje i co jest czytane; czestotliwosc uzycia wymaga telemetrii, ktora odnotowujesz jako osobne zrodlo, jesli jest dostepna.

Output tego toru (konkretna mapa domeny i rejestr luk dla twojego produktu) to WYNIK: trzymaj go w swoim projekcie, nie w tym repo z szablonami. To repo pozostaje czystym, generycznym zrodlem.

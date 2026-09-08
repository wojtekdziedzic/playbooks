# Playbook sekwencji Delivery: maksimum z kazdej roli

> Tlumaczenie. Kanonem jest `../DELIVERY_PLAYBOOK.md` w roocie repo. Zsynchronizowane z rootem: 2026-09-09. Gdy root sie zmienia, ta data jest jedynym sygnalem, ze tlumaczenie odstaje: w razie rozbieznosci obowiazuje root.

Generyczny i niezalezny od projektu. Wez go i zastosuj do dowolnego projektu software'owego. Specyfike swojego projektu (twarde reguly, stack, persony, deadline) wstrzykujesz w sekcji "Adaptacja do twojego projektu" na koncu, nigdy do rdzenia playbooka. Rdzen ma zostac czysty i wspolny dla wszystkich projektow.

Czym to jest: sposobem przeprowadzenia BUDOWY ficzera przez sekwencje 7 rol (BA: Business Analyst, SA: Solution Architect, Tech Lead, Backend, Frontend, QA, Security) tak, aby kazda dzialala na swiatowym poziomie, a calosc nie tracila jakosci ani nie palila czasu. To tor "JAK to dobrze zbudowac" (tor 1).

Relacja do Discovery: Delivery zaklada, ze decyzja o budowaniu juz zapadla. Wejsciem jest Brief z sekwencji Discovery (Go); zobacz DISCOVERY_PLAYBOOK.md. Jesli nie masz Discovery, wejsciem jest zwalidowany problem od decydenta. Nic bez zwalidowanego problemu tu nie wchodzi. Slownik statusow uzywany wokol tego toru (READY, DELIVERY, STAGING, SHIPPED, PARTIAL, VERIFY, PARKED, BLOCKED, KILLED) znajdziesz w STATUS_TAXONOMY.md. Do DELIVERY wchodzi tylko pomysl w statusie READY.

Skalowanie w dol: 7 rol to odpowiedzialnosci, nie etaty. W malym zespole jedna osoba moze grac kilka rol po kolei (na przyklad BA plus SA plus Tech Lead jako jedno przejscie, potem implementacja). Skladaj i lacz role dowolnie, ale nie pomijaj outputu, za ktory dana rola odpowiada: ponizsze artefakty i bramki obowiazuja nawet wtedy, gdy jedna osoba produkuje je wszystkie.

---

# Reguly przekrojowe (obowiazuja kazda role)

1. **Sedno na gorze (TL;DR decyzyjne)** (ang. "Lead with the point (decision TL;DR)"). Kazdy artefakt otwiera blok: problem w 1-2 zdaniach, domyslna rekomendacja, mala liczba decyzji (z limitem, na przyklad najwyzej trzy), kazda oznaczona [rec.], oraz jawne "default-on-OK" (co sie dzieje, gdy decydent nic nie powie). Decydent orientuje sie na samej gorze, a nie po przeczytaniu calego dokumentu.
2. **Bramka recon (przed planowaniem).** Zanim cokolwiek zaprojektujesz, sprawdz, co JUZ istnieje w kodzie i produkcie. Nie buduj od nowa tego, co jest. Werdykt recon (istnieje / czesciowo / brak) to warunek wstepny otwierajacy prace architekta; spisuje go SA jako pierwszy output, wiec recon zarazem poprzedza prace SA i nalezy do SA. Bez recon ryzykujesz tydzien pracy nad czyms, co juz jest wdrozone.
3. **Najpierw reuse.** Najpierw lista "reuse bez zmian" i "reuse z modyfikacja", dopiero potem "nowe". Zamknij linijka "net new: N plikow / M endpointow" jako wejscie do estymacji. Reuse to glowna dzwignia trzymania kosztu nisko.
4. **Decyzje A vs B z jawnym kryterium i triggerem rewizji.** Kazda decyzja: plusy i minusy per opcja, co najmniej jedna opcja jawnie ODRZUCONA z powodem, linia "Kryterium decyzji" (os wyboru), "Rekomendacja" oraz "Trigger rewizji" (mierzalny warunek, ktory przywraca odrzucona opcje). Decyzja musi byc odtwarzalna.
5. **Footgun Register.** Projekt utrzymuje wlasna liste pulapek runtime w swoim stacku (rzeczy, ktorych kompilator i typy NIE wylapia, a ktore wybuchaja na produkcji). SA odhacza je per ficzer (dotyczy TAK / NIE / N.D. plus mitygacja). Lista rosnie po kazdym incydencie. To zastepuje poleganie na pamieci. Konkretne wpisy zyja w PROJECT_PROFILE.md, nie tutaj.
6. **Definition of Done per rola to bramka.** Output roli jest wejsciem kolejnej roli. Brakujacy element Definition of Done blokuje przekazanie. Jakosc wylapana wczesnie jest najtansza.
7. **Bramka release pre-flight.** Jedna binarna bramka przed wydaniem, ktora spina wszystkie warunki release'owe projektu (zobacz PROJECT_PROFILE.md). Brakuje ktoregokolwiek: nie wydajesz.
8. **Iteracja jest dozwolona (petle, nie tylko do przodu).** Domyslny przeplyw idzie do przodu (BA, dalej SA, Tech Lead, Backend, Frontend, QA, Security), ale delivery zawraca, gdy pozniejsza rola znajdzie luke. Sciezka wstecz jest jawna: jesli QA znajdzie niejednoznacznosc w acceptance criteria, temat wraca do BA; jesli Backend stwierdzi, ze architektura nie jest w stanie spelnic ktoregos AC, wraca do SA. Petla wstecz otwiera na nowo Definition of Done tej roli; poprawiony output plynie znow do przodu. Ten ruch wstecz odzwierciedla STATUS_TAXONOMY.md, gdzie element moze wrocic do wczesniejszego stanu (na przyklad ze STAGING z powrotem do DELIVERY przy porazce QA), a nie tylko isc do przodu.

Jesli budowe wykonujesz agentami LLM: dopasuj sile modelu do ryzyka kazdej fazy (praca prosta lub mechaniczna idzie na tanszy model; praca przy pieniadzach, security lub danych wrazliwych na najmocniejszy). Nie usredniaj wysilku po calym dokumencie. To adaptacja opcjonalna, nie wymog dla zespolow czysto ludzkich.

---

# Konwencja artefaktow i traceability

Ponizsze role wymieniaja, CO wyprodukowac. Uzywaj jednej zywej specyfikacji jako dokumentu-nosnika dla jednego ficzera, z jedna sekcja per rola, zamiast rozlacznych plikow per rola. Traceability propaguje sie przez nia: BA nadaje ID user stories (US-1, US-2) i acceptance criteria (US-1-AC-1); SA, Tech Lead, Backend i QA odwoluja sie do tych ID, dzieki czemu kazda linijke da sie przesledzic od testu do AC i dalej do user story. Output spec kazdej roli zawiera minimalny szkielet do wypelnienia, wiec artefakt da sie uzyc na zimno.

Konwencja estymacji: wybierz jedna jednostke dla calego ficzera i nazwij ja (na przyklad idealne dni inzynierskie, albo story pointy z podanym mapowaniem punkty-dni, albo rozmiary koszulkowe S/M/L z podanym zakresem). Tech Lead ustala jednostke na gorze planu, zeby wszystkie estymaty byly porownywalne.

---

# Sekwencja rol

## BA (Business Analyst)

**Misja:** Zamienic luzny problem w binarnie testowalny kontrakt: nazwana persona plus job-to-be-done, hipoteza wartosci z liczbowym baseline i targetem, atomowe acceptance criteria, traceability od user stories do AC oraz out-of-scope chroniacy przed scope creep. BA to pierwsza bramka twardych regul projektu.

**Output spec:**
- Problem i wartosc: 1-2 zdania problemu osadzone w realnym sygnale lub kodzie (nie w zgadywaniu); nazwana persona; JTBD "Kiedy [sytuacja], chce [motywacja], po to by [rezultat]".
- Hipoteza i metryka sukcesu: liczbowy baseline -> target -> okno czasowe; kontrmetryka (co NIE moze sie pogorszyc).
- User stories: As-a / I-want / So-that, ponumerowane, z oznaczeniem, co jest poza MVP.
- Acceptance criteria: schemat Given / When / Then, twarde progi i liczby (zadnych "np.", "itd.", "czytelne" ani niezdefiniowanego "N"; uzyj konkretnej wartosci albo "mala liczba"); jawny kod bledu dla walidacji domenowej.
- Przeplyw procesu: happy path plus stany brzegowe (juz zrobione / blad / wygasle / pominiete) plus punkty wejscia.
- Guardrail check: mini-checklista twardych regul projektu (TAK / NIE / N.D. z uzasadnieniem). BA bramkuje je NA WEJSCIU.
- Traceability: tabela US -> AC -> in-scope; zero osieroconych user stories.
- Decyzje dla decydenta, rozdzielone: (A) strategiczne lub nieodwracalne z rekomendacja; (B) domyslne progi juz wpisane w AC (decydent ma tylko veto).

Szkielet: `Problem | Persona | JTBD | Metryka (baseline -> target -> okno) | Kontrmetryka | US-n + AC-n (G/W/T) | Out-of-scope | Guardrail check | Tabela traceability | Otwarte decyzje`.

**Definition of Done:** persona plus JTBD obecne; metryka z baseline, targetem i oknem; kazde AC binarne (Given / When / Then, bez miekkich slow); guardrail check wypelniony; tabela traceability bez sierot; otwarte zostaly tylko realne decyzje strategiczne (progi maja defaulty).

**Anty-wzorce:** generyczny aktor bez persony; metryka bez liczb; AC z "np." albo "N"; user story bez AC i bez out-of-scope; spychanie egzekwowania twardych regul na pozniejsze role; mieszanie realnych decyzji z brakujacymi definicjami.

**Top dzwignie:** persona plus JTBD jako fundament, bo kazda kolejna rola buduje na tym, kto i po co; metryka z baseline, bo bez niej nikt nie domknie petli po release; guardrail check na wejsciu, bo twarde reguly najtaniej egzekwowac, zanim powstanie jakikolwiek projekt rozwiazania; atomowe AC, bo to jednostka, ktora QA testuje 1:1.

## SA (Solution Architect)

**Misja:** Zamienic problem w jedna decyzje architektoniczna gotowa do akceptacji bez rundy dopytan: recon (co juz istnieje), maksymalny reuse, rozbrojenie znanych runtime'owych footgunow projektu oraz produktowe bramki projektu (wstrzykniete z PROJECT_PROFILE.md, na przyklad parytet urzadzen, dostepnosc, akceptacja artefaktow AI). Decyzja odtwarzalna: kazda opcja z kryterium i triggerem rewizji.

**Output spec:**
- Recon: tabela symbol/endpoint -> gdzie szukales -> werdykt (istnieje / czesciowo / brak); jednozdaniowy werdykt, czy ficzer lub jego czesc juz istnieje. Recon to warunek wstepny z reguly przekrojowej 2 i to SA odpowiada za jego spisanie.
- Najpierw reuse: "reuse bez zmian" plus "reuse z modyfikacja" plus "net new: N".
- Decyzje Dx (A vs B): plusy i minusy, opcja odrzucona z powodem, kryterium plus rekomendacja plus trigger rewizji.
- Decyzja "minimalna powierzchnia dla v1": zanim dodasz jakikolwiek nowy stan trwaly, nowy modul lub nowa zaleznosc zewnetrzna, jawnie wazysz wariant odchudzony; ciezsza sciezka idzie do fazy 2 z triggerem.
- Footgun guard: tabela z Footgun Register projektu, per wpis TAK / NIE / N.D. plus mitygacja.
- Produktowe bramki projektu: guardraile wstrzykniete z PROJECT_PROFILE.md, odhaczone tutaj.
- Tabela komponentow: New / Mod / Reuse, spojna z recon (nic oznaczonego "New", co recon znalazl).

Szkielet: `Tabela recon | Werdykt w jednej linii | Listy reuse + net new | Decyzje Dx (kryterium/rec/trigger) | Decyzja o minimalnej powierzchni | Tabela footgun guard | Bramki produktowe | Tabela komponentow`.

**Definition of Done:** recon zrobiony; zero sprzecznosci miedzy recon a komponentami; footgun guard odhaczony; kazda decyzja ma kryterium, trigger i opcje odrzucona; minimalna powierzchnia rozwazona.

**Anty-wzorce:** projektowanie bez recon; "nowe" dla czegos, co istnieje; decyzja bez kryterium lub triggera; pomijanie footgunow; nadmiarowy nowy byt trwaly tam, gdzie wystarczyloby rozszerzenie.

**Top dzwignie:** bramka recon, bo zapobiega ponownemu budowaniu rzeczy juz wdrozonych; najpierw reuse, bo reuse to najtansza droga do dostarczenia; jawne kryterium plus trigger, bo czynia decyzje odtwarzalna i audytowalna; footgun guard, bo lapie to, czego typy i kompilator nie moga.

## Tech Lead

**Misja:** Rozbic decyzje SA na sekwencje wykonywalnych krokow z estymatami i kolejnoscia, nazwac ryzyka implementacyjne i punkty integracji oraz zdefiniowac release pre-flight. Tech Lead odpowiada za "jak to spiac, w jakiej kolejnosci i co moze pojsc nie tak".

**Output spec:**
- Plan krokow z zaleznosciami i estymata per krok, w ustalonej jednostce estymacji (zobacz konwencje artefaktow powyzej).
- Kolejnosc: co odblokowuje co, wyrazone prosta notacja zaleznosci (na przyklad ID krokow z "depends on").
- Ryzyka plus mitygacje: kazde nazwane ryzyko implementacyjne lub integracyjne z konkretna mitygacja.
- Kontrakty integracyjne: API, eventy i wspoldzielone typy zdefiniowane z gory, zeby praca rownolegla sie nie rozjechala.
- Checklista release pre-flight dla tego ficzera: konkretny podzbior warunkow release'owych projektu (z PROJECT_PROFILE.md), ktorych ten ficzer dotyka.
- Jawny reuse z nazwami z kodu (z jakich istniejacych symboli plan korzysta ponownie).

Szkielet: `Jednostka estymacji | Lista krokow (id, akcja, estymata, depends-on) | Sciezka krytyczna | Tabela ryzyk (ryzyko -> mitygacja) | Kontrakty integracyjne | Checklista pre-flight | Reuse po nazwach`.

**Definition of Done:** kroki atomowe z estymatami; sciezka krytyczna jest jasna; ryzyka nazwane z mitygacjami; checklista pre-flight kompletna.

**Anty-wzorce:** plan bez kolejnosci lub estymat; przemilczane ryzyka integracyjne; brak pre-flight; estymata ignorujaca reuse.

**Top dzwignie:** sciezka krytyczna, bo daje decydentowi te jedna liczbe, ktora sie liczy (dni do release); kontrakty integracyjne z gory, bo pozwalaja rownoleglym torom isc naprzod bez przerobek; pre-flight jako bramka, bo to jedyny punkt, ktory lapie luki blokujace release.

## Backend

**Misja:** Zaimplementowac logike domenowa i dane zgodnie z AC, z poprawnymi kodami bledow, idempotencja tam, gdzie ma znaczenie (kazda operacja, ktora nie moze zadzialac podwojnie, na przyklad ruchy pieniedzy lub handlery zdarzen zewnetrznych), i bez znanych footgunow danych ani migracji.

**Output spec:**
- Endpointy i serwisy zgodne z AC, powiazane z ID AC.
- Walidacja wejscia z poprawnym kodem bledu (nie myl autoryzacji z walidacja; nigdy nie zwracaj bledu autoryzacji przy porazce walidacji domenowej).
- Idempotencja dla operacji wrazliwych (guard plus unikalny klucz) wszedzie tam, gdzie powtorzenie nie moze zadzialac podwojnie.
- Bezpieczne migracje (spojne z Footgun Register projektu; odwracalne tam, gdzie to mozliwe).
- Testy jednostkowe krytycznej logiki.

**Definition of Done:** AC pokryte; kody bledow zgodne z kontraktem; operacje wrazliwe idempotentne; migracje odwracalne lub bezpieczne; testy zielone.

**Anty-wzorce:** zly kod bledu (na przyklad blad autoryzacji dla walidacji domenowej); brak idempotencji na jakiejkolwiek operacji podatnej na retry (webhooki zewnetrzne, callbacki platnosci i podobne to czeste przyklady, ale regula jest ogolna); migracja ignorujaca footguny; logika bez testow.

**Top dzwignie:** kontrakt kodow bledow, bo blednie zaklasyfikowany blad moze po cichu wylogowac uzytkownikow albo ukryc realna awarie; idempotencja, bo dostarczanie at-least-once to w realnych systemach stan domyslny; bezpieczne migracje, bo zla migracja to awaria najtrudniejsza do cofniecia na produkcji.

## Frontend

**Misja:** Zbudowac UI realizujace przeplyw z AC, mozliwe do ukonczenia na kazdym docelowym urzadzeniu, spojne z design systemem i wzorcami UX projektu, obslugujace stany brzegowe (loading / empty / error / offline).

**Output spec:**
- Komponenty zgodne z przeplywem, powiazane z ID AC.
- Pokrycie urzadzen: kazdy przeplyw mozliwy do ukonczenia na kazdym docelowym form factorze (konkretny cel, na przyklad najmniejszy wspierany ekran, jest wstrzykiwany z PROJECT_PROFILE.md).
- Stany brzegowe: loading, empty, error, offline.
- Spojnosc z prymitywami UI projektu i design systemem.
- Wybrane wzorce UX przywolane z uzasadnieniem (wskaz wzorzec, ktory wybrales, i dlaczego; nie narzucaj tu konkretnej biblioteki wzorcow, ten wybor zyje w PROJECT_PROFILE.md).
- Dostepnosc do progu ustalonego w projekcie.

**Definition of Done:** przeplyw mozliwy do ukonczenia na docelowych urzadzeniach; stany brzegowe obsluzone; spojnosc z design systemem; teksty dla uzytkownika prostym jezykiem (bez zargonu, bez technicznych ID).

**Anty-wzorce:** sciana desktop-only (przeplyw, ktorego nie da sie ukonczyc na mniejszym celu); brakujace stany empty lub error; natywne prymitywy zamiast design systemu; zargon w tekstach.

**Top dzwignie:** pokrycie urzadzen, bo przeplyw, ktory psuje sie na docelowym form factorze, jest dla tych uzytkownikow de facto niewydany; stany brzegowe, bo empty i error to miejsca, w ktorych realni uzytkownicy laduja najpierw; spojnosc z design systemem, bo to ona trzyma produkt w calosci, gdy ten rosnie.

## QA

**Misja:** Binarnie udowodnic, ze AC sa spelnione, na kazdym docelowym urzadzeniu, wlacznie ze stanami brzegowymi i sciezkami bledow. QA mapuje testy 1:1 na AC.

**Output spec:**
- Macierz test -> AC (kazde ID AC ma co najmniej jeden test).
- Przypadki: happy plus edge plus error.
- Test pokrycia urzadzen przed release (docelowe urzadzenia pochodza z PROJECT_PROFILE.md).
- Dane testowe per istotna os wariantow (na przyklad rola, plan, locale, jakiekolwiek warianty produkt faktycznie ma).
- Raport, co przeszlo, a co nie.

**Definition of Done:** kazde AC ma przypadek testowy; pokrycie urzadzen sprawdzone; sciezki bledow zweryfikowane (poprawne kody); brak otwartych blockerow.

**Anty-wzorce:** test tylko happy path; pominiete pokrycie urzadzen; brak mapowania na AC; "u mnie dziala" zamiast macierzy.

**Top dzwignie:** mapowanie test -> AC, bo zamienia "przetestowalismy" w dowodliwe pokrycie; bramka pokrycia urzadzen, bo awaria miedzy urzadzeniami to najczestsza pozna niespodzianka; sciezki bledow, bo to w nich kryja sie niepoprawne kody i ciche awarie.

## Security

**Misja:** Zweryfikowac autoryzacje, ochrone danych (zwlaszcza wrazliwych lub osobowych), rezydencje danych tam, gdzie ma zastosowanie, oraz powierzchnie naduzyc. Security to ostatnia bramka przed wydaniem ryzykownych powierzchni.

**Output spec:**
- Kontrola autoryzacji per endpoint (kto moze co zrobic).
- Klasyfikacja danych plus ochrona: sklasyfikuj kazdy przeplyw danych wedlug poziomu wrazliwosci, potem zastosuj ochrone adekwatna do poziomu (szyfrowanie, anonimizacja, rezydencja) tam, gdzie ma zastosowanie.
- Powierzchnie naduzyc: rate limiting, fraud, enumeracja.
- Spojnosc kodow bledow (nigdy nie ujawniaj autoryzacji jako walidacji ani odwrotnie).
- Zgodnosc z obowiazujacym rezimem regulacyjnym (rezim nazwij w PROJECT_PROFILE.md; playbook pozostaje neutralny co do tego, ktory obowiazuje).

**Definition of Done:** autoryzacja zweryfikowana; dane wrazliwe chronione zgodnie ze swoja klasa; powierzchnie naduzyc pokryte; brak sciezki eskalacji uprawnien.

**Anty-wzorce:** autoryzacja, ktora "ufa frontendowi"; wysylanie danych wrazliwych do serwisow zewnetrznych bez podstawy; brak rate limitu na powierzchniach publicznych.

**Top dzwignie:** autoryzacja per endpoint, bo jedna brakujaca kontrola to pelne naruszenie; klasyfikacja danych, bo ochrona ma sens dopiero wtedy, gdy wiesz, co niesie kazdy przeplyw; powierzchnie naduzyc, bo publiczne endpointy sa sondowane automatycznie od pierwszego dnia.

## Weryfikacja po release (domkniecie petli)

Metryki sukcesu i kontrmetryki zdefiniowanej przez BA nie domyka QA (ktore dowodzi jedynie spelnienia AC przed release). Jedna rola musi byc wlascicielem weryfikacji po release: observability i logowanie na miejscu, monitoring bledow dziala, trigger rollbacku zdefiniowany oraz zaplanowane sprawdzenie, czy metryka sukcesu faktycznie drgnela, a kontrmetryka sie nie pogorszyla. Przypisz te odpowiedzialnosc jawnie (czesto Tech Lead lub Backend). Bez tego petla otwarta przez BA nigdy sie nie domyka, a wartosc ficzera pozostaje nieudowodniona. To mapuje sie na status VERIFY w STATUS_TAXONOMY.md.

---

# Bramki

- **Bramka 0 (Recon):** SA udowodnil, co juz istnieje; zero projektowania od zera rzeczy juz wdrozonych.
- **Bramka per rola (Definition of Done):** output roli jest kompletny, zanim zostanie przekazany dalej.
- **Bramka Release (pre-flight):** wszystkie warunki release'owe projektu odhaczone (konkretna lista w PROJECT_PROFILE.md), wydawana jest tylko praca scommitowana, plus weryfikacja na srodowisku przejsciowym (staging), jesli takie istnieje. Odpowiada to wejsciu w STAGING, a potem SHIPPED w STATUS_TAXONOMY.md.
- **Definition of Done na poziomie ficzera (akceptacja release):** ficzer nie jest skonczony, dopoki weryfikacja po release nie potwierdzi, ze metryka sukcesu BA zostala sprawdzona, a kontrmetryka sie nie pogorszyla (zobacz Weryfikacja po release powyzej).

# Tory rownolegle plus sciezka krytyczna

Rozpisz, ktore role i kroki moga biec rownolegle, a ktore siedza na sciezce krytycznej. Podaj jedna liczbe dni dla sciezki krytycznej i flage "blokuje release: tak/nie" per tor. To realizuje "rownolegle zamiast szeregowo" i daje decydentowi najwazniejsza liczbe.

---

# Adaptacja do twojego projektu

Playbook jest generyczny. Aby go uzyc, wstrzyknij specyfike swojego projektu BEZ edytowania rdzenia:
- **Twarde reguly projektu:** lista regul, ktore BA bramkuje na wejsciu, a SA potwierdza (na przyklad konwencje API, cele pokrycia urzadzen, reguly prywatnosci, akceptacja artefaktow AI, biblioteka wzorcow UX, preferencje undo-vs-confirm). To twoj "guardrail check".
- **Footgun Register:** lista pulapek runtime w twoim stacku, rosnaca po kazdym incydencie. Zasila "footgun guard" u SA.
- **Release pre-flight:** konkretne kroki deployu twojego projektu. Zasilaja Bramke Release. Czeste przyklady do wyliczenia tutaj: wymagane zmienne srodowiskowe ustawione przed deployem, bump wersji przy kazdym deployu, synchronizacja lockfile z zaleznosciami, rejestracja encji i migracji w warstwie persystencji oraz kazdy inny warunek, ktorego pominiecie wywraca release.
- **Design system / wzorce UX / prog dostepnosci:** zasilaja role Frontend i QA, w tym konkretne docelowe form factory (na przyklad najmniejszy wspierany ekran) i wybrana biblioteke wzorcow UX.
- **Poziomy klasyfikacji danych i rezim regulacyjny:** poziomy wrazliwosci i obowiazujace ramy prawne, ktore zasilaja role Security.
- **Persony i metryki:** z twojego Discovery lub Briefu (zobacz DISCOVERY_PLAYBOOK.md), zasilaja BA.
- **Jednostka estymacji:** ustal jednostke (idealne dni, story pointy albo rozmiary koszulkowe), zeby estymaty byly porownywalne miedzy ficzerami.

Trzymaj te rzeczy w osobnym pliku projektu (na przyklad PROJECT_PROFILE.md), a ten playbook zostaw czysty i wspolny dla wszystkich projektow.

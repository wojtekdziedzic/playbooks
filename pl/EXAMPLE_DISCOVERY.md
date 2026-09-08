# Przyklad roboczy: pelny przebieg Discovery Sequence (fikcyjny)

> Tlumaczenie. Kanonem jest `../EXAMPLE_DISCOVERY.md` w roocie repo. Zsynchronizowane z rootem: 2026-09-09. Gdy root sie zmienia, ta data jest jedynym sygnalem, ze tlumaczenie odstaje: w razie rozbieznosci obowiazuje root.

To jest PRZYKLAD DYDAKTYCZNY. Kazda liczba, persona, cytat i konkurent ponizej sa WYMYSLONE. Pokazuje, jak przeprowadzic Discovery Sequence od poczatku do konca na zmyslonym produkcie, tak aby bylo widac, co wytwarza kazda z 8 rol, jak rozstrzygaja sie bramki i jak zapada werdykt Go/No-Go. To NIE jest prawdziwy produkt, NIE sa to prawdziwe dane i nie ma zadnego zwiazku z zadnym rzeczywistym projektem. Uzywaj go razem z DISCOVERY_PLAYBOOK.md (metoda) i STATUS_TAXONOMY.md (statusy).

Przyklad jest celowo pouczajacy: NIE konczy sie czystym Go. Konczy sie werdyktem WEAK z zaprojektowanym fake-door RAT i jawna regula persevere / pivot / kill, bo taka sciezka uczy metody lepiej niz Go przyklepane z automatu. Wypelniony szablon Brief pokazano na koncu jako artefakt, ktory POWSTALBY, gdyby RAT przeszedl.

---

## Fikcyjne tlo

Produkt: "DeskHive", SaaS dla malych i srednich przestrzeni coworkingowych do zarzadzania rezerwacjami hot deskow (czlonkowie rezerwuja biurko na dzien lub kilka godzin z telefonu).

Pomysl poddawany Discovery: "Smart Desk Swap": gdy czlonek, ktory zarezerwowal biurko, nie zamelduje sie w ciagu 20 minut od startu swojego slotu, DeskHive automatycznie zwalnia to biurko z powrotem do dostepnej puli i (opcjonalnie) powiadamia czlonkow z listy oczekujacych, ze wlasnie zwolnilo sie biurko.

Fikcyjny profil projektu uzyty w tym przebiegu (tego typu rzecz mieszkalaby w PROJECT_PROFILE.md):
- North star: utrzymane platne miejsca (miesiecznie aktywne placace przestrzenie, ktore przedluzaja subskrypcje).
- Dzwignie outcome w grze: retencja (operatorzy przestrzeni przedluzaja), aktywacja (czlonkowie faktycznie rezerwuja).
- Zrodla sygnalu: zdarzenia rezerwacji w aplikacji, telemetria no-show, czaty supportowe operatorow, 3 wywiady z operatorami.
- Segmenty person: "Operator" (prowadzi przestrzen, nasz placacy klient) i "Czlonek" (rezerwuje biurka, nie jest bezposrednio naszym klientem).
- Wybrany tani test popytu: kafelek fake-door w dashboardzie operatora.

---

## PDL (Product Discovery Lead): orkiestracja

Ustawia ramy tak, zeby tor pozostal tani i zbiegl do binarnej decyzji.

- Opportunity Solution Tree:
  - Outcome (szczyt): retencja placacych przestrzeni.
  - Szansa: operatorzy traca zaufanie do DeskHive, gdy oplacone biurka stoja puste, a czlonkowie sa odsylani z kwitkiem ("biurka widma").
  - Pomysl: Smart Desk Swap (auto-zwalnianie no-show + powiadomienie listy oczekujacych).
  - Eksperyment (lisc): kafelek fake-door mierzacy intencje operatorow, by to wlaczyc.
- Dokladnie jedno najbardziej ryzykowne zalozenie (nazwane, wlasnosc PDL): "Operatorzy WLACZA automatyczne zwalnianie biurek, godzac sie na to, ze DeskHive anuluje w ich imieniu oplacona rezerwacje czlonka." Wszystko inne (tresc powiadomien, okno 20 minut, mechanika listy oczekujacych) jest drugorzedne.
- Time-box: jeden cykl Discovery = 5 dni roboczych. RAT, jesli uruchomiony, maks 10 dni na zywo.
- Kontrola petli: maks 2 iteracje na tym pomysle przed eskalacja do twardej decyzji (kill, park z warunkiem albo akceptacja nazwanego ryzyka rezydualnego).
- Stany bramek na starcie: wszystkie OPEN.

---

## PS (Product Strategist): problem i outcome

Ramuje bol, a nie ficzer, i podpina go pod dzwignie.

- Sformulowanie problemu (bol): "Operatorzy widza, jak sprzedane przez nich biurka stoja puste w godzinach szczytu, bo czlonkowie no-show nigdy nie zwalniaja swojej rezerwacji, wiec przestrzen wyglada w aplikacji na pelna, choc na sali sa wolne miejsca, a czlonkowie z ulicy odchodza z kwitkiem."
- Powiazanie z outcome (mechanizm): retencja. Puste, ale zarezerwowane biurka podwazaja u operatora wiare w dokladnosc narzedzia; operatorzy, ktorzy przestaja ufac danym o oblozeniu, odchodza przy odnowieniu. Odzyskiwanie biurek widm podnosi uzyteczna pojemnosc i odbudowuje zaufanie.
- Obecne obejscie: operatorzy recznie obchodza sale, wypatruja puste biurka i wlasnorecznie zwalniaja je w panelu admina (jedna fikcyjna operatorka mowi, ze robi to "trzy albo cztery razy w ruchliwy poranek").
- Czestotliwosc i dotkliwosc: poranne szczyty, 2 do 5 biurek widm na ruchliwy dzien na przestrzen sredniej wielkosci; dotkliwosc srednio-wysoka (utracony przychod z klientow z ulicy + nadszarpniete zaufanie), ale istnieje reczne obejscie, co sygnalizuje, ze bol jest realny, choc czesciowo zaabsorbowany.
- Anty-cel: to NIE rozwiazuje overbookingu, cennika ani dlugoterminowego przypisania biurek. Odzyskuje wylacznie sloty no-show.

Auto-atak: reczne obejscie, ktore "zajmuje tylko minute", to zolta flaga dla gotowosci do adopcji automatyzacji. Zgloszone do PRC i SKEPTIC.

---

## UXR (User Researcher): JTBD i sygnaly

Oddziela to, co operatorzy mowia, od tego, co robia.

- Persona: "Operator" (zarzadza przestrzenia na 60 do 120 biurek, nietechniczny, w porannym szczycie zyje w dashboardzie).
- JTBD: "Kiedy moja przestrzen wyglada w aplikacji na pelna, a ja widze puste biurka na sali, chce, zeby system automatycznie zwalnial no-show, tak zebym mogl sadzac klientow z ulicy bez osobistego pilnowania kazdego biurka."
- Diagram sil:
  - Push (od dzisiejszego stanu): reczne obchodzenie sali jest meczace i podatne na bledy w godzinach szczytu.
  - Pull (ku Smart Desk Swap): automatyczne odzyskiwanie, mniej niankowania.
  - Nawyk: operatorzy juz ufaja wlasnym oczom i recznemu przyciskowi zwalniania; to dziala.
  - Lek: "a co, jesli anuluje czlonka, ktory wlasnie parkowal samochod?" Strach przed rozgniewaniem placacego czlonka.
  - Ocena: pull + push sa realne, ale lek jest wysoki, a nawyk zakorzeniony. Wypadkowa sil NIE jest jednoznacznie dodatnia. To jest sedno.
- Tabela dowodow:
  | Sygnal | Zrodlo | Sila | Sugeruje |
  |---|---|---|---|
  | Telemetria no-show: ~7% zarezerwowanych biurek nigdy sie nie melduje | zdarzenia rezerwacji w aplikacji (behawioralne) | silny | problem biurek widm jest realny i mierzalny |
  | "Obchodze sale, zeby zwalniac biurka" | 2 z 3 wywiadow z operatorami (deklaratywne) | sredni | reczne obejscie istnieje i jest uzywane |
  | "Nigdy nie pozwolilbym, zeby software anulowal czlonka beze mnie" | 1 z 3 wywiadow (deklaratywne) | slaby, ale glosny | lek adopcyjny wokol auto-anulowania |
- Mowi vs robi: operatorzy MOWIA, ze problem pustych biurek boli (spojnie), ale NIE ROBIA jeszcze niczego, co dowodzi, ze oddaliby decyzje o anulowaniu softwareowi (luka behawioralna).
- Sanity probki: 3 wywiady, saturacja NIE osiagnieta (trzeci wywiad wciaz wydobyl nowa obiekcje). Baza 3 to hipoteza, a nie dowod, dla pytania o adopcje. WSKAZNIK no-show to solidne dane behawioralne; GOTOWOSC DO AUTOMATYZACJI juz nie.

Auto-atak: silny sygnal (7% no-show) odpowiada na pytanie "czy jest problem", a nie "czy to zautomatyzuja". To rozne pytania; nie pozwol, zeby jedno legitymizowalo drugie.

---

## MKT (Market and Competition Analyst)

Osadza pomysl na tle alternatyw i fosy.

- Krajobraz:
  - Bezposredni konkurenci (fikcyjni): "DeskFlow" i "SpotBee" oferuja rezerwacje hot deskow; zaden nie zwalnia dzis no-show automatycznie.
  - Prawdziwe obejscie (faktyczny konkurent): wlasne oczy operatora + reczny przycisk zwalniania. Darmowe, zaufane, zero ryzyka rozgniewania czlonka.
- Luka: zaden gracz nie automatyzuje odzyskiwania no-show; bol jest przez operatorow uznawany, ale nieobsluzony przez software.
- Dopasowanie do fosy: wzmacnia rdzen (dokladnosc oblozenia to argument sprzedazowy DeskHive), a nie zwykle wyrownanie do konkurencji. Dobre dopasowanie.
- Okno czasowe: raczej teraz, ale nie pilne. Zaden konkurent tego nie wypuszcza; z nikim sie nie scigamy. Okno to "wkrotce", bo zaufanie do danych o oblozeniu jest naszym wyroznikiem, ale nikt nie przystawia nam do glowy pistoletu pierwszego gracza.
- Klasyfikacja konkurencyjna (soczewka pozycji rynkowej): wyroznik (nikt inny tego nie ma).

Auto-atak: "nikt inny tego nie robi" moze oznaczac niewykorzystana luke ALBO to, ze operatorzy tak naprawde nie chca, by software podejmowal decyzje o anulowaniu. MKT nie umie tych dwoch rzeczy rozroznic; musza to zrobic PRC i EXP.

---

## PRC (Pricing and Behavioral Analyst): gotowosc do placenia plus Kano

Odpowiada na pytanie "czy to jest tego warte" i czy zachowanie faktycznie sie zmieni.

- Klasyfikacja Kano: performance dla OPERATORA (wiecej odzyskanych biurek = wieksza satysfakcja, skaluje sie z tym, jak dobrze to dziala). Ryzykuje bycie indifferent, a nawet reverse dla CZLONKA (czlonek, ktoremu auto-anulowano rezerwacje, jest niezadowolony). Dwustronne, a strona czlonka moze odbic rykoszetem.
- Sygnal gotowosci do placenia: behawioralnie na razie zaden. Operatorzy nie placa dzis ekstra za reczny przycisk, a obejscie jest "darmowe". Nie istnieje zaden cenowy dowod gotowosci. Do walidacji w EXP.
- Wartosc vs naklad (jeden pomysl, proste punktowanie): wartosc srednio-wysoka (dotyka rdzeniowej dzwigni zaufania); naklad sredni (detekcja no-show + auto-zwalnianie + powiadomienie listy oczekujacych + ustawienie opt-in operatora + komunikat o anulowaniu widoczny dla czlonka).
- Kontr-koszt (ogon, nie sama budowa): obciazenie supportu od rozzloszczonych auto-anulowanych czlonkow; konfigurowalne przez operatora okno karencji do utrzymania; przypadki brzegowe (czlonek melduje sie w 21. minucie). Ten ogon jest nietrywialny.
- Lekki test wykonalnosci: brak oczywistego blokera. Zdarzenia check-in i czasy slotow juz istnieja w modelu danych; auto-zwalnianie to zaplanowany job. Gleboka wykonalnosc odlozona do SA w Delivery.

Auto-atak: klasyczna pulapka delighter-bez-must-have ominieta (to performance, podpiete pod realna dzwignie), ale rykoszet po stronie czlonka i nieobecny sygnal gotowosci do placenia pozostaja nierozstrzygniete. Nie pozwol, by srednio-wysoka wartosc przykryla zerowy dowod popytu na ZACHOWANIE adopcyjne.

---

## DATA (Analytics): baseline, metryka, instrumentacja

Przybija liczby i pre-rejestruje prog RAT.

- Baseline (z realnego zrodla, tutaj fikcyjny): wskaznik no-show = 7,0% zarezerwowanych slotow biurek w ostatnich 30 dniach (zdarzenia rezerwacji w aplikacji). Zalogowane reczne zwolnienia: ~38% tych no-show jest ostatecznie zwalnianych recznie; pozostale ~62% stoi puste do konca slotu.
- Metryka sukcesu (binarnie sprawdzalna): z biurek widm NIE zwalnianych dzis recznie odzyskiwac automatycznie co najmniej 50% w ciagu 6 tygodni od wlaczenia Smart Desk Swap, bez wzrostu skarg czlonkow. Baseline 0% auto-odzysku -> cel 50% -> okno 6 tygodni.
- Kontr-metryka (co nie moze spasc): retencja rezerwacji czlonkow. Jesli auto-anulowanie sprawia, ze czlonkowie rezerwuja mniej (albo skarza sie czesciej), ficzer oblewa, nawet jesli odzyskuje biurka. Twardy sufit: wskaznik skarg czlonkow nie moze wzrosnac o wiecej niz +1 na 100 rezerwacji.
- Instrumentacja RAT: kafelek fake-door w dashboardzie operatora. Mierzymy: klikniecia intencji wlaczenia, a z nich ukonczenie kroku potwierdzenia ("Tak, pozwol DeskHive auto-anulowac no-show w mojej przestrzeni").
- Pre-rejestrowany prog sukcesu RAT (kontrakt przekazany EXP): >= 30% operatorow, ktorzy WIDZA kafelek, klika "Wlacz", ORAZ >= 50% klikajacych konczy krok potwierdzenia. Ponizej tego = zalozenie adopcyjne nie jest wsparte.
- Sanity probki: hybryda jakosciowo-behawioralna. Obronialna, jesli kafelek zostanie pokazany >= 40 odrebnym kontom operatorow w oknie testu (zadeklarowane minimum, a nie roszczenie do p-value).

Auto-atak: klikniecie "Wlacz" to intencja, a nie realne zachowanie zycia z auto-anulowaniami przez tygodnie. Prog mierzy apetyt, nie trwalosc; to ograniczenie jest nazwane, nie ukryte.

---

## EXP (Experiment Designer): RAT

Projektuje najtanszy test, ktory moze ZABIC pomysl, uzywajac pre-rejestrowanego progu od DATA.

- Najbardziej ryzykowne zalozenie (falsyfikowalne, od PDL): "Operatorzy wlacza automatyczne zwalnianie biurek, godzac sie na to, ze DeskHive anuluje w ich imieniu rezerwacje placacego czlonka."
- Wybrana metoda: kafelek fake-door / painted-door w dashboardzie operatora. Najtansza adekwatna metoda, bo otwartym pytaniem jest INTENCJA ADOPCJI, a te kafelek mierzy przy zerowym backendzie (bez schedulera, bez powiadomien, bez zbudowanego flow po stronie czlonka).
- Dlaczego nie taniej: rozmowa juz sie odbyla (3 wywiady) i dala sygnal glosny, ale cienki; potrzebujemy behawioralnego kliku od wielu operatorow, a to daje kafelek. Dlaczego nie drozej: zbudowanie najpierw prawdziwego auto-zwalniania byloby dokladnie tym marnowaniem dni, przed ktorym Discovery ma chronic.
- Setup: jeden kafelek w dashboardzie "Smart Desk Swap (nowosc): auto-zwalnianie biurek no-show" z krotka linia wartosci i przyciskiem "Wlacz"; klikniecie otwiera modal potwierdzenia; potwierdzenie pokazuje uczciwy komunikat: "Dziekujemy. Smart Desk Swap nie jest jeszcze aktywny. Badamy zainteresowanie i wyslemy e-mail, gdy bedzie gotowy." Zadne ustawienie nie jest faktycznie przelaczane.
- Budzet per test (uzgodniony przed startem): maks 10 dni na zywo, maks 1 dzien pracy inzyniera na budowe kafelka + modala + logowania zdarzen. Jesli nie da sie tego zbudowac w tym budzecie, to nie jest fake-door.
- Regula decyzyjna (spisana PRZED testem, zwiazana z progiem DATA):
  - PERSEVERE (Go): >= 30% operatorow, ktorzy widza kafelek, klika Wlacz ORAZ >= 50% z nich potwierdza. Apetyt adopcyjny wsparty; przechodzimy do Brief i Delivery.
  - PIVOT: klikniecia Wlacz przekraczaja 30%, ale potwierdzenie sie zalamuje (lek przed anulowaniem czlonkow zabija na modalu). Zmiana zakresu na "zasugeruj zwolnienie, operator zatwierdza jednym tapnieciem" (human-in-the-loop) i ponowny bieg.
  - KILL: < 15% klika Wlacz. Operatorzy tego nie chca; park albo kill, bez ponownego argumentowania.
  - (Miedzy 15% a 30%, albo niejednoznaczny srodek: tylko jedna dodatkowa iteracja, zgodnie z limitem 2 od PDL.)
- Czego test NIE waliduje: trwalosci uzycia przez tygodnie, wskaznika skarg po stronie czlonkow, wlasciwej dlugosci okna karencji, tresci powiadomien.
- Bariera uczciwosci: fake-door nie bierze pieniedzy, niczego nie przelacza, a po kliknieciu pokazuje jasny komunikat "jeszcze nie aktywne". Zaufanie zostaje zachowane.

Auto-atak: galaz pivot to prawdziwa wartosc tego RAT; jesli potwierdzenie sie zalamie, to jest odkrycie, a nie porazka.

---

## SKEPTIC (Red-team): adwersarz

Domyslnie stawia No-Go i zmusza reszte do obalenia tego.

- Atak na dowod: jedyny SILNY sygnal (7% no-show) dowodzi problemu, a nie adopcji rozwiazania. Sygnal adopcyjny opiera sie na 3 wywiadach, z ktorych jeden glosno protestowal. Probka za cienka, by twierdzic, ze jest popyt na auto-anulowanie. Zarzut stoi.
- Atak na popyt: alternatywne wyjasnienie dla "operatorzy tego chca": chca ZWOLNIONYCH PUSTYCH BIUREK, niekoniecznie SOFTWARE'U decydujacego o anulowaniu. Jednotapowy prompt "zwolnic tego no-show?" moglby zaspokoic ta sama prace bez zadnego leku. Deklarowane zainteresowanie moze dotyczyc rezultatu, a nie tego mechanizmu.
- Atak na wartosc: kontr-koszt niedowazony. Auto-anulowanie placacego czlonka, ktory spoznil sie 2 minuty, to granat w zaufanie; obciazenie supportu i churn czlonkow moga przewyzszyc zysk z odzyskanych biurek.
- Atak na fose: realna, ale "zaden konkurent tego nie ma" moze znaczyc, ze rynek juz sie nauczyl, ze czlonkowie nienawidza auto-anulowania.
- Pre-mortem ("wdrozylismy, poleglo, dlaczego", 3 przyczyny):
  1. Czlonkowie buntuja sie przeciw niespodziewanym anulowaniom; operatorzy wylaczaja to w ciagu tygodnia.
  2. Operatorzy w ogole tego nie wlaczaja (lek > apetyt); ficzer wychodzi do zerowej adopcji.
  3. Okno 20 minut jest zle dla wiekszosci przestrzeni i nie ma dobrego uniwersalnego domyslnego; ciagle skargi.
- Decydent vs uzytkownik: to wyglada na cos, czego chce OPERATOR WPATRZONY W DASHBOARD (schludne oblozenie), ale CZLONEK, ktoremu anulowano, jest cichym przegranym. Pilnuj ryzyka dwustronnosci.
- Werdykt: WEAK. Nie KILL (problem jest realny, a tani test moze to rozstrzygnac), nie SURVIVES (zachowanie adopcyjne jest niedowiedzione, a jeden wywiad aktywnie protestowal). Dokladny dowod, ktory przelaczylby WEAK na SURVIVES: fake-door RAT przekraczajacy pre-rejestrowany prog DATA (>= 30% klikniec Wlacz ORAZ >= 50% potwierdzen), ALBO behawioralny sygnal, ze operatorzy juz gdzie indziej wlaczaja automatyzacje w stylu auto-anulowania.

---

## Stany bramek i werdykt Go/No-Go

| Bramka | Stan | Dlaczego |
|---|---|---|
| Bramka 0: dopasowanie do outcome | PASS | Podpina sie pod retencje z jawnym mechanizmem (zaufanie do danych o oblozeniu napedza odnowienia). |
| Bramka 1: dowod | PASS | Wskaznik no-show 7% to silny sygnal behawioralny, ze problem biurek widm jest realny. |
| Bramka 2: popyt | OPEN | Popyt na TEN mechanizm (auto-anulowanie) jest niedowiedziony; fake-door RAT jest zaprojektowany, by go zmierzyc. |
| Bramka 3: oplacalnosc | OPEN | Wartosc srednio-wysoka, ale kontr-koszt (rykoszet u czlonkow) i gotowosc do placenia pozostaja nierozstrzygniete do czasu RAT. |
| SKEPTIC | WEAK | Problem realny, zachowanie adopcyjne niedowiedzione; nazwano dokladny dowod, ktory to przelacza. |

WERDYKT: NEED-MORE (WEAK). Ani Go, ani No-Go. Pomysl przechodzi do statusu RAT zgodnie ze STATUS_TAXONOMY.md, niosac ze soba nazwane najbardziej ryzykowne zalozenie i zaprojektowany test fake-door. Bramki Popytu i Oplacalnosci pozostaja OPEN, dopoki RAT nie zwroci liczby wzgledem pre-rejestrowanego progu DATA.

Co dzieje sie dalej, wedlug galezi (zdecydowane z gory, a nie po wyniku):
- RAT przekracza prog -> WEAK przelacza sie na SURVIVES, wszystkie bramki PASS, werdykt staje sie Go, ponizszy Brief zostaje wypelniony, a pomysl przechodzi do READY.
- RAT zalicza klikniecia Wlacz, ale oblewa potwierdzenie -> PIVOT do wariantu human-in-the-loop "zatwierdz zwolnienie", jedna powtorka (w ramach limitu 2 iteracji od PDL).
- RAT < 15% klikniec Wlacz -> KILL (nie wraca bez nowego sygnalu) albo PARK z wyzwalaczem "wroc, jesli wskaznik no-show przekroczy 12% albo 3+ operatorow poprosi o auto-zwalnianie bez pytania".

---

## Brief do Delivery (pokazany wypelniony, warunkowo, jesli RAT przejdzie)

To jest artefakt, ktory WYPRODUKOWALABY galaz Go. Pokazane liczby to ilustracyjny wynik zaliczenia; sa fikcyjne.

```
## Brief Discovery do Delivery: Smart Desk Swap
Data: 2026-06-23   Werdykt: GO

PROBLEM (bol uzytkownika, nie ficzer): Operatorzy traca przychod z klientow z ulicy i zaufanie, bo czlonkowie no-show nigdy nie zwalniaja zarezerwowanych biurek, wiec aplikacja pokazuje komplet, podczas gdy biurka stoja puste.
PERSONA + JTBD: Operator, "Kiedy moja przestrzen wyglada na pelna, a ja widze puste biurka na sali, chce, zeby no-show byly zwalniane automatycznie, tak zebym mogl sadzac klientow z ulicy bez pilnowania kazdego biurka."
OUTCOME (dzwignia): retencja. Odzyskiwanie biurek widm odbudowuje zaufanie do danych o oblozeniu, co napedza odnowienia placacych przestrzeni.
DOWOD POPYTU: fake-door RAT: 41% operatorow, ktorzy widzieli kafelek, kliknelo Wlacz; 63% z nich potwierdzilo auto-anulowanie (oba wyniki powyzej pre-rejestrowanego progu 30% / 50%).
METRYKA SUKCESU: auto-odzysk biurek widm niezwalnianych recznie 0% -> 50% w ciagu 6 tygodni od wlaczenia.  |  KONTR-METRYKA: wskaznik skarg czlonkow nie moze wzrosnac o wiecej niz +1 na 100 rezerwacji.
WARTOSC vs KOSZT: Kano=performance (strona operatora)  ocena=srednio-wysoka  naklad~sredni (detekcja + job auto-zwalniania + powiadomienie listy oczekujacych + ustawienie opt-in + komunikat anulowania dla czlonka)
DOPASOWANIE DO FOSY: rdzen (wyroznik zaufania do danych o oblozeniu)   OKNO: teraz (zaden konkurent tego nie ma; wzmacnia nasz argument sprzedazowy)
NAJBARDZIEJ RYZYKOWNE ZALOZENIE (zwalidowane): "operatorzy wlacza sterowane software'em auto-anulowanie placacych czlonkow": potwierdzone przez fake-door przekraczajacy oba pre-rejestrowane progi.
GRANICE / POZA ZAKRESEM v1: bez logiki overbookingu, bez cennika, bez dlugoterminowego przypisania biurek; okno karencji sztywno 20 min w v1 (konfigurowalne pozniej).
GUARDRAILE DLA BUDOWY (juz znane): anulowanie widoczne dla czlonka musi byc uczciwe i odwracalne w oknie karencji; nigdy nie anuluj bez powiadomienia czlonka; opt-in per przestrzen, domyslnie OFF (zrodlo: regula dwustronnego zaufania z PROJECT_PROFILE.md).
WERDYKT SKEPTIC: SURVIVES, fake-door przekroczyl oba pre-rejestrowane progi i galaz pivot nie byla potrzebna; kontr-metryka po stronie czlonkow idzie do Delivery jako twardy guardrail.
```

---

## Czego ten przyklad ma nauczyc

1. Silny sygnal (7% no-show) dowodzi PROBLEMU; nie dowodzi ADOPCJI wybranego mechanizmu. Discovery trzyma te dwa pytania osobno.
2. Glosna obiekcja z pojedynczego wywiadu wystarcza, by bramka Popytu pozostala OPEN, ale nie wystarcza do KILL. Wlasciwy ruch to tani test, a nie klotnia.
3. DATA pre-rejestruje prog; EXP konsumuje go bez zmian; regula decyzyjna (persevere / pivot / kill) jest spisana PRZED testem. Zadnego przesuwania slupkow po wyniku.
4. Werdykt WEAK to produktywny wynik: nazywa dokladny dowod, ktory go przelacza, kieruje pomysl do RAT (a nie do mglistego backlogu) i ustawia limit 2 iteracji, zeby nie mogl mielic w nieskonczonosc.
5. Brief wypelnia sie tylko przy Go i niesie kontr-metryke oraz guardraile projektu dalej, tak zeby Delivery odziedziczylo ograniczenia, a nie tylko ficzer.

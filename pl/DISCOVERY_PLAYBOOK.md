# Playbook sekwencji Discovery: maksimum z kazdej roli

> Tlumaczenie. Kanonem jest `../DISCOVERY_PLAYBOOK.md` w roocie repo. Zsynchronizowane z rootem: 2026-09-09. Gdy root sie zmienia, ta data jest jedynym sygnalem, ze tlumaczenie odstaje: w razie rozbieznosci obowiazuje root.

Generyczny, niezalezny od projektu. Uzywaj go do kazdej analizy typu "czy budowac i co budowac", zanim cokolwiek trafi do delivery. Specyfike projektu (outcome, termin, zrodla sygnalow, reguly, persony) wstrzykuj w sekcji "Adaptacja do twojego projektu", nie w rdzeniu.

Czym to jest: tor UPSTREAM, ktory odpowiada na dwa binarne pytania, zanim cokolwiek trafi do Delivery: (1) czy problem jest realny i czego uzytkownik naprawde oczekuje, (2) czy warto to budowac teraz. Discovery zabija slaby pomysl NAJTANSZYM eksperymentem (godziny), zanim delivery spali dni albo tygodnie.

Relacja do Delivery: Discovery konczy sie ZANIM zacznie sie projektowanie rozwiazania. Wynik Go to Brief, ktory staje sie wejsciem dla BA w sekwencji Delivery (zob. DELIVERY_PLAYBOOK.md). Wynik No-Go to zaparkowanie z warunkiem powrotu. Nic nie wchodzi do Delivery bez Go.

Slownik statusow: ten tor tworzy pomysly i przesuwa je miedzy statusami zdefiniowanymi w STATUS_TAXONOMY.md (IDEA, DISCOVERY, RAT, READY, PARKED, KILLED). Traktuj ten plik jako jedyne zrodlo nazw statusow i dozwolonych przejsc.

Przyklad wypelniony: w EXAMPLE_DISCOVERY.md znajdziesz w pelni wypelniony przebieg sekwencji wraz z wynikowym Brief.

---

# Sekwencja: mapa

Role: 7 rol sekwencyjnych plus PDL jako ciagly orkiestrator = lacznie 8.

Orkiestracja przez caly czas: PDL (Product Discovery Lead).

1. PS (Product Strategist): problem plus outcome (do czego sie przyczepia).
2. UXR (User Researcher): JTBD plus sygnaly (co uzytkownik robi, a nie mowi).
3. MKT (Market and Competition Analyst): kto juz to robi, czego brakuje, jakie jest okno.
4. PRC (Pricing and Behavioral Analyst): czy ktos za to zaplaci albo bedzie tego uzywal, Kano, sklonnosc do placenia.
5. DATA (Analytics): baseline plus metryka sukcesu plus instrumentacja testu, oraz zarejestrowany z gory prog sukcesu RAT.
6. EXP (Experiment Designer): RAT, najtanszy test, ktory MOZE zabic pomysl, oparty na progu zarejestrowanym z gory przez DATA.
7. SKEPTIC (Red-team): adwersarz, probuje zabic pomysl i kazda sekcje powyzej.

Bramki (binarne, blokujace):
- Bramka 0: dopasowanie do outcome. Pomysl przyczepia sie do jednego mierzalnego outcome z jawnym mechanizmem wplywu. Brak = zaparkowanie.
- Bramka 1: dowod. Problem ma dowod (sygnal albo zachowanie), a nie tylko "tak mi sie wydaje".
- Bramka 2: popyt. Istnieje sygnal popytu (behawioralny bije deklarowany) albo zaprojektowany RAT, ktory go zmierzy.
- Bramka 3: oplacalnosc. Wartosc vs naklad jest rozstrzygnieta, koszt jest wart slotu.
- Bramka Go/No-Go: decyzja binarna. Go daje Brief do Delivery. No-Go daje zaparkowanie z warunkiem powrotu.

Rekurencja: tor nie jest jednorazowy. To petla (zob. "Petla ciagla"). Pomysl moze przechodzic przez nia wielokrotnie, az ktoras bramka da twarde Go albo trwale No-Go.

---

# Role

## PDL (Product Discovery Lead): orkiestracja

Misja: utrzymac tor TANIM i ZBIEZNYM: jeden outcome, jedno najbardziej ryzykowne zalozenie, jasna sciezka do decyzji binarnej. Wlasciciel Opportunity Solution Tree i bramki Go/No-Go. Chroni czas zespolu: godzina discovery musi byc tansza niz godzina zmarnowanego budowania.

Specyfikacja wyniku: Opportunity Solution Tree (od outcome przez szanse i pomysly do eksperymentow); dokladnie jedno nazwane najbardziej ryzykowne zalozenie; time-box cyklu; stany bramek (PASS / FAIL / OPEN z powodem).

Definition of Done: OST z outcome na szczycie i bez osieroconych pomyslow; dokladnie jedno najbardziej ryzykowne zalozenie; ustawiony time-box; Go/No-Go jest binarne i ma nazwanego wlasciciela; ustalona regula kontroli petli (maksymalna liczba iteracji przed eskalacja, zob. "Petla ciagla").

Anty-wzorce: paraliz analityczny; pomysl bez outcome; piec rownorzednych "ryzyk" zamiast jednego; projektowanie rozwiazania (to robota Delivery); pozwalanie, by pomysl mielil sie w petli bez limitu iteracji.

Top dzwignie: jeden outcome plus jedno najbardziej ryzykowne zalozenie; time-box, ktory wymusza decyzje; Go/No-Go jako jedyne wyjscie do Delivery.

## PS (Product Strategist): Problem i Outcome

Misja: zamienic pomysl w ostro zdefiniowany PROBLEM (nie rozwiazanie), przyczepiony do mierzalnego outcome. Pilnuje, by nie projektowac rozwiazania, zanim problem zostanie nazwany.

Specyfikacja wyniku: sformulowanie problemu (bol uzytkownika, nie ficzer); powiazanie z outcome (jawny mechanizm wplywu: akwizycja / aktywacja / retencja / przychod / zgodnosc z przepisami); obecny workaround (co uzytkownik robi dzis zamiast tego); czestotliwosc i dotkliwosc; anty-cel (czego pomysl NIE rozwiazuje).

Definition of Done: problem ujety jako bol, nie rozwiazanie; jawny mechanizm wplywu na outcome; opisany realny workaround; oszacowana dotkliwosc.

Anty-wzorce: problem przebrany za rozwiazanie; outcome typu "uzytkownik bedzie zadowolony" bez dzwigni; pominiety workaround (tani workaround sygnalizuje niska sklonnosc do placenia).

Top dzwignie: problem jako bol; jawny mechanizm wplywu; workaround jako przyblizenie sklonnosci do placenia.

## UXR (User Researcher): JTBD i sygnaly

Misja: ustalic, czego uzytkownik NAPRAWDE oczekuje, oddzielajac to, co mowi, od tego, co robi. Formalizuje Job-To-Be-Done i kopie w realnych sygnalach, zamiast je wymyslac. Przy malej bazie uzytkownikow celuj w glebokie sygnaly jakosciowe i behawioralne, nie w ankiety.

Specyfikacja wyniku: nazwana persona; JTBD w formie "Kiedy [sytuacja], chce [motywacja], aby [outcome]"; diagram sil (push / pull / habit / anxiety, z ocena, czy pull + push > habit + anxiety); tabela dowodow (sygnal / zrodlo / sila / co sugeruje, z co najmniej 1 sygnalem niedeklaratywnym albo jawnym "brak dowodu = hipoteza dla RAT"); wyrazny podzial "mowi vs robi". Sanity check proby jakosciowej: podaj, ile bylo sesji lub wywiadow i czy osiagnieto saturacje (nowe sesje przestaja wnosic nowe watki); baza 1 lub 2 to hipoteza, nie dowod.

Definition of Done: persona plus JTBD wyprowadzone z sygnalu, nie z zalozenia; wypelnione sily; co najmniej 1 sygnal behawioralny albo jawna hipoteza dla RAT; sprawdzona luka mowi/robi; okreslona wielkosc proby jakosciowej i stwierdzona jej wystarczalnosc.

Anty-wzorce: ankieta na garstce uzytkownikow traktowana jak dane; deklaracja "uzywalbym tego" traktowana jak dowod popytu; JTBD zza biurka; pytania sugerujace; ogloszenie watku na podstawie jednego wywiadu.

Top dzwignie: zachowanie bije deklaracje; diagram sil (czy uzytkownik zmieni nawyk); JTBD z sygnalu.

## MKT (Market and Competition Analyst)

Misja: osadzic pomysl na rynku: kto juz to robi (produkty plus workaroundy), czego brakuje, jakie jest nasze okno. Pilnuje, by nie scigac sie na parity tam, gdzie nie mozemy wygrac, i kieruje ku wzmacnianiu realnego moat.

Specyfikacja wyniku: krajobraz (produkty plus realne workaroundy); luka (czego brakuje albo czego uzytkownicy nienawidza); moat-fit (czy wzmacnia rdzen, czy to tylko parity); okno czasowe (teraz vs pozniej, plus powod); klasyfikacja konkurencyjna (must-have parity / performance / differentiator / nice-to-have).

Uwaga o klasyfikacji: klasyfikacja MKT to soczewka pozycji rynkowej (gdzie to sie plasuje wzgledem konkurencji). Etykiety Kano od PRC to soczewka satysfakcji uzytkownika (jak uzytkownicy reaguja na obecnosc lub brak). To dwie odrebne osie, nie ta sama skala; pomysl moze byc rynkowym differentiatorem (MKT), a zarazem delighterem Kano albo wrecz indifferent (PRC). Czytaj je razem, nie sklejaj jednej w druga.

Definition of Done: krajobraz z uwzglednieniem workaroundow; nazwana luka z dowodem; ocena moat-fit; rozstrzygniete okno.

Anty-wzorce: budowanie parity na silnym polu zasiedzialego gracza; ignorowanie workaroundu; "zbudujmy to, bo oni to maja" bez moat-fit.

Top dzwignie: workaround jako realny konkurent; moat-fit; okno czasowe.

## PRC (Pricing and Behavioral Analyst): sklonnosc do placenia plus Kano

Misja: odpowiedziec "czy to sie OPLACA": czy ktos zaplaci, czy zachowanie rzeczywiscie sie zmieni i jaki jest naklad wzgledem zwrotu. Klasyfikuje wg Kano i wazy wartosc vs naklad. Tnie pomysly, ktore sa przyjemne, ale nie ruszaja ani portfela, ani zachowania.

Specyfikacja wyniku: klasyfikacja Kano (must-have / performance / delighter / indifferent) z uzasadnieniem; sygnal sklonnosci do placenia (albo "brak, do walidacji w EXP"); wartosc vs naklad (prosty scoring, albo RICE przy rankowaniu wielu pomyslow); kontrkoszt (utrzymanie, wsparcie, zlozonosc, nie tylko budowa).

O nakladzie i wykonalnosci: PRC szacuje naklad z perspektywy cenowej i behawioralnej, a nie glebokiej wykonalnosci technicznej. Zrob tu lekki check wykonalnosci (czy istnieje oczywisty bloker, ktory czyni pomysl niepraktycznym albo znacznie drozszym, niz wyglada). Gleboka analize wykonalnosci technicznej odloz do SA w Delivery; nie pozwol, by Go padlo na pomysl z oczywista sciana wykonalnosci.

RICE, gdy uzywane: Reach (ilu osob dotyczy w danym okresie), Impact (jak mocno rusza outcome w pojedynczym przypadku), Confidence (jak pewne sa dane wejsciowe), Effort (czas osobowy). Wynik = Reach x Impact x Confidence / Effort. Dla pojedynczego pomyslu uzywaj prostego scoringu wartosc vs naklad; po RICE siegaj tylko przy rankowaniu kilku pomyslow wzgledem siebie.

Definition of Done: etykieta Kano; sygnal sklonnosci do placenia albo jawna hipoteza; policzona wartosc vs naklad; policzony kontrkoszt utrzymania; zrobiony lekki check wykonalnosci albo gleboka wykonalnosc jawnie odlozona do SA.

Anty-wzorce: delighter, gdy nie ma must-have; "wszyscy to pokochaja" bez sygnalu sklonnosci do placenia; naklad liczony tylko jako budowa, bez ogona utrzymaniowego; gold-plating.

Top dzwignie: Kano (kolejnosc); sklonnosc do placenia bije entuzjazm; kontrkoszt utrzymania.

## DATA (Analytics): baseline plus metryka plus instrumentacja

Misja: przygwozdzic liczby: baseline z realnego stanu, metryka sukcesu z celem i oknem, kontrmetryka oraz sposob instrumentacji RAT. Bez DATA discovery konczy sie na "chyba dziala".

Specyfikacja wyniku: baseline (liczba z realnego zrodla, nie z pamieci); metryka sukcesu (od baseline przez cel do okna, sprawdzalna binarnie); kontrmetryka; instrumentacja RAT (jak bedziemy mierzyc, z progiem sukcesu ustalonym PRZED testem); sanity proby (statystyczna vs jakosciowa, nazwana jawnie; dla jakosciowej nazwij, co czyni probe obronialna, na przyklad osiagnieta saturacja albo zadeklarowana minimalna liczba sesji).

Przekazanie do EXP: DATA rejestruje z gory prog sukcesu RAT (liczbe, ktora przelacza persevere / pivot / kill). EXP konsumuje dokladnie ten prog w swojej regule decyzyjnej. To kontrakt: EXP nie moze wymyslac ani przesuwac progu po fakcie.

Definition of Done: baseline z realnego zrodla; metryka sprawdzalna binarnie; istnieje kontrmetryka; RAT ma metode pomiaru i prog ustalony przed startem; sanity proby sprawdzone, a przy probie jakosciowej stwierdzona jej wystarczalnosc.

Anty-wzorce: zgadywany baseline; metryka bez celu lub okna; sam sukces, bez kontrmetryki; udawanie statystyki na malenkiej probie; definiowanie pomiaru po tescie.

Top dzwignie: baseline z danych; kontrmetryka; prog sukcesu ustalony przed testem.

## EXP (Experiment Designer): RAT (Riskiest Assumption Test)

Misja: zaprojektowac NAJTANSZY eksperyment, ktory moze ZABIC pomysl, zanim powstanie choc jedna linia kodu produkcyjnego. Bierze najbardziej ryzykowne zalozenie, wybiera najtansza metode z drabinki i z gory definiuje persevere / pivot / kill w oparciu o prog zarejestrowany z gory przez DATA.

Drabinka tanich testow (od najtanszego): rozmowa z uzytkownikiem; fake-door / painted door (kafelek albo CTA mierzacy intencje, zero backendu); klikalna makieta; strona landing lub cennik (gdy pytanie dotyczy sklonnosci do placenia); concierge / Wizard of Oz (zrob recznie, zanim zautomatyzujesz).

Specyfikacja wyniku: najbardziej ryzykowne zalozenie (falsyfikowalne); wybrana metoda (i dlaczego jest najtansza z adekwatnych); setup (co stawiamy, czas i koszt); budzet per test (maksymalny czas i koszt uzgodnione przed startem, zeby "tani" test nie zamienil sie po cichu w budowe); regula decyzyjna ustalona PRZED testem (persevere / pivot / kill wzgledem progu DATA); czego test NIE waliduje; guardrail uczciwosci (fake-door nie pobiera pieniedzy i nie niszczy zaufania; po kliknieciu pokaz jasny komunikat).

Definition of Done: test celuje w najbardziej ryzykowne zalozenie; najtansza adekwatna metoda; budzet czasu i kosztu per test ustalony przed startem; regula decyzyjna spisana przed startem, powiazana z progiem zarejestrowanym z gory przez DATA; nazwane granice testu; guardrail uczciwosci na miejscu.

Anty-wzorce: test, ktory nie moze obalic pomyslu; walidowanie latwego zalozenia zamiast najbardziej ryzykownego; budowanie "malego MVP", gdy wystarczylby fake-door; regula decyzyjna spisana po wyniku; fake-door, ktory niszczy zaufanie; "tani" test bez sufitu kosztow, ktory puchnie w budowe.

Top dzwignie: fake-door (najtanszy walidator popytu); regula decyzyjna przed testem; concierge przed automatyzacja.

## SKEPTIC (Red-team): adwersarz

Misja: probowac ZABIC pomysl i podwazyc kazda sekcje powyzej. Domyslnie mowi "No-Go" i zmusza reszte, by to obalila. Chroni przed zakochaniem sie we wlasnym pomysle.

Specyfikacja wyniku: atak na dowod (zachowanie czy deklaracja, wielkosc proby, bias pytan sugerujacych); atak na popyt (alternatywne wyjasnienie sygnalu); atak na wartosc (niedoszacowany naklad, kontrkoszt); atak na moat; pre-mortem ("wyszlo na produkcje, poleglo, dlaczego", z 3 przyczynami); jawne "czy tego chce decydent, czy uzytkownik"; werdykt KILL / WEAK / SURVIVES z uzasadnieniem.

Skala werdyktow i powrot: KILL = fatalna wada bez taniego srodka zaradczego, zaparkuj albo zabij pomysl. WEAK = konkretna luka jest nieudowodniona, nazwij dokladnie, jaki nowy dowod przelaczylby ja na SURVIVES (na przyklad zdany RAT wzgledem progu DATA albo sygnal behawioralny, ktorego brakuje UXR). SURVIVES = na kazdy atak odpowiedziano dowodem. Przy ponownym wejsciu w petle WEAK zmienia sie w SURVIVES tylko wtedy, gdy ten nazwany dowod sie pojawi, a nie przez ponowne przekonywanie.

Definition of Done: kazda sekcja dostala konkretny atak; pre-mortem z 3 przyczynami; sprawdzone "decydent vs uzytkownik"; jednoznaczny werdykt; przy werdykcie WEAK nazwany dokladny dowod, ktory by go przelaczyl.

Anty-wzorce: sceptycyzm na pokaz; atakowanie tylko slabych sekcji; brak pre-mortem; werdykt WEAK bez wskazanej sciezki do SURVIVES.

Top dzwignie: domyslne "No-Go" do obalenia; pre-mortem; "czego chce decydent vs czego chce uzytkownik".

---

# Bramka: Go/No-Go

Go tylko wtedy, gdy WSZYSTKIE bramki maja PASS i SKEPTIC = SURVIVES. Trzy wyjscia:
1. Go: powstaje Brief do Delivery (ponizej), a pomysl przechodzi do READY zgodnie ze STATUS_TAXONOMY.md.
2. No-Go (zaparkowanie): z jawnym warunkiem powrotu ("wracamy, gdy [warunek]"). Bez warunku pomysl NIE wraca (przeciw puchnieciu backlogu). To status PARKED w STATUS_TAXONOMY.md.
3. Need-more (WEAK): wraca do EXP po mocniejszy RAT; time-box i limit iteracji PDL pilnuja, zeby nie zrobila sie z tego niekonczaca petla.

# Szablon: Brief do Delivery

```
## Brief z Discovery do Delivery: <nazwa>
Data: <YYYY-MM-DD>   Werdykt: GO

PROBLEM (bol uzytkownika, nie ficzer): <1 zdanie>
PERSONA + JTBD: <persona>, "Kiedy ..., chce ..., aby ..."
OUTCOME (dzwignia): <akwizycja / aktywacja / retencja / przychod + mechanizm>
DOWOD POPYTU: <sygnal behawioralny / wynik RAT z liczba>
METRYKA SUKCESU: <baseline -> cel -> okno>  |  KONTRMETRYKA: <co nie moze spasc>
WARTOSC vs KOSZT: Kano=<must-have / performance / delighter / indifferent>  score=<...>  naklad~<...>
MOAT-FIT: <rdzen / parity>   OKNO: <teraz / pozniej + powod>
NAJBARDZIEJ RYZYKOWNE ZALOZENIE (zwalidowane): <jakie bylo, jak je obalono, wzgledem progu DATA>
GRANICE / POZA ZAKRESEM v1: <co NIE wchodzi>
GUARDRAILE DLA BUDOWY (juz znane): <reguly projektu dotykajace tego ficzera; zrodlo: sekcja Adaptacja do twojego projektu / PROJECT_PROFILE.md>
WERDYKT SKEPTIC: SURVIVES, <co przekonalo>
```

# Petla ciagla

Discovery to nie jednorazowa bramka, to cykl:
1. Sygnaly z danego okresu (rozmowy, zachowania uzytkownikow, bledy, zgloszenia) zasilaja inbox szans.
2. Triage (PDL): ktoremu outcome to sluzy, czy jest warte cyklu; wieksze pomysly dostaja najbardziej ryzykowne zalozenie.
3. Prowadz 1 lub 2 tanie RAT-y rownolegle z biezaca budowa (dwa tory, nie kolejka).
4. Go/No-Go: Go daje Brief; No-Go parkuje z warunkiem powrotu.
5. Okresowy przeglad parkingu: warunek spelniony oznacza powrot na tor, inaczej pomysl dalej spi.

Rekurencyjne "maksimum z kazdej roli": kazda rola konczy samoatakiem na wlasna sekcje; SKEPTIC atakuje w poprzek wszystkich sekcji; tor krazy, az ktoras bramka da twarde Go albo trwale No-Go. Kontrola petli: PDL ustala maksymalna liczbe iteracji na pomysl (na przyklad 3); po uderzeniu w limit bez twardego Go eskaluj do decyzji (zabij, zaparkuj z warunkiem albo zaakceptuj nazwane ryzyko rezydualne), zamiast mielic dalej.

---

# Adaptacja do twojego projektu

Wstrzyknij specyfike (BEZ edytowania rdzenia):
- Outcome / north star: jeden mierzalny cel, do ktorego PS przyczepia problemy.
- Zrodla sygnalow: skad UXR czerpie zachowania i feedback (analityka, rozmowy, support, bledy).
- Reguly projektu: co zasila pole "guardraile dla budowy" w Brief.
- Persony: realne segmenty twoich uzytkownikow.

Trzymaj to w osobnym pliku projektu (np. PROJECT_PROFILE.md). Ten playbook zostaw czysty i wspolny dla wszystkich projektow.

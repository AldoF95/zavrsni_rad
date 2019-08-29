Uvod
====

Strojno učenje je tehnologija koja je sve prisutnija u današnjem
svijetu: predviđanje vremena, burzovnih cijena, klasifikacija kupca,
itd. Strojno učenje je primjenjivo u svim industrijama iz kojih se mogu
formatizirati izvorni podaci. Na raspolaganju za izradu ovog završnog
rada su podaci iz hotelijerske industrije te kao ciljni zadatak je
predvidjeti ako će rezervacija biti izbrisana. U upravljanju prihoda,
brisanje rezervacija ima veliki utjecaj na krajnju zaradu te
posjedovanje informacije u naprijed ako će rezervacija biti izbrisana
ili ne omogućava poduzimanja postupaka u sprječavanju ili vođenju istih
na adekvatan način. Takvi postupci mogu dovesti na sveukupni porast
prihoda, što je krajnji cilj bilo kojeg poduzeća. Ispitivanje provedeno
2011. godine od Kimesa [@Kimes2011], pokazalo je da 24.6% ispitanika (od
kojih 78.4% su iz hotelijerske industrije) misli da u slijedećih 5
godina tehnologija će imati veliku ulogu u upravljanju prihodima, a
17.8% su rekli da prognoziranje i analitičke metode će također imati
veliki utjecaj. U slijedećim poglavljima se obrađuju metode strojnog
učenja i, suvremenija pod-kategorija, dubokog učenja: popraćeno sa kodom
obrađuje se od prikupljanja podataka i njihovog čišćenja do krajnjeg
rezultata prognoziranja.

Pregled literature
==================

Upravljanje prihodima je izvorno razvijeno 1966. godine u avionskoj
industriji, tek kasnije uvedena u drugim industrijama poput
hotelijerstva, ugostiteljstvo, casinima itd. Značajan broj radova je
izvedeno na temi predviđanje potražnje, međutim samo nekoliko njih
([@Huang2013; @Antonio2018; @Falk2018; @Leeuwen2018; @Morales2008; @Nuno2017; @Antonio2019])
se koncentriraju specifično na metodologiji ovog završnog rada
[@Antonio2018]. Kao što se može primijetiti u tablici
[\[previousWork\]](#previousWork){reference-type="ref"
reference="previousWork"}, svi radovi koriste fokusirane podatke nad
pojedinim hotelima i svi dostupni podaci su ispod 300 000 zapisa, sa
iznimkom rada Koolea, Hopmana i Leeuwena [@Leeuwen2018] koji imaju bazu
podataka veću od milijun zapisa međutim izvor nisu hoteli nego
ugostiteljske nekretnine sa kapacitetom soba ne većom od dva. Korišteni
podaci su PNR vrste (eng. Passenger Name Record): izraz koji potjeće iz
avijonske industrije te kasnije preuzet u hotelijerku industriju kao
definicija podataka rezervacija; PNR podaci obuhvačaju informacije o
korisniku, tko će putovati ili prespavati prema rezervaciji, pojedinosti
usluge, cijena i slično [@Sokel2002]. Korištene metode predviđanja
variraju: korištena metoda ovisi o vrsti i veličini podataka na
raspolaganju, pa tako i metode variraju od strojnog učenja i dubokog
učenja. Iz tablice
[\[previousWork\]](#previousWork){reference-type="ref"
reference="previousWork"} se vidi da najčešći korišteni model je stablo
odlučivanja i njegove varijacije: Boosted decision tree, XGBoost, Random
forest. U ovom završnom radu će se koristiti model XGBoost-a, te
opravdanja i razlozi odabira tog modela se obrazlažu u slijedećem
poglavlju.

  ---------------- ------------------------ ------------ ------------- ---------------- ----- -------
      **Lit.**     **Metoda**               **Godina**   **Dataset**   **Br. hotela**         
      podataka                                                                                
       (min)                                                                                  
    [@Nuno2017]    Boosted decision tree    2017         73K           4                Da    0.879
   [@Antonio2018]  XGBoost                  2017         N/A           2                Da    0.84
    [@Huang2013]   BPN GRNN                 2013         N/A           N/A              N/A   0.808
    [@Falk2018]    N/A                      2018         233K          9                Da    0.92
   [@Morales2008]  C4.4 RndForest SVM KLR   2009         240K          1                Ne    N/A
   [@Leeuwen2018]  RndForest                2018         1.27M         Not Hotels (7)   Da    0.89
   [@Antonio2019]  XGBoost                  2019         100K          8                Da    0.777
  ---------------- ------------------------ ------------ ------------- ---------------- ----- -------

[\[previousWork\]]{#previousWork label="previousWork"}

Pošto su podaci podijeljeni po hotelima, odnosno nisu jednoobrazni za
bilo koji hotel, tako su i rezultati istraživanja: dobivena preciznost i
točnost modela vrijedi samo ta specifičan hotel. Baza podatak na
raspolaganju za ovaj završni rad ima 661 857 zapisa od 26 hotela koji se
razlikuju po veličini i kvaliteti: u model ulaze svi podaci, ne
razdvojeni po hotelima, što ujednačuje rezultat na sve hotele. Morales i
Wang [@Morales2008] predlažu dva modela podataka: sezonski prosjek i PNR
podaci. Iako je prvi jako popularan u praksi, PNR podaci su dokazali da
donose bolje rezultate. Podaci na raspolaganju je kombinacija od dva
modela: količina osobnih podataka gosta je svedena na minimum,
ostavljeni su samo podaci za koje se misli da mogu utjecati na završni
rezultat kao što je država porijekla, prisutnost djece itd. Također
treba pripaziti na načinu rezervacije. Nove tehnologije dovode do
stvaranja novih usluga: u ovom slučaju su se razvile online putničke
agencije (eng. Online Travel Agencies, OTAs) što značajno olakšavaju
proces rezerviranja te brisanje iste. Falk i Vieru [@Falk2018] su
dokazali da rezervacije napravljene preko online agencija imaju veću
stopu brisanja nego ostale rezervacije.

Metoda strojnog učenja
======================

Ovo predstavlja klasifikacijski problem, stoga se koristi nadzirani
klasifikacijski algoritam. Strojevi podržanog vektora (eng. Support
Vector Machines), Stablo odlučivanja (eng. Decision Trees), Logistička
regresija (eng. Logistic Regression) itd. su svi poznati algoritmi za
klasifikaciju. Kako bi se odabralo najprikladniji algoritam koji će dati
najbolje moguće rezultate, napravila se funkcija za usporedbu modela.
Usporedba se napravila na uzorku od 5000 nasumičnih zapisa te provjerena
sa 10-strukom unakrsnom validacijom. U tablici
[\[table1\]](#table1){reference-type="ref" reference="table1"} su
prikazani F1-rezultati usporedbe u padajućem redoslijedu: F1-rezultat
predstavlja odnos između preciznosti i odaziva, te kao takav se smatra
dobrim pokazateljem kvalitete modela. Tablica
[\[table1\]](#table1){reference-type="ref" reference="table1"} pokazuje
da algoritam XGBoost ima najbolji rezultat, te on je korišten u
modeliranju modela za predikciju brisanja rezervacija.

  **Model**       **F1-rezultat**
  -------------- -----------------
  `SVC-linear`     0.578  0.137
  `SVC-rbf`        0.678  0.001
                   0.692  0.016
                   0.697  0.016
                   0.708  0.008
  XGBoost          0.768  0.012

  : Rezultati usporedbe algoritma[]{label="table1"}

Povećanje gradijenta (eng. Gradient Boosting) radi na način da završno
predviđanje sastavi od puno slabijih modela predviđanja, te na svakoj
interakciji novi slabi klasifikator se nadoda na prijašnji model na
način da ispravlja grešku. Ekstremno povećanje gradijenta (eng. Extreme
Gradient Boosting, XGBoosting) radi na sličan način ali rezultat je
točniji jer kontrolira pre-fittanje te je procesorski efikasiniji pošto
koristi algoritam za paralelizaciju stabla [@Gupta2015]. Kao i kod
svakog algoritma temeljenom na stablu odlučivanja, tako i kod
XGBoosting-a je najveći problem odrediti strukturu stabla: postoje puno
kombinacija stabala te pronaći najoptimalniju može zahtijevati veliku
procesorsku snagu [@Xia2017]. U tu svrhu, XGBoosting koristi pohlepan
algoritam uveden od Chena i Guestrina [@Chen1994], \"Osnovno točni
pohlepni algoritam\" (eng. \"Basic exact greedy algorithm\"): prvo
sortira podatke prema vrijednostima atributa a zatim posjećuje
vrijednosti kako bi sakupio gradijentnu statistiku za ocijeniti
strukturu prema jednadžbi
[\[equation01\]](#equation01){reference-type="ref"
reference="equation01"}.

$$\label{equation01}
    G=\frac{1}{2}
    \bigg[
    \frac{(\sum_{i\epsilon I_L} g_i)^2}{\sum_{i\epsilon I_L} h_i+\lambda}+
    \frac{(\sum_{i\epsilon I_R} g_i)^2}{\sum_{i\epsilon I_R} h_i+\lambda}-
    \frac{(\sum_{i\epsilon I} g_i)^2}{\sum_{i\epsilon I} h_i+\lambda}
    \bigg] - 
    \gamma$$

Gdje g~i~ i h~i~ predstavljaju prvi i drugi redoslijed gradijenta na
funkciji gubitka; I~L~ i I~R~ označavaju skupove uzoraka lijeve i desne
grane stabla; $\lambda$ je konstanta te $\gamma$ je parametar
kompleksnosti. Algoritma se zaustavlja kada G$<$0, te najveći G označuje
optimalno grananje na čvoru [@Xia2017].

Za pisanje algoritama i modeliranje XGBoosting-a se koristila knjižnica
Scikit-learn: prikladna je za nadzirane i ne nadzirane probleme srednjih
veličina, raspolaže sa jednostavnim sučeljem sa svrhom dovođenja
prednosti strojnog učenja ljudima koji ne raspolažu takvim predznanjem,
odnosno za ne profesionalce [@Pedregosa2011].

Metoda dubokog učenja
=====================

Duboko učenje je podpolje strojnog učenja koji ima kompleksniju
strukturu i temelji se na umjetnim neuronskim mrežama: mreže su
sastavljene od više ne linearnih skrivenih razina, gdje rezultat svake
razine predstavlja ulaz slijedećoj razini [@Yu2011]. Neuronska mreža je
paralelna, distribuirana struktura obrade informacija koja obrađuje
elemente međusobno povezanim jednosmjernim signalnim kanalima
[@Neilsen1992]. Dubinsko učenje se u većini slučajeva koristi za
procesiranje podataka na ljudskoj razini, kao na primjer prepoznavanje
slika i govora. Međutim može se aplicirati i za jednodimenzionalne
ulazne podatke kao što je u ovom slučaju: ulaz u mrežu je
jednodimenzionalni vektor koji predstavlja jedan zapis. Zbog
komplicirane strukture mreža, potrebna je veća procesorska snaga za
treniranje modela. Dodatni problem predstavlja definiranje parametra
mreža: teško je precizno definirati prikladnu dubinu mreže i ostale
parametre kao što su stopa učenja ili broj ciklusa. Kao takve, dubinske
neuronske mreže su dobar kandidat za hiper-parametarsko pretraživanje
koje će se razmatrati u 6. poglavlju. Kao i u slučaju algoritma strojnog
učenja, uspoređuje se tri klasifikacijski nadzireni algoritmi dubinskog
učenja te provjereni sa 10-strukom unakrsnom validacijom nad uzorku od
5000 zapisa: gusta neuronska mreža (eng. Dense Neuron Network, DNN),
rekurzivna neuronska mreža (eng. Recursive Neuron Network, RNN) te
konvolucijska neuronska mreža (eng. Convolutional Neuron Network, CNN).
Prema tablici [\[DeepModels\]](#DeepModels){reference-type="ref"
reference="DeepModels"} DNN daje najbolji rezultat te kao takav se
koristio za slijedeća testiranja u sklopu dubinskog učenja.

  **Model**    **accuracy**
  ----------- ---------------
  DNN          0.696  0.0005
  RNN          0.632  0.132
  CNN          0.688  0.021

  : Usporedba dubinskih modela[]{label="DeepModels"}

DNN predstavlja jednu od jednostavnijih mreža iz skupine algoritama
dubinskog učenja: sastavljene su od niza elemenata, tzv. neuroni, koji
uzimaju ulaz i težinski faktor nad konekciji koji varira kako bi se
smanjio rezultat određene funkcije gubitka [@Farre2018]. Pohrana ulaznih
podataka se razvija slijedno, odnosno širenje u naprijed se izvršava
nivo po nivo, bez preskakanja. Osobnost DNNa je što svaki neuron je
spojen sa svakim neuronom slijedeće skrivene razine, što znači da sa
svakom dodatnom razinom struktura i vrijeme treniranja postaju
zahtjevniji. Jedan od izazova korištenja neuronskih mreža je što se
smatraju algoritmi crne kutije: iako se poznaje način rada neurona,
veliki broj konekcija i razina predstavlja problem za interpretirati
unutarnji rad istih [@Farre2018].

$$\label{reluFunction}
    F(x) = max(0, x) = \Big\{{{ 0, x<0}\atop{x, x \geq 0}}$$

U tablici [\[DNNmodel\]](#DNNmodel){reference-type="ref"
reference="DNNmodel"} je prikazana struktura, odnosno skrivene razine,
neuronske mreže: sve razine su guste vrste; veličine razina se postepeno
smanjuje prema izlaznoj razini koja ima 7 izlaznih neurona, odnosno
odgovara sa mogućim brojem kategorija; aktivacijske funkcije su 'relu'
(funkcija [\[reluFunction\]](#reluFunction){reference-type="ref"
reference="reluFunction"}), osim izlazne razine koja ima aktivacijsku
funkciju 'softmax' za određivanje predviđene kategorije.

  ----------------- ----- ---------
   **Vrsta sloja**        
   (broj neurona)         
      funkcija            
        Dense        512    relu
        Dense        256    relu
        Dense        128    relu
        Dense        128    relu
        Dense        128    relu
        Dense        32     relu
        Dense         7    softmax
  ----------------- ----- ---------

  : Struktura DNN modela[]{label="DNNmodel"}

Analiza podataka
================

U kreiranju predicijskih modela, velik utjecaj ima vrsta podataka na
raspolaganju, njihova čistoća i odabir prikladnih atributa. Kod
modeliranja, veliki dio vremena se provodi upravo na tom procesu:
čišćenju i odabiru podataka. Za ovaj završni rad na raspolaganju je
dataset od anonimnih rezervacija od 26 hotela kroz razdoblje od tri
godine (2016, 2017, 2018); od kuda proizlaze 661 857 rezervacija. Za
svrhu analize podataka se koristila Python knjižnica Pandas: alat za
statističku analizu podataka dizajnirana kao zamjena za R verziju za
manipulaciju podataka; sa temeljnom strukturom \"Podatkovnog okvira\"
(eng. DataFrame), knjižnica nadopunjuje ostatak znanstvenih Python
knjižnica, čineći ju dobrim kandidatom i za veće baze podataka
[@McKinney2011].

Inženjerstvo atributa
---------------------

Dobro koncepirani atributi neke pute mogu efikasnije obuhvatiti važnost
informacije nego izvorni atributi [@Howbert2012]. Dataset ima puno
tekstualnih podataka, što za procesiranje klasifikacijskoga modela
predstavlja problem. Kako bi se normalizirali tekstualni podaci,
primijenjeno je one-hot kodiranje, specifično na slijedećim atributima:
`COUNTRY`, `CHANNEL` te `STATUS` `RESERVATION`. Vremenski atributi kao
što je `VRIJEME KREIRANJA REZERVACIJE` ne dovode predvidljivost: datum,
iako brojčana vrijednost, nema informacijsku vrijednost za model. Za
vremenske atribute potrebno je izvest važnije vremenske atribute koje
promatrane kao cijelinu opisuju početni vremenski atribut:
`DAY OF THE WEEK` (dan u tjednu), `YEAR` (godina), `DAYS TO`
`CANCELLATION` (dani to brisanja), `DAYS TO` `CHECKIN` (dani do
prijave).

$$\label{eqsin}
    x_{sin} = sin\left(\frac{2\pi x}{max(x)}\right)$$ $$\label{eqcos}
    x_{cos} = cos\left(\frac{2\pi x}{max(x)}\right)$$ Neki od tih
atributa imaju cikličku prirodu i kao takvi moraju se tretirati
prikladno: najveća vrijednost se nalazi odmah pokraj najmanje
vrijednosti. To se postiglo koristeći $sin$ (eq.
[\[eqsin\]](#eqsin){reference-type="ref" reference="eqsin"}) i $cos$
(eq. [\[eqcos\]](#eqcos){reference-type="ref" reference="eqcos"})
funkcije [@Chakraborty2018]. Funkcije
[\[eqsin\]](#eqsin){reference-type="ref" reference="eqsin"} i
[\[eqcos\]](#eqcos){reference-type="ref" reference="eqcos"} pretvore
vremenski podataka u koordinate kruga, koji točno prikazuje ciklički
podatak.

  **Naziv**                     **Raspon (max - min)**          **Opis**
  ----------------------------- ------------------------------- -------------------------------------------------------------------
  `YEAR`                        2016 - 2018                     Year when reservation was first created
  `NUMBER OF DAYS`              1 - 640                         Booked days
  `COUNTRY`                     0 - 162                         Costumer home country
  `ROOM TYPE`                   0 - 75                          Type of the room
  `DEPOSIT`                     0 - 143663                      Amount of the deposit
  `ROOM NUMBER`                 1 - 450                         Number of rooms booked
  `CHILDREN`                    0/1                             Indicates if children are present
  `PERSONS`                     1 - 90                          Number of persons
  `NIGHTS`                      0 - 3948                        Number of booked nights
  `SIN/COS WEEKDAY CREATED`     (-0.866) - 0.866 / (-1) - 1     Day of the week when reservation was created
  `SIN/COS WEEK CREATED`        (-0.9995) - 0.9995 / (-1) - 1   Week of the year when reservation was created
  `SIN/COS MONTH CREATED`       (-1) - 1 / (-1) - 1             Month of the year when reservation was created
  `SIN/COS WEEKDAY CONFIRMED`   (-0.866) - 0.866 / (-1) - 1     Day of the week when reservation was confirmed
  `SIN/COS WEEK CONFIRMED`      (-0.9995) - 0.9995 / (-1) - 1   Week of the year when reservation was confirmed
  `SIN/COS MONTH CONFIRMED`     (-1) - 1 / (-1) - 1             Month of the year when reservation was confirmed
  `SIN/COS WEEKDAY CHECK IN`    (-0.866) - 0.866 / (-1) - 1     Day of the week of check in date
  `SIN/COS WEEK CHECK IN`       (-0.9995) - 0.9995 / (-1) - 1   Week of the year of check in date
  `SIN/COS MONTH CHECK IN`      (-1) - 1 / (-1) - 1             Month of the year of check in date
  `SIN/COS WEEKDAY CHECK OUT`   (-0.866) - 0.866 / (-1) - 1     Day of the week of check out date
  `SIN/COS WEEK CHECK OUT`      (-0.9995) - 0.9995 / (-1) - 1   Week of the year of check out date
  `SIN/COS MONTH CHECK OUT`     (-1) - 1 / (-1) - 1             Month of the year of check out date
  `CHANNEL`                     0 - 8                           Method of reservation
  `RESERVATION STATUS`          0 -10                           Reservation status
  `DAYS TO CHECK IN`            0 - 1224                        Number of days between created reservation date and check in date

Prikaz podataka
---------------

Kako bi se odabrali relevanti atributi koji ulaze u model, potrebno je
shvatiti podatke na raspolaganju, njihovo značenje i ponašanje. Ovo
poglavlje analizira kretanje podataka te grafički prikaz istih. Na
figuri [1](#fig: cancellationRatio){reference-type="ref"
reference="fig: cancellationRatio"} su prikazane stope brisanja za svaki
hotel: pojedinačni hotel ima različitu stopu brisanja, koja se kreće od
minimuma 4.54% do maksimuma 40.58%. Međutim, sveukupni prosjek je od
28.64% što je u skladu sa prijašnjim radovima.

![Stopa brisanja rezervacija po
hotelu[]{label="fig: cancellationRatio"}](resources/cancel_ratio.pdf){#fig: cancellationRatio
width="\\textwidth"}

Falk i Vieru [@Falk2018] su dokazali da sa povećanje uporabe online
agencija također dovodi do povećanja stope brisanja rezervacija. Prateći
taj zaključak, zbog unaprijeđenja tehnologije i pristupa online
agencijama, kroz godine bi stopa brisanja rezervacija rasla. Međutim u
slučaju podataka na raspolaganju, kao što se može primijetiti na figuri
[2](#fig: cancellationRatioYear){reference-type="ref"
reference="fig: cancellationRatioYear"}, stopa brisanja je stabilna kroz
sve tri godine, sa prosjekom od 32.23% i standardnom devijacijom od
2.11%.

![Stopa brisanja rezervacija po
godini[]{label="fig: cancellationRatioYear"}](resources/cancel_ratio_per_year.pdf){#fig: cancellationRatioYear
width="\\textwidth"}

Iz figure [3](#fig: cancellationsRes){reference-type="ref"
reference="fig: cancellationsRes"} (prikazana sa logaritamskom skalom)
se primjećuje da ima očekivani eksponencijalni rast u broju rezervacija
sa manjim rasponom broju dana prije prijave u hotel. Međutim, ima i
neočekivani rast u rezervacijama skoro godinu dana u naprijed.
Objašnjenje za taj neočekivani rast stoji u činjenici da prosječno 65%
tih rezervacija je za sezonsko razdoblje (uzimajući u obzir da sezonsko
razdoblje je između 01.05. i 30.09.) sa prosječno 6.6% brisanih
rezervacija za to razdoblje. Ostalih 35% neočekivanih rezervacija su za
razdoblje izvan sezone, rezultirajući sa prosječnom stopom brisanja od
29.9%. Broj brisanja rezervacija je u skladu sa eksponencijalnim rastom
rezervacija: to dokazuje da rezervacija ima veću vjerojatnost da se
izbriše kako se datum prijave približava.

![Odnos obrisanih podataka između datuma rezervacije i broj dana
brisanja prije prijave u hotel
(check-in)[]{label="fig: cancellationsRes"}](resources/cancellations_reservations.pdf){#fig: cancellationsRes
width="\\textwidth"}

Optimizacija i testiranja
=========================

Testiranje dataseta
-------------------

Nakon provođenja postupka inženjersta atributa, dobilo se dataset sa tri
vrste ciljnog atributa. Kako bi se odabralo najprikladnijeg provelo se
testiranje nad svim triju datasetima. Ciljni atributi se razlikuju po
slijedećim aspektima:

-   Binarni izlaz: dvije izlazne klase gdje jedinica označava *brisano*
    a nula *ne obrisano*.

-   Kategorički izlaz prve vrste **CAT CH**: izlaz je podijeljen u 8
    kategorija temeljene na broj dana između stvaranje rezervacije i
    prijave u hotel.

-   Kategorički izlaz druge vrste **CAT RES**: izlaz je podijeljen u 7
    kategorija temeljene na broj dana između kreiranja rezervacije i
    datuma brisanja rezervacije (gdje nulta kategorija označava ne
    obrisane rezervacije).

Testiranje se provelo na dva različita seta parametra. Pošto su izlazi
kategoričke vrste, potrebne su i agregacijske metode kako bi se dovelo
rezultat na binarinu predikciju. Set parametra su:

-   Set parametra **A**: stopa učenja (eng. learning rate) 0.2; veličina
    stabla (eng. number of estimators) 200; maksimalna dubina (eng.
    maximum depth) 2.

-   Set parametra **B**: stopa učenja (eng. learning rate) 0.01;
    veličina stabla (eng. number of estimators) 1000; maksimalna dubina
    (eng. maximum depth) 4.

te agregacijske metode:

-   Agregacijska metoda **50%**: prag od 50%, gdje rezultati ispod praga
    znače da rezervacija *nije obrisana*.

-   Agregacijska metoda **binarna**: nulta kategorija označava da
    rezrevacija *nije obrisana*, dok sve druge označavaju da je
    rezervacija *obrisana*.

Iz tablice
[\[tab: datasets-results\]](#tab: datasets-results){reference-type="ref"
reference="tab: datasets-results"} se vidi da dataset sa kategoričkim
izlazima temeljeni na broj dana između kreiranja rezervacije i datuma
brisanja iste (CAT RES), skupa sa binarnom agregacijskom metodom
(binary) te set parametra sa nižim vrijednostima (A), daju najbolji
rezultat. Kao takav, taj dataset se koristilo za sva slijedeća
treniranja i testiranja.

   **Parametri**  **Ciljani izlaz**   **Agregacija**    **F1-score**
  --------------- ------------------- ---------------- --------------
         A        binary              None                  0.77
         B        binary              None                  0.78
         A        `CAT CH`            50%                   0.75
         A        `CAT CH`            binary                0.74
         A        `CAT RES`           50%                   0.72
         A        `CAT RES`           binary                0.83
         B        `CAT CH`            50%                   0.77
         B        `CAT CH`            binary                0.74
         B        `CAT RES`           50%                   0.72
         B        `CAT RES`           binary                0.66

  : Testiranje dataseta[]{label="tab: datasets-results"}

Optimizacija hiper-parametra
----------------------------

Algoritmi strojnog učenja i dubokog učenja su definirani po parametrima
i hiper-parametra. Parametre se mogu definirati direktno iz početne
strukture algoritma, ali hiper-parametri su višeg nivoa te moraju se
odrediti i optimizirati prije početka treniranja jer mogu drastično
utjecati na efikasnost algoritma [@Xia2017]. Postoje razni algoritmi za
traženje optimalnih vrijednosti hiper-parametra; za algoritam
XGBoostinga se koristila izravna metoda pretrage: mrežno pretraživanje
(eng. grid search). Metoda prolazi kroz sve kombinacije predefiniranih
vektora mogučih vrijednosti, te kombinacija sa najboljem rezultatom se
smatra optimalnim rješenjem. Negativna strana ove meotode je što brzina
izvođenja ovisi o veličini dataseta i algoritma nad kojem se aplicira.
Za algoritam XGBoosting-a se odabralo tri hiper-parametra za koje se
smatra utječu na kvalitetu modela i brzinu treniranja istog: stopa
učenja (eng. learning rate), broj procjenitelja (eng. number of
estimators) i dubina stabla (eng. tree depth). Stopa učenja je
vrijednost doprinosa funkciji gubitka nakon svake iteracije; broj
procjenitelja predstavlja broj stabala u strukturi, odnosno veličina
stabla. Za veće količine podataka se preporuča imati manji broj
procjenitelja pošto veći broj ulaze u problem pre-učenja [@Xia2017].
Prema tome, korelacija između veličine stabla i dubine stabla je
definirana prema slijedećoj formuli: $$\label{treeSize}
    J \leq 2^D$$ Gdje J označava veličinu stabla te D maksimalnu dubinu
stabla [@Xia2017]. Mrežno pretraživanje se izvelo nad raznim veličinama
podataka te validirano sa 10-strukom unakrsnom metodom. U tablici
[\[gridSearch02\]](#gridSearch02){reference-type="ref"
reference="gridSearch02"} su prikazane provjerene vrijednosti
hiper-parametra: veza između veličine stabla i dubine stabla je u skladu
sa jednadžbom ([\[treeSize\]](#treeSize){reference-type="ref"
reference="treeSize"}).

  **Hiper-parametar**    **Vrijednosti**
  ---------------------- ---------------------
  Learning rate          \[0.05, 0.1, 0.15\]
  Number of estimators   \[100, 200, 300\]
  Maximum tree depth     \[3, 5, 7, 9\]

  : Grid search - distribucije[]{label="gridSearch02"}

Tablica [\[gridSearchResults\]](#gridSearchResults){reference-type="ref"
reference="gridSearchResults"} prikazuje najbolje rezultate pretrage:
veličina dataseta sa korištenim hiper-parametrima za dobiveni rezultat.
Rezultati pokazuju da kombinacija hiper-parametra \[Stopa učenja,
veličina stabla, dubina stabla\]=\[0.15, 300, 9\] daje najbolji
rezultat.

  --------- -------------- ----- --- -------
                                     
   dataset                           
   učenja                            
   stabla                            
   stabla    **F1-score**            
     1%          0.15       300   7   0.787
     10%         0.15       300   9   0.892
     20%         0.15       300   9   0.909
     40%         0.15       300   9   0.922
     60%         0.15       300   9   0.928
     80%         0.15       300   9   0.932
    100%         0.15       300   9   0.935
  --------- -------------- ----- --- -------

  : Grid search - rezultati[]{label="gridSearchResults"}

Druga metoda za optimizaciju hiper-parametra je Bayesianova
optimizacijske metoda. Ova metoda se koristila za optimizaciju modela
dubinskog učenja DNN. Za razliku od mrežne metode, Bayesianova metoda
radi na način da vrijednosti parametra imaju Gaussianovu razdiobu, te se
kreće po razdiobi dok ne pronađe optimalnu kombinaciju [@Snoek2017]. Kao
što ime sugerira, Bayesianova optimizacijska metoda se temelji na
Bayesovom teoremu vjerojatnosti: vjerojatnost modela (M) prema danim
dokazima (E) je proporcionalno vjerojatnosti E prema M pomnoženoj sa
prethodnom vjerojatnosti od M [@Brochu2010]:

$$P(M|E) \propto P(E|M)P(M)$$

Kod optimizacijske primjere, prior označava vjerovanje o mogućim
vrijednostima za hiper-parametre. Bayesianova metoda je različita od
ostalih metoda jer napravi model vjerojatnosti od ulazne funkcije, te
koristi taj model kako bi odredio gdje se pomaknuti slijedeće na
razdiobi vrijednosti hiper-parametra [@Snoek2017]. Kod mrežne metode je
potrebno odrediti vektore vrijednosti kako bi algoritam odradio sve
kombinacije, dok kod Bayesianove metode je potrebno odrediti samo
minimum i maksimum vrijednosti za svaki hiper-parametar. Za DNN model se
odabralo četiri hiper-parametra za optimizaciju: stopa učenja (eng.
Learning rate), stopa izbačaja podataka (eng. Dropout rate), broj
razdoblja (eng. Epochs) te veličina hrpe (eng. Batch size). U tablici
[\[BayesOptimization\]](#BayesOptimization){reference-type="ref"
reference="BayesOptimization"} su prikazane odabrane granice razdiobe za
optimizaciju.

  **Hiper-parametar**   **Vrijednosti**
  --------------------- -----------------
  Learning rate         (1e-9, 1e-4)
  Dropout rate          (0.1, 0.3)
  Epochs                (5, 30)
  Batch size            (60, 120)

  : Bayesianova optimizacija - početne
  vrijednosti[]{label="BayesOptimization"}

Kod Bayesianove optimizacije dva parametra su značajna: broj
optimizacija, odnosno broj puta će algoritam se pomicati po modelu
vjerojatnosti te broj nasumičnih istraživanja unutar ponuđenih granica.
U svrhu ovog testa, oba parametra su postavljena na 15. Test je odrađen
na 20% dataseta, od kojih 20% je odvojeno za validaciju.

  **Hiper-parametar**    **Vrijednosti**
  --------------------- -----------------
  Learning rate               1e-8
  Dropout rate                0.14
  Epochs                        6
  Batch size                   80
  **Accuracy**             **0.7531**

  : Bayesianova optimizacija - optimizirane
  vrijednosti[]{label="BayesOptimization-result"}

Važnost atributa
----------------

Kod treniranje modela, atributi imaju različite utjecaje na rezultat:
jedan atribut može više utjecati na način da njegova promjena u
vrijednosti odlučuje u krajnjem rezultatu. Algoritam XGBoost dopušta
izvuči vrijednost važnosti svakog atributa. U figuri
[4](#fig: featureDistributions){reference-type="ref"
reference="fig: featureDistributions"} je prikazana distribucija
vjerojatnosti od šest atributa koji najviše utječu na model. Važnost
atributa pomaže u donošenju odluka u stvarnim okolnostima: atributi u
modelu predstavljaju opis jednog događaja; prema tome, ako jedan atribut
ima veći utjecaj na rezultat modela, također može imati veći utjecaj na
ishod događaja u stvarnim okolnostima. Kao što je dokazao Leeuwen
[@Leeuwen2018], atribut kanala je među najvažnijima u modelu: to
pokazuje da odabir kanala za rezervaciju ima veći utjecaj na ishod
brisanja rezervacije. Također imaju veliku važnost vremenski atributi:
vrijeme kreiranja rezervacije i broj dana između rezervacije i datuma
prijave. To pokazuje da osim kanala, veliki utjecaj ima i odabir vremena
rezervacije.

![Distribucija vjerojatnosti od šest najvažnijih
atributa[]{label="fig: featureDistributions"}](resources/feature_distributions_top_six.pdf){#fig: featureDistributions
width="\\textwidth"}

Zaključak
=========

U ovom završnom radu se koncentriralo na postignuće visoke preciznosti u
prognoziranju brisanja rezervacija koristeći razne metode strojnog i
dubokog učenja. Algoritam XGBoost je dao bolje rezultate na odnosu na
gustu neuronsku mrežu (DNN), prvenstveno radi nedostatka procesorke
snage za optimizaciju i treniranje kompleksnije mreže. Unatoč tome,
XGBoost je dobar odabir za klasifikacijske probleme, zbog svoje brzine
treniranja, dotreniravanja i mogućnosti paralelizacije procesa.
Bayesianova metoda optimizacije je zbog svoje osobnosti pretraživanja
temeljenom na modelu vjerojatnosti, bolja od mrežne metode koja ovisi o
vektorskim ulazima odabrani od strane stvaratelja modela i optimizacije.
U ovom radu, odabir atributa se izvelo na vizualan i analitički način;
buduće izvedbe bi obuhvatile naprednije načine selekcije atributa:
Challita, Khalil i Beauseroy [@Challita2016] predlažu metodu strojnog i
dubinskog učenja, temeljenoj na težinskim faktorima, koja efikasno
odabere najvažnije atribute kao ulaz modela.

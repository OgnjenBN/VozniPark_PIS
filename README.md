# Vozni park

## UVOD 

U okviru ovog seminarskog rada obrađena je tema upravljanja voznim parkom kroz izradu web aplikacije koristeći savremene tehnologije za frontend i backend razvoj. Aplikacija je razvijena koristeći **HTML**, **CSS** i **JavaScript** za frontend, čime je omogućena intuitivna i interaktivna korisnička interakcija. Korišten je **Bootstrap** framework za brže i responzivnije oblikovanje korisničkog interfejsa.
Sa serverske strane, za izgradnju backend dijela aplikacije korišćen je **FastAPI** framework, poznat po jednostavnosti implementacije i brzini, koji podržava osnovne CRUD (Create, Read, Update, Delete) operacije za upravljanje podacima. U radu sa podacima, korišćen je **MySQL** kao baza podataka, što je omogućilo efikasno skladištenje, upravljanje i pretragu podataka o voznom parku.
Cilj ovog projekta bio je da se kroz integraciju različitih tehnologija napravi funkcionalna aplikacija koja će omogućiti jednostavno praćenje i upravljanje vozilima u voznom parku, uz intuitivan korisnički interfejs i stabilnu backend podršku.

## STRUKTURA PODATAKA 

Projekat “Vozni park” organizovan je u nizu fajlova i direktorijuma koji zajedno čine funkcionalnu cjelinu za upravljanje evidencijom vozila, vozača i radnih naloga. Svaki fajl ima specifičnu ulogu u projektu: <br>
•	app: Ovaj direktorijum sadrži backend aplikaciju razvijenu koristeći FastAPI framework u Python-u. Sadrži API rute, modele podataka i logiku aplikacije. <br>
•	web: Direktorijum koji sadrži sve fajlove za frontend implementaciju. Ovde se nalaze HTML, CSS i JavaScript fajlovi koji omogućavaju korisnicima interakciju sa backend-om putem web interfejsa.<br>
•	README: Dokumentacija projekta koja pruža osnovne informacije o projektu.. <br>
•	requirements: Fajl koji sadrži listu Python biblioteka potrebnih za izvršavanje aplikacije. Ovo uključuje FastAPI, uvicorn, pydantic i druge zavisnosti. <br>
•	sample_data.py: Python skripta koja služi za unošenje podataka u MySQL bazu. Ova skripta pomaže u testiranju aplikacije sa realnim podacima. <br>
•	create_usersql: Skripta za kreiranje korisnika u MySQL bazi podataka. Automatizuje proces kreiranja korisničkih naloga u sistemu. <br>
•	Docker folderi: Direktorijumi koji sadrže Dockerfile i docker-compose.yml fajlove za konfigurisanje Docker kontejnera. Ovo omogućava laku reprodukciju aplikacije u Docker okruženju. <br>
•	.env: Konfiguracioni fajl za virtualno okruženje, koji sadrži važne konfiguracije kao što su putanja do baze podataka i druge podesive parametre.
Ova struktura omogućava jasnu organizaciju i održavanje projekta “Vozni park”, omogućavajući developerima efikasno razvijanje, testiranje i implementaciju novih funkcionalnosti

## BACKEND

Za backend aplikacije korišćen je FastAPI framework, koji je relativno novi Python web framework, poznat po svojoj jednostavnosti, brzini i podršci za asinhrone operacije. FastAPI je posebno pogodan za razvoj aplikacija koje moraju brzo obrađivati zahteve i omogućava laganu integraciju sa standardnim Python bibliotekama, što ga čini idealnim izborom za našu aplikaciju za upravljanje voznim parkom. FastAPI je odabran zbog podrške za brze i efikasne CRUD operacije (kreiranje, čitanje, ažuriranje i brisanje podataka) koje su bile ključne za upravljanje podacima o vozilima. Implementacija CRUD operacija omogućila je kreiranje novih zapisa (npr. unosa novih vozila), prikaz postojećih zapisa, kao i ažuriranje i brisanje podataka o vozilima u voznom parku.<br>
Backend aplikacija koristi **SQLAlchemy ORM** (Object Relational Mapper) za rad sa MySQL bazom podataka. SQLAlchemy omogućava efikasno mapiranje Python klasa na tabele u MySQL-u, čime se pojednostavljuje rad sa bazom podataka kroz objektno-orijentisani pristup.<br> <br>
FastAPI koristi **Pydantic** za validaciju ulaznih podataka, što osigurava da podaci koji stižu od korisnika budu ispravni pre nego što se sačuvaju u bazu podataka. Na primer, kada korisnik unese podatke o novom vozilu, Pydantic validira da li su uneti svi potrebni podaci i da li su u ispravnom formatu. <br>
Backend se sastoji od nekoliko ključnih komponenti: <br>
•	API rute <br>
•	Modeli podataka <br>
•	Šeme <br>
•	DB fajl <br>
•	Main fajl <br>

Аpi rute u ovom projektu su definisane tako što se prvo kreira API router (APIRouter()) kako bi se grupisale rute. Zatim se za svaku rutu definiše dekorator (npr. @router.post("/")). Unutar funkcije za rutu, specificira se šta ruta radi i povezuje se sa bazom koristeći **db: Session = Depends(get_db).** Nakon toga se šalju zahtjevi bazi podataka i dobijaju odgovori. Na kraju, funkcija vraća podatke kao odgovor na zahtev, omogućavajući organizovano i efikasno upravljanje rutama i operacijama nad podacima u bazi. <br>

Modeli podataka reprezentuju strukturu podataka koji se koriste u aplikaciji. Na primjer, model za vozača predstavlja klasu koja definiše strukturu tabele “vozaci” u bazi podataka. Ispod tabele definisan je odnos sa modelom “RadniNalog” koji omogućava navigaciju između vozača i njegovih radnih naloga. Varijabla back_populates='vozac' omogućava obostranu vezu između dva modela. <br>

@router.post("/", response_model=VoziloOut)
def create_vozilo(vozilo: VoziloCreate, db: Session = Depends(get_db)):
db_vozilo = Vozilo(**vozilo.dict())
db.add(db_vozilo)
db.commit()
db.refresh(db_vozilo)
return db_vozilo



 

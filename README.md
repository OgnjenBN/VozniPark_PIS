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

**API RUTE**  u ovom projektu su definisane tako što se prvo kreira API router (APIRouter()) kako bi se grupisale rute. Zatim se za svaku rutu definiše dekorator (npr. @router.post("/")). Unutar funkcije za rutu, specificira se šta ruta radi i povezuje se sa bazom koristeći **db: Session = Depends(get_db).** Nakon toga se šalju zahtjevi bazi podataka i dobijaju odgovori. Na kraju, funkcija vraća podatke kao odgovor na zahtev, omogućavajući organizovano i efikasno upravljanje rutama i operacijama nad podacima u bazi. <br>

**Modeli** podataka reprezentuju strukturu podataka koji se koriste u aplikaciji. Na primjer, model za vozača predstavlja klasu koja definiše strukturu tabele “vozaci” u bazi podataka. Ispod tabele definisan je odnos sa modelom “RadniNalog” koji omogućava navigaciju između vozača i njegovih radnih naloga. Varijabla back_populates='vozac' omogućava obostranu vezu između dva modela. <br>


```
class Vozac(Base):
    __tablename__ = 'vozaci'

    id = Column(Integer, primary_key=True, index=True)
    ime = Column(String(30), index=True)
    prezime = Column(String(30), index=True)
    broj_vozacke_dozvole = Column(String(15), unique=True)
    datum_isteka_dozvole = Column(Date)
    kategorije_vozacke_dozvole = Column(String(15))
    kontakt_informacije = Column(String(100))
    ogranicenja_za_voznju = Column(String(50))
    status = Column(Enum('aktivno', 'neaktivno', name='status_vozaca'), default='aktivno')

    radni_nalozi = relationship('RadniNalog', back_populates='vozac')
```
<br>

**ŠEME** za radne naloge definišu strukturu i validaciju podataka pomoću Pydantica. Klase se kreiraju kao naslednici BaseModel iz Pydantica i predstavljaju modele podataka sa atributima i validacionim pravilima. Enumeracija (StatusRadnogNalogaEnum) definiše tri moguća statusa radnog naloga: otvoren, u toku, i završen. Ovo osigurava da atribut statusa može imati samo dozvoljene vrijednosti. Bazična klasa (RadniNalogBase) sadrži osnovne atribute radnog naloga, uključujući ID vozila i vozača, opis zadatka, datum i vreme izdavanja, rok završavanja i status. Klasa za kreiranje (RadniNalogCreate) nasleđuje bazičnu klasu i koristi se za validaciju prilikom kreiranja novih radnih naloga. Klasa za izlazne podatke (RadniNalogOut) dodaje dodatni atribut id i koristi se za povratne podatke prema klijentu. Ova struktura omogućava striktno definisanje i validaciju podataka, čineći aplikaciju sigurnijom i pouzdanijom. <br>

```
 class StatusRadnogNalogaEnum(str, Enum):
    otvoren = 'otvoren'
    u_toku = 'u toku'
    zavrsen = 'zavrsen'

class RadniNalogBase(BaseModel):
    vozilo_id: int
    vozac_id: int
    opis_zadatka: str
    datum_i_vrijeme_izdavanja: datetime
    rok_zavrsavanja: datetime
    status: StatusRadnogNalogaEnum

class RadniNalogCreate(RadniNalogBase):
    pass

class RadniNalogOut(RadniNalogBase):
    id: int
    
    class Config:
       from_attributes = True
```
<br>

**Db fajl** konfiguriše vezu sa MySQL bazom podataka koristeći SQLAlchemy i čita vrijednosti iz .env fajla za postavljanje parametara baze. Prvo se učitavaju vrijednosti iz .env fajla, uključujući URL baze podataka. Zatim se kreira engine koji povezuje SQLAlchemy sa bazom podataka. SessionLocal je konfigurisan za kreiranje sesija sa bazom, sa opcijama autocommit=False i autoflush=False za ručno upravljanje transakcijama i osvježavanjem podataka. Base je deklarativna baza klasa iz koje će sve SQLAlchemy klase modela naslijediti. Funkcija get_db definiše zavisnost koja upravlja životnim ciklusom sesije baze podataka, otvarajući sesiju prije operacija sa bazom i zatvarajući je nakon završetka operacija.

*Sesija* 
<br>

```
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.declarative import declarative_base
from dotenv import load_dotenv
import os

load_dotenv(dotenv_path=os.path.join(os.path.dirname(__file__), '.env'))

MYSQL_ROOT_PASSWORD = os.getenv("MYSQL_ROOT_PASSWORD")
MYSQL_DATABASE = os.getenv("MYSQL_DATABASE")

DATABASE_URL = os.getenv("DATABASE_URL")


engine = create_engine(DATABASE_URL)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()

# Dependency
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```
**Main fajl** definiše osnovnu konfiguraciju FastAPI aplikacije. Kreira se FastAPI instanca sa prilagođenim URL-ovima za dokumentaciju i dodaje se CORS middleware koji omogućava zahtjeve sa svih origin-a. Funkcija startup_event inicijalizuje bazu podataka pri pokretanju aplikacije pozivom init_db(). Definiše se jednostavna root ruta koja vraća "Hello, World", a sve definisane API rute uključene su pomoću app.include_router (api_router). <br>

```
from fastapi import FastAPI
from db import init_db
from api.routes import api_router
from fastapi.middleware.cors import CORSMiddleware


app = FastAPI(docs_url="/api/docs/", openapi_url="/api/openapi.json")

origins = ["*"]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)




@app.on_event("startup")
def startup_event():
    init_db()  # Inicijalizujte bazu podataka kada aplikacija počne sa radom

@app.get("/api/")
def read_root():
    return {"Hello": "World"}


app.include_router(api_router)
```

## FRONTEND

Frontend aplikacija je ključna komponenta korisničkog interfejsa (UI) i odgovorna je za interakciju korisnika sa aplikacijom. U ovom projektu, frontend je razvijen koristeći HTML, CSS, JavaScript, a za brzo i responzivno oblikovanje korišćen je Bootstrap framework. Kroz kombinaciju ovih tehnologija omogućeno je kreiranje jednostavnog i intuitivnog interfejsa za rad sa voznim parkom. <br>

HTML (HyperText Markup Language): HTML je korišćen za definisanje strukture web stranice. Svaka stranica sadrži osnovne elemente poput zaglavlja, tabela, formi i dugmadi koje korisnici koriste za pregled podataka i interakciju sa sistemom. <br>

```
<table class="table">
                <thead>
                    <tr>
                        <th scope="col">R.br.</th>
                        <th scope="col">Marka</th>
                        <th scope="col">Model</th>
                        <th scope="col">Reg.</th>
                        <th scope="col">Istek reg.</th>
                        <th scope="col">Godište</th>
                        <th scope="col">Gorivo</th>
                        <th scope="col">Status</th>
                        <th scope="col">Izmjeni</th>
                        <th scope="col">Izbriši</th>
                    </tr>
                </thead>
                <tbody id="tableBody">

                </tbody>
            </table>
```

Sledećа  kod prikazuju HTML koda za filterisanje vozila putem padajućeg menija.
```
<form id="filterFormVozila">
        <label for="filterBrand">Marka:</label>
        <select id="filterBrand" onchange="filterVehicles()">
                    <option value="all">Sve marke</option>
                    <option value="Volkswagen">Volkswagen</option>
                    <option value="BMW">BMW</option>
                    <option value="Audi">Audi</option>
                    <option value="Mercedes">Mercedes</option>
                    <option value="Toyota">Toyota</option>
                    <option value="Ford">Ford</option>
                    <option value="Skoda">Skoda</option>
                    <option value="Seat">Seat</option>
                    <option value="Reanult">Renault</option>
                    <option value="Honda">Honda</option>
                    <option value="Opel">Opel</option>
                    <option value="Peugeot">Peugeot</option>
                    <option value="Dacia">Dacia</option>
        </select>
```

**CSS** (Cascading Style Sheets): CSS je korišćen za stilizovanje HTML elemenata i kreiranje vizualno privlačnog i korisnički prijatnog interfejsa. Pomoću CSS-a definisane su boje, fontovi, raspored elemenata, kao i prilagođeni izgled različitih komponenata.

Takođe, implementirali smo opciju “Tamnog režima” gdje korisnik može izabrati opciju da li će interfejs biti prikazan u podrazumijevanoj bijeloj boji ili crnoj boji gdje manje zamara oči. U HTML smo dodali samo dugme za tamni režim, dok u CSS (kao I JavaScript-u) smo dodali kod za implementiranje tamnog režima.
```
body.dark-mode {
    --background-color: #333;
    --text-color: #eee;
    --table-bg: #444;
    --table-border: #555;
}

body.dark-mode table {
    background-color: var(--table-bg);
    border-color: var(--table-border);
}

body.dark-mode th, body.dark-mode td {
    border-color: var(--table-border);
    background-color: var(--table-bg);
    color: var(--text-color);
}
```

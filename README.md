# Vozni park

## UVOD 

U okviru ovog seminarskog rada obrađena je tema upravljanja voznim parkom kroz izradu web aplikacije koristeći savremene tehnologije za frontend i backend razvoj. Aplikacija je razvijena koristeći HTML, CSS i JavaScript za frontend, čime je omogućena intuitivna i interaktivna korisnička interakcija. Korišten je Bootstrap framework za brže i responzivnije oblikovanje korisničkog interfejsa.
Sa serverske strane, za izgradnju backend dijela aplikacije korišćen je FastAPI framework, poznat po jednostavnosti implementacije i brzini, koji podržava osnovne CRUD (Create, Read, Update, Delete) operacije za upravljanje podacima. U radu sa podacima, korišćen je MySQL kao baza podataka, što je omogućilo efikasno skladištenje, upravljanje i pretragu podataka o voznom parku.
Cilj ovog projekta bio je da se kroz integraciju različitih tehnologija napravi funkcionalna aplikacija koja će omogućiti jednostavno praćenje i upravljanje vozilima u voznom parku, uz intuitivan korisnički interfejs i stabilnu backend podršku.

## STRUKTURA PODATAKA 

Projekat “Vozni park” organizovan je u nizu fajlova i direktorijuma koji zajedno čine funkcionalnu cjelinu za upravljanje evidencijom vozila, vozača i radnih naloga. Svaki fajl ima specifičnu ulogu u projektu:
•	app: Ovaj direktorijum sadrži backend aplikaciju razvijenu koristeći FastAPI framework u Python-u. Sadrži API rute, modele podataka i logiku aplikacije.
•	web: Direktorijum koji sadrži sve fajlove za frontend implementaciju. Ovde se nalaze HTML, CSS i JavaScript fajlovi koji omogućavaju korisnicima interakciju sa backend-om putem web interfejsa.
•	README: Dokumentacija projekta koja pruža osnovne informacije o projektu..
•	requirements: Fajl koji sadrži listu Python biblioteka potrebnih za izvršavanje aplikacije. Ovo uključuje FastAPI, uvicorn, pydantic i druge zavisnosti.
•	sample_data.py: Python skripta koja služi za unošenje podataka u MySQL bazu. Ova skripta pomaže u testiranju aplikacije sa realnim podacima.
•	create_usersql: Skripta za kreiranje korisnika u MySQL bazi podataka. Automatizuje proces kreiranja korisničkih naloga u sistemu.
•	Docker folderi: Direktorijumi koji sadrže Dockerfile i docker-compose.yml fajlove za konfigurisanje Docker kontejnera. Ovo omogućava laku reprodukciju aplikacije u Docker okruženju.
•	.env: Konfiguracioni fajl za virtualno okruženje, koji sadrži važne konfiguracije kao što su putanja do baze podataka i druge podesive parametre.
Ova struktura omogućava jasnu organizaciju i održavanje projekta “Vozni park”, omogućavajući developerima efikasno razvijanje, testiranje i implementaciju novih funkcionalnosti

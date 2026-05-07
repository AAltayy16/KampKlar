# KampKlar
1. Forside
Prosjekttittel: KampKlar
Navn: Arda
Klasse: 2IMI
Kort beskrivelse av prosjektet:
KampKlar er en nettapplikasjon for nybegynnere innen kampsport. Brukeren velger en kampsport som feks boksing, karate, judo, MMA, taekwondo eller BJJ – og får opp relevante øvelser og teknikker tilpasset nybegynnernivå. Applikasjonen er bygget med Flask som backend og henter øvelsedata fra en MariaDB-database.

2. Systembeskrivelse
Formål med applikasjonen:
Målet er å gjøre det enklere å komme i gang med kampsport ved å samle grunnleggende øvelser og teknikker på ett sted. Brukeren skal slippe å lete rundt på ulike nettsider – alt er organisert etter sport og vanskelighetsgrad.

Brukerflyt:
Brukeren åpner startsiden og velger en kampsport fra et rutenett.
Siden viser en liste med øvelser for valgt sport.
Brukeren kan filtrere øvelsene på nivå (nybegynner / videregående) eller type (teknikk, forsvar, kondisjon osv.).
Hvert øvelseskort kan ekspanderes for å vise steg for steg instruksjoner.

Teknologier brukt:
Python / Flask
MariaDB
HTML / CSS / JS
(valgfritt) Docker / Nginx / Gunicorn / Waitress


3. Server-, infrastruktur- og nettverksoppsett
Servermiljø
Ubuntu VM / lokal maskin med Python 3.x og MariaDB installert.
Nettverksoppsett

IP-adresse: 127.0.0.1 (lokal utvikling)
Port: 5000 (Flask dev-server) / 80 (Nginx i produksjon)
Brannmurregler: Port 5000 åpen internt, port 80/443 eksponert eksternt

Klient (nettleser) → Flask (port 5000) → MariaDB (port 3306)
Tjenestekonfigurasjon

Flask kjøres med Waitress eller Gunicorn i produksjon
Miljøvariabler lagres i .env-fil
Filrettigheter: kun applikasjonsbrukeren har lesetilgang til .env


4. Prosjektstyring -- GitHub Projects (Kanban)
Prosjektet ble styrt med GitHub Projects i tre kolonner:

To Do – planlagte oppgaver (f.eks. "Lag databasetabell for øvelser")
In Progress – aktive oppgaver under utvikling
Done – fullførte oppgaver

Issues ble brukt til å spore enkeltfunksjoner og feil.
Refleksjon: Kanban-tavlen hjalp med å holde oversikt over hva som gjensto og forhindret at jeg jobbet på for mange ting samtidig.

5. Databasebeskrivelse
Databasenavn: kampklar
Tabeller:
TabellFeltDatatypeBeskrivelsesportsidINTPrimærnøkkelsportsnameVARCHAR(100)Navn på sporten (f.eks. "Boksing")sportsiconVARCHAR(10)Emoji-ikonexercisesidINTPrimærnøkkelexercisessport_idINTFremmednøkkel til sportsexercisesnameVARCHAR(255)Navn på øvelsenexerciseslevelVARCHAR(50)beginner / intermediate / advancedexercisestypeVARCHAR(100)Teknikk, Forsvar, Kondisjon osv.exercisesdescriptionTEXTKort beskrivelseexercisesstepsTEXTJSON-array med steg
SQL-eksempel:
sqlCREATE TABLE sports (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  icon VARCHAR(10)
);

CREATE TABLE exercises (
  id INT AUTO_INCREMENT PRIMARY KEY,
  sport_id INT NOT NULL,
  name VARCHAR(255) NOT NULL,
  level VARCHAR(50),
  type VARCHAR(100),
  description TEXT,
  steps TEXT,
  FOREIGN KEY (sport_id) REFERENCES sports(id)
);

6. Programstruktur
kampklar/
 ├── app.py
 ├── templates/
 │    ├── index.html
 │    └── exercises.html
 ├── static/
 │    ├── style.css
 │    └── script.js
 ├── .env
 └── requirements.txt
Databasestrøm:
HTML (velg sport) → Flask (hent øvelser) → MariaDB → Flask → HTML (vis øvelser)

7. Kodeforklaring
app.py – Inneholder alle Flask-ruter:

GET / – Viser forsiden med alle tilgjengelige kampsporter hentet fra databasen.
GET /sport/<int:sport_id> – Henter og viser øvelser for valgt sport. Støtter valgfri query-parameter ?level=beginner for filtrering.
GET /api/exercises/<int:sport_id> – JSON-endepunkt for asynkron henting av øvelser (brukes av JavaScript).

Alle databasespørringer er parameteriserte for å forhindre SQL-injeksjon.

8. Sikkerhet og pålitelighet

.env – Databasepassord og hemmelig nøkkel lagres her, aldri i kildekoden.
Miljøvariabler – Lastes inn med python-dotenv ved oppstart.
Parameteriserte spørringer – Alle SQL-kall bruker %s-plassholdere.
Validering – Brukerinput (sport-ID, filternivå) valideres før det sendes til databasen.
Feilhåndtering – try/except-blokker rundt alle databasekall; brukervennlige feilmeldinger returneres ved feil.


9. Feilsøking og testing
Typiske feil og løsninger:
FeilÅrsakLøsningAccess denied for userFeil passord i .envSjekk .env-filen og MariaDB-brukertillatelserTable doesn't existGlemte å kjøre SQL-migreringenKjør CREATE TABLE-skriptene manueltjinja2.TemplateNotFoundFeil mappenavn på templateSørg for at filen ligger i templates/-mappen
Testmetoder:

Manuell testing i nettleseren for hvert endepunkt
Testet filtrering med ulike query-parametere
Verifisert at SQL-injeksjon ikke er mulig ved å prøve ' OR 1=1 -- som input


10. Konklusjon og refleksjon
Hva lærte du?
Jeg lærte hvordan Flask kobler seg til en database og sender data til HTML-maler via Jinja2. Jeg fikk også bedre forståelse for hvordan fremmednøkler strukturerer data i relasjonsdatabaser.
Hva fungerte bra?
Filtreringsfunksjonen med query-parametere fungerte godt og var enkel å implementere. Strukturen med separate tabeller for sport og øvelser gjorde det lett å legge til ny data.
Hva ville du gjort annerledes?
Jeg ville satt opp Docker fra starten av for et mer konsistent utviklingsmiljø, og skrevet automatiserte tester for rutene.
Hva var utfordrende?
Det var utfordrende å håndtere steps-feltet som JSON i databasen og parse det korrekt i Flask før det ble sendt til templaten.

11. Kildeliste

flask.palletsprojects.com
w3schools.com
mariadb.com/docs
python-dotenv dokumentasjon

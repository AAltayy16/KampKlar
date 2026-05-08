# KampKlar
1. Forside
Prosjekttittel: KampKlar

Navn: Arda

Klasse: 2IMI

Kort beskrivelse av prosjektet:
KampKlar er en nettapplikasjon for nybegynnere innen kampsport. Brukeren velger en kampsport som feks boksing, karate, judo, MMA, taekwondo eller BJJ og får opp relevante øvelser og teknikker tilpasset nybegynnernivå. Applikasjonen er bygget med Flask som backend og henter øvelsedata fra en MariaDB database. Jeg ønsker at alle kan bli komfortabel med kampsport og ikke trenger å bli redd for å starte det.

2. Systembeskrivelse
Formål med applikasjonen:
Målet er å gjøre det enklere å komme i gang med kampsport ved å samle grunnleggende øvelser og teknikker på ett sted. Brukeren skal slippe å lete rundt på ulike nettsider alt er organisert etter sport og vanskelighetsgrad.

Brukerflyt:
Brukeren åpner startsiden og velger en kampsport fra et rutenett.
Siden viser en liste med øvelser for valgt sport.
Brukeren kan filtrere øvelsene på nivå (nybegynner / videregående) eller type (teknikk, forsvar, kondisjon osv.).
Hvert øvelseskort kan ekspanderes for å vise steg for steg instruksjoner.

Teknologier jeg skal:
Python / Flask
MariaDB
HTML / CSS / JS

3. Server infrastruktur og nettverksoppsett
Servermiljø
Ubuntu VM / lokal maskin med Python og MariaDB installert.
Nettverksoppsett

IP-adresse: 10.200.14.12
Port: 3306

Viktig setup.
Klient (nettleser) - request - Flask (routes) - Python (logikk) - MariaDB (data) - tilbake via Flask - Jinja2
(HTML) - vises i nettlesern

4. Prosjektstyring GitHub Projects (Kanban)

Issues ble brukt til å spore enkeltfunksjoner og feil.
Refleksjon: Kanban tavlen hjalp med å holde oversikt over hva som gjensto og forhindret at jeg jobbet på for mange ting samtidig. Jeg er forstatt ikke ferdig med den og skal fortsette med dokumentasjonen.


6. Programstruktur
kampklar/
__pychache__
static/ style.css
templates/ base.html, index.html, sport.html.
venv
.env
.gitignore
app.py
requirements.txt

Databasestrøm:
HTML (velg sport) → Flask (hent øvelser) → MariaDB → Flask → HTML (vis øvelser)

8. Kodeforklaring
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
Under utviklingen av nettsiden møtte jeg på utfordringer som "Internal server error" og "SQL" Ved å bruke Flask sin debug-modus klarte jeg å tolke "Internal Server Error" meldinger for å rette opp i skrivefeil og manglende filer, mens SQL-feil lærte meg å være nøyaktig med tabellnavn og databasetilkoblinger. For å sikre at løsningen ble stabil, testet jeg systemet ved å navigere gjennom alle ruter, verifisere at nye data fra MySQL dukket opp umiddelbart på nettsiden, og kontrollere at designet var responsivt. Til slutt bekreftet jeg sikkerheten ved å sjekke at privat opplysninger i .env-filen ikke ble lastet opp til GitHub, noe som beviste at versjonskontrollen og .gitignore fungerte som planlagt

10. Konklusjon og refleksjon
Hva lærte du?
Jeg lærte hvordan Flask kobler seg til en database og sender data til HTML-maler via Jinja2. Jeg fikk også bedre forståelse for hvordan fremmednøkler strukturerer data i relasjonsdatabaser.

Hva fungerte bra?
Filtreringsfunksjonen med query-parametere fungerte godt og var enkel å implementere. Strukturen med separate tabeller for sport og øvelser gjorde det lett å legge til ny data. Det som også gikk bra var database fra terminalen, det er veldig gøy og kult å jobbe med, jeg fikk lage databasen, tabeller, og håndtere forskjellig data på en bra og oversiktlig måte.

Hva ville du gjort annerledes?
Jeg ville satt opp prosjekt basen fra starten av for et mer konsistent utviklingsmiljø, og skrevet automatiserte tester for rutene.

Hva var utfordrende?
Det som var utfordrende var å skrive ned python kodene på hodet, det klarte jeg desverre ikke. Jeg måtte gå tilbake til tidligere prosjekter og teams veiledninger for hjelp.



12. Kildeliste

flask.palletsprojects.com
w3schools.com
mariadb.com/docs
python-dotenv dokumentasjon
gemini (KI)


Huskekode

  id INT AUTO_INCREMENT PRIMARY KEY NOT NULL,
  name VARCHAR(100) NOT NULL,
  icon VARCHAR(10),
  description TEXT

  


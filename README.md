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

3. Server infrastruktur og nettverksoppsett
Servermiljø
Ubuntu VM / lokal maskin med Python 3.x og MariaDB installert.
Nettverksoppsett

IP-adresse: 10.200.14.12
Port: 3306

Klient (nettleser) - request - Flask (routes) - Python (logikk) - MariaDB (data) - tilbake via Flask - Jinja2
(HTML) - vises i nettlesern

4. Prosjektstyring GitHub Projects (Kanban)

Issues ble brukt til å spore enkeltfunksjoner og feil.
Refleksjon: Kanban-tavlen hjalp med å holde oversikt over hva som gjensto og forhindret at jeg jobbet på for mange ting samtidig.


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

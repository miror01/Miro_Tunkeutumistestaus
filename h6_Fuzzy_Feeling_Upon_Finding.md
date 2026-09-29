## x) Hoikkala 2026: Fuzzing with ffuf
* ffuf on monitoiminen HTTP-bruteforce-fuzzaukseen tarkoitettu työkalu.
* Sillä voidaan lähettää suuria määriä pyyntöjä kohteisiin, ja tarkastella vastauksia poikkeavuuksien löytämiseksi.
* Pyynnöt ovat hyvin kustomoitavissa, ja itse työkalu on suunniteltu keskittymään joustavuuteen.
* Pyynnöissä käytetään sanalistoja, jotka voivat olla joko tiedostoja, tai STDIN-syötteitä.
* Fuzzauksessa voi targettaa mitä vaan kohtaa HTTP-pyynnöstä, muun muassa URL:ää, HTTP-headeria, tai dataa.


## a) Vaultline
### Säännöt

Scope. Mikä on kohde?
* Tehtävässä harjoituskohteeksi on annettu itse ffuf nettisivu osoitteessa `ffuf.io.fi`.

Rules of engagement. Mitä sille saa tehdä, eli mitä tai millaisia menetelmiä saa käyttää?
* ffuf-nettisivulle saa tehdä ihan minkälaista ffuf-testausta vain haluaa. Mikään nettisivulla ei ole itsensä mukaan aitoa, tai herkkää.

Mihin oikeutesi tehdä tietoturvatestausta tähän kohteeseen perustuu?
* Oikeuteni testata nettisivua perustuu nettisivun tekijän nettisivulle kirjoitettuun lupaan.

Riskit ja mitigointi.
* Liian suuresta määrästä HTTP-pyyntöjä palvelin voi kuormittua, joten olisi suositeltavaa rajoittaa fuzzaamisen nopeutta ffufin `-rate`-asetuksella.
* Emme myöskään halua vahingossakaan päätyä Scope-alueen ulkopuolelle, joten on suositeltavaa tarkistaa huolellisesti kohdeosoite, ja tehdä testausta vain harjoituksissa määriteltyjen menetelmien mukaan.


## b) ffuf
Harjoituksessa tarvittavaan ominaisuuteen tarvitaan ffuf-versiota 2.3.0, tai uudempaa. Ohjeiden mukaan tarkistin oman versioni komennolla `ffuf -v`. Versioni oli 2.1.0, joten menin ffuf:in github repositorion kautta asentamaan kaliin uusimman version. Sopiva versio kaliini oli `ffuf_2.3.0_linux_amd64.tar.gz`. Purin tiedoston komennolla `tar -xzf ffuf_2.3.0_linux_amd64.tar.gz`.

<img width="627" height="472" alt="image" src="https://github.com/user-attachments/assets/abb425c7-fecb-41be-9ef8-af906e7750f0" />


Siirsin uuden ffuf version `/usr/local/bin`-hakemistoon komennolla `sudo mv ffuf /usr/local/bin`. Tarkistin ffuf-version, sekä preflight ominaisuudet ohjeiden mukaan komennolla `ffuf -h | grep -c preflight` ja kaikki pelaa kuten pitää.

<img width="384" height="336" alt="image" src="https://github.com/user-attachments/assets/31d889bc-fa4a-48f9-bb62-5837c844a87b" />

## c1) Content discovery
### Goal: Find the paths that exist but are not linked from anywhere.

Alotin koko tehtävän ffuf.io.fi -nettisivun ohjeiden "Getting Started"- osion mukaan. Ekoina komentoina syötin kaliin:
```bash
curl -O https://ffuf.io.fi/wordlists/content.txt
curl -O https://ffuf.io.fi/wordlists/passwords.txt

ffuf -w content.txt -u https://ffuf.io.fi/FUZZ
```
Komennot toimi ongelmitta ja viimeisestä komennosta kaliin tuli tuloste:

<img width="1266" height="970" alt="image" src="https://github.com/user-attachments/assets/24cef06f-c233-4bcc-9c00-d36da15e01ee" />

Erittäin paljon tuloksia, suurin osa niistä olemattomia. Suurimmassa osassa tuloksista on sanoja 135, joten on pääteltävissä, että nämä ovat niitä tuloksia poluista joita ei ole edes olemassa, joten filtteröin seuraavaksi ne pois lisäämällä komentoon parametrin `-fw 135`.

<img width="1284" height="763" alt="image" src="https://github.com/user-attachments/assets/506251b9-eb7f-4216-a3c5-1b15aa49b25a" />

Tulokset vähentyi huimasti, 2000 tuloksesta jäljelle jäi 14. Nämä ovat niitä oikeasti olemassa olevia polkuja.

## c2)

Ohjeissa näkyy, että oletusasetuksilla ffuf löytää vain tiettyjä vastauskoodeja. 2 kiinnostavaa polkua ei palauta 200, ja toinen niistä ei palaudu lainkaan oletusasetuksia käyttävällä ffuf ajolla. Täten käyttöön tulee uusi parametri, `-mc`, ja `-fc`. Käytin komentona `ffuf -w content.txt -u https://ffuf.io.fi/FUZZ -mc all -fc 200` ja sain tällaisen tuloksen:

<img width="1195" height="895" alt="image" src="https://github.com/user-attachments/assets/4faa1c57-900a-4e0d-978c-c09ea6fa0ac0" />

Eli haetaan kaikki response status-koodit, ja filtteröidään pois ne joiden status on 200. Jäljelle jäi 6.

## c3)

Yritin jatkaa ohjeiden mukaan. tässä kohdassa tulee 2 uutta parametriä, -recursion, sekä -recursion-depth, joten kokeilin käyttää niitä yhdessä -fw 135 parametrin kanssa. Uusia tuloksia tuli muutama, kuten `db.sql.bak`.

<img width="766" height="727" alt="image" src="https://github.com/user-attachments/assets/822e7610-8cd7-4d1e-ba48-5005dafa9a58" />


## c4)

C4-tehtävässä siirrytään testaamaan fuzzausta URL:in sijasta HTTP-pyynnön headeriin. Poistin aiemmin käytetystä komennosta FUZZ-kohdan, koska emme enää fuzzaa URL:ia. Komentona oli siis nyt `ffuf -w content.txt -u https://ffuf.io.fi/ -H "Host: FUZZ.ffuf.io.fi" -fw 135`. Käytin tässä heti -fw 135- parametriä, sillä halusin filtteröidä edelleen kaikki virheelliset tulokset pois. Huomasin kuitenkin, että tuo 135 sanaa ei enää päde, ja tuloksia tuli silti kaikki 2000.

<img width="865" height="498" alt="image" src="https://github.com/user-attachments/assets/576e9b5f-e074-44c5-a716-cdf8ac3109ea" />

Kuvasta näkee, että Words osio koostuu nyt laajalti 377 sanasta, joten muutin `-fw` parametrin siihen ja ajoin komennon uudestaan. Tuloksena tuli pelkkä admin. 

<img width="850" height="496" alt="image" src="https://github.com/user-attachments/assets/993ab961-b004-4a84-8be6-1c0111e866f4" />

Hämmennyin, koska tehtävässä sanotaan, että pitäisi löytyä 3 eri tulosta. Koitin myös passwords.txt-listaa, ja koitin kysyä tekoälyltä apua, joka oikeastaan vain valitti, että ei saa näyttää tuloksia kyberturvallisuusriskien vuoksi, lukuisista selvennyksistä huolimatta, että tämä on koulutehtävä ja testaukseen sallittu ympäristö. En tiedä miten tehtävässä kuuluisi enää edetä.

## c9)

Valitettavasti tämä oli minulle jo aivan käsittämätöntä, joten tein tehtävän ohjekirjan ja tekoälyn avulla. ffuf.io.fi- sivulta löytyy C9:ään nämä ohjeet: 
```bash
cat > login.raw <<'EOF'
GET /login HTTP/1.1
Host: ffuf.io.fi
Accept: text/html

EOF

ffuf -w passwords.txt -u https://ffuf.io.fi/login -X POST \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "csrf_token=CSRFTOKEN&username=admin&password=FUZZ" \
  -preflight login.raw \
  -preflight-var 'CSRFTOKEN:name="csrf_token" value="([a-f0-9]+)"' \
  -preflight-mode per-request \
  -mc 302
```
Joten kokeilin niitä.

<img width="826" height="685" alt="image" src="https://github.com/user-attachments/assets/07bb69b6-8b14-47af-a5d0-d231e8884bdc" />

Jäin jumiin, enkä saa tehtävää suoritettua. Tekoälystäkään ei ole apua, kun tuloste on joka kerta tämä.

<img width="1053" height="565" alt="image" src="https://github.com/user-attachments/assets/84f62c0e-fe75-4f54-be87-648856203a24" />

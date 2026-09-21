## x) Tiivistelmä

### Karvinen 2022: Cracking Passwords with Hashcat
* Järjestelmät ei tallenna alkuperäisiä salasanoja, sen sijaan järjestelmät tallentaa salasanat hasheina, eli tiivisteinä.
* Tiivisteet ovat yksisuuntaisia, eli kun tiiviste on tehty, sitä ei voi suoraan kääntää takaisin tiivisteen perusteella.
* Löytyy kuitenkin ohjelmia kuten hashcat, joilla voidaan hakea miljoonia eri sana, numero, ja kirjainyhdistelmiä, ja näistä palautuvaa tiivistettä voidaan verrata tiivisteeseen joka halutaan murtaa.
* Hashcatin kaltaisia ohjelmia kannattaa käyttää yhdessä suurien sanakirjojen kanssa, joissa voi olla miljoonia eri yhdistelmiä, kuten Rockyou.txt, jossa on 14 miljoonaa eri sanaa.
* Jotta tiiviste voidaan murtaa, täytyy tietää tiivisteen tyyppi. Tiivistetyypin selvittämistä varten löytyy ohjelmia kuten `hashid`.
* Jos salasana murretaan onnistuneesti, hashcat palauttaa tilaksi: `Cracked`. Tämän jälkeen salasanan voi tallentaa tekstitiedostoon tai näyttää komennolla `--show`. Jos salasanaa ei löydy, hashcat palauttaa vastaukseksi `Exhausted`.

#### Oma ajatus: Onneksi ei ole realistinen uhka, että oma salasana tulisi murretuksi, jos salasanan monipuolisuuteen on nähty vähänkään vaivaa eikä se ole vain tyyliä "kissa123".


### Karvinen 2023: Crack File Password With John
* Kuten käyttäjätunnukset, myös monet tiedostomuodot, kuten 7zip, ZIP, ja PDF voidaan suojata salasanoilla.
* Tiedostojen salasanojen murtamiseen löytyy ohjelmia kuten John The Ripper, joka hyödyntää myös salasanan selvittämisessä sanakirjoja.
* John The Ripper vaatii lukuisia ohjelmistoja toimiakseen, komento kalissa näyttääkin suurinpiirtein tältä: `sudo apt-get -y install micro bash-completion git build-essential libssl-dev zlib1g zlib1g-dev zlib-gst libbz2-1.0 libbz2-dev atool zip wget`.
* Artikkelissa asennetaan John The Ripperin laajennettu Jumbo-versio. Se tukee useampia tiiviste- ja tiedostotyyppejä, kuten salattuja  7zip, ZIP, ja PDF- tiedostoja.

## a) Asenna Hashcat ja testaa sen toiminta murtamalla esimerkkisalasana.

Ensiksi asennetaan hashcat Kaliin. Komentona `sudo apt update` ja `sudo apt install hashcat`.

Seuraavaksi testasin hascatin toiminnan murtamalla testisalanan. Käytin salasanana jo aiemmin artikkelissa murrettua sanaa `summer`. Tein salasanasta MD5-tiivisteen joka tulostuu uuteen tekstitiedostoon komennolla `echo -n "summer" | md5sum > tiiviste.txt`.

<img width="190" height="54" alt="h5_2" src="https://github.com/user-attachments/assets/b1ccee87-4d83-4f72-92aa-a19985ffc273" />


Komennossa on tärkeää käyttää parametriä `-n`, sillä se estää rivinvaihdon lisäyksen sanan loppuun. Käytännössä ilman parametriä, echo-komento lisäisi itse sanan perään rivinvaihtomerkin niin, että tiivistettävä teksti/data olisi oikeasti `summer\n`, ja ei täten enää toimisi tarkoitukseen, kun tiiviste muuttuu aivan täysin.


Tarkistin tiivisteen cat-komennolla. md5sum lisää tiivisteen perään välilyöntejä ja -merkin, jotka ovat myös tiivisteeseen vaikuttavaa tietoa, joten poistin ne nanolla.

<img width="233" height="122" alt="h5_3" src="https://github.com/user-attachments/assets/1055394b-34ae-4ca1-a170-ca635986f4bb" />


Tarkistin `hashid`- komentoa tarkistaakseni todennäköisimmät tiivistetyypit tiivisteelle. Tiedän itse, että tyyppinä on MD5, kun juuri itse loin tiivisteen, mutta tällä komennolla siitä otettaisiin tositilanteessa jossa tiiviste ei ole itse tehty, selvää.


<img width="288" height="193" alt="h5_4" src="https://github.com/user-attachments/assets/cc0e3873-e998-4e94-89b1-c9467d8fbc6b" />


Valmiissa Kalissa on yleensä rockyou.txt valmiina, ja sen voi tarkistaa komennolla `ls usr/share/wordlists`. löysin sieltä `rockyou.txt.gz`, joka meinaa, että tiedosto löytyy, mutta pakattuna. Ekana purin sen komennolla `sudo gzip -d`.

<img width="617" height="123" alt="image" src="https://github.com/user-attachments/assets/e3ef50a8-46b8-47af-9bf3-638aa201516a" />

On aika ottaa hashcat käyttöön. Komentona toimii `hashcat -m 0 tiiviste.txt /usr/share/wordlists/rockyou.txt`. 

Komennossa on 4 osaa:
* `hashcat` kertoo käytettävän ohjelman.
* `-m 0` kertoo tiivisteen tyypin. Aiemmasta komennosta saatiin selville, että MD5 vastaa numeroa 0.
* `tiiviste.txt` on meidän tekstitiedosto jossa tiiviste sijaitsee
* `/usr/share/wordlists/rockyou.txt` on sanakirja, jota tässä tehtävässä haluamme käyttää.


<img width="362" height="194" alt="image" src="https://github.com/user-attachments/assets/442916c2-386c-4e5b-bfad-72dfd16e7e28" />

Komento ei toimi. Koska teen harjoitusta virtualboxissa, toteutuksessa tulee mutkia matkaan siinä miten hashcat haluaisin käyttää näyönohjainta prosessorin sijaan, muttei pysty. Korjasin seuraavaksi hashcatin niin, että se pystyy käyttää prosessoria harjoitukseen. Komentona toimi `sudo apt install pocl-opencl-icd`.

Ajoin komennon `hashcat -m 0 tiiviste.txt /usr/share/wordlists/rockyou.txt` uudestaan, ja tällä kertaa toimi. Salasana "summer" on niin helppo kohde että suorituskyky ei tässä harjoitustehtävässä vaikuta niin paljoa että haittaisi käyttää CPU:ta.

<img width="472" height="340" alt="image" src="https://github.com/user-attachments/assets/72f2faa1-db09-4893-95a9-765be92bb0ae" />


Näytetään tulos käyttämällä aiemmin mainittua `--show` parametriä, eli `hashcat -m 0 tiiviste.txt --show`. Kuten näkyy, tiiviste sekä salasana ovat selvillä.

<img width="270" height="63" alt="image" src="https://github.com/user-attachments/assets/30b44e5d-a443-4597-ae41-f5bc12e601c2" />



## c) Asenna John the Ripper ja testaa sen toiminta murtamalla jonkin esimerkkitiedoston salasana.

Alotin asentamalla kaikki John The Ripperiin vaadittavat ohjelmistot. Ohjeista löytyvä `zlib-gst` ei enää toimi, joten jätin sen pois. 

<img width="566" height="337" alt="image" src="https://github.com/user-attachments/assets/a0383a8f-24f7-4fb8-974f-b8822ecb7007" />

Seuraavaksi kloonasin JtR-Jumbo-version Openwallin repositoriosta. Komentona `git clone https://github.com/openwall/john.git`. Ja siirryin `cd`-komennolla src-hakemistoon. Sueraavaksi komento `./configure` ja JtR on konfiguroitu. Vielä viimeiseksi komennoksi `make -s clean && make -sj2` ja se pitäisi hetken kasaamisen jälkeen olla valmis.

<img width="305" height="71" alt="image" src="https://github.com/user-attachments/assets/6f95e464-19e3-4d2d-bd0a-90210977cde4" />

Suorittamiseen meni pari minuuttia. Seuraavaksi siirryn hakemistoon run komennolla `cd ../run` kirjoitin komennon `./john` joka käynnisti JtR:n.

<img width="467" height="81" alt="image" src="https://github.com/user-attachments/assets/abca5ed6-2aea-4a5d-a353-009fbd2868ca" />


Seuraavaksi latasin tero.zip- testitiedoston ja tarkistin että se tuli.

<img width="619" height="175" alt="image" src="https://github.com/user-attachments/assets/e8d772c2-b9db-4518-9514-daf55b4b1dfb" />

Kokeilin avata sen ja se oli tietenkin salasanasuojattu. Alotin murtamisen tekemällä ZIP:lle tiivisteen komennolla `./zip2john tero.zip > tero.zip.hash`. Annoin tiivisteen JtR:lle komennolla `./john tero.zip.hash`

<img width="638" height="215" alt="image" src="https://github.com/user-attachments/assets/371f36f5-2126-4cfe-bced-6b2efd4afc66" />

Tiiviste saatiin murrettua onnistuneesti ja salasanaksi paljastui `butterfly`.

<img width="460" height="227" alt="image" src="https://github.com/user-attachments/assets/1cc7910f-ca36-4885-9408-71b8b7ae282a" />

Sitten vielä koitin unzip uusiksi ja luin SECRET.md -tiedoston.

<img width="404" height="220" alt="image" src="https://github.com/user-attachments/assets/a1edf8e8-0e6e-4848-85ff-6e42b16ca7ab" />

## e) Tiedosto. Tee itse tai etsi verkosta jokin salakirjoitettu tiedosto, jonka saat auki. Murra sen salaus.

Valitsin formaatiksi 7z. Tein ekana echo-komennolla uuden tiedoston, ja salasanasuojasin sen 7z:llä.

<img width="369" height="196" alt="image" src="https://github.com/user-attachments/assets/a6050acc-33e8-4f6a-918b-1d0e8ed10f9c" />

Loin sille salasanan `sunshine`, joka löytyy rockyou-sanakirjasta.

Poistin alkuperäisen suojaamattoman tiedoston `rm Miro.txt`. Sitten muunsin 7z.tiedoston muotoon jonka JtR ymmärtää. Komentona `./7z2john.pl Miro.7z > Miro.hash`. Tarkistin tiivisteen komennolla `cat Miro.hash`.

Seuraavaksi aloin murtamaan salasanaa. Käytin JtR:ää ja sanalistana rockyou.txt:tä, ja salasana saatiin hyvin nopeasti selville.


<img width="589" height="182" alt="image" src="https://github.com/user-attachments/assets/b3df477f-aaf7-4d00-b20f-9ed239b8cee5" />

Purin tiedoston vielä sanasanaa käyttäen ja se onnistui. 

<img width="404" height="233" alt="image" src="https://github.com/user-attachments/assets/549f6349-463b-4eb7-af33-5e34bca986c3" />


## f) Tiiviste. Tee itse tai etsi verkosta salasanan tiiviste, jonka saat murrettua.

Käytin aiemmin MD5:tä, joten kokeilen nyt SHA-256-tiivistemuotoa. Tein ekana samalla tavalla kun ekassa tehtävässä SHA-256-tiivisteen käyttämällä sanaa joka sisältyy rockyou.txt:ssä. valitsin salasanaksi `mustang`. Samalla tavalla kun aiemmin, käytin nanoa poistamaan välilyönnit ja viivat.

<img width="537" height="107" alt="image" src="https://github.com/user-attachments/assets/d206f4de-3fe8-4813-8352-920e0ef3d509" />

`hashid`- komennolla taas selville tyyppi, ja SHA256:n hashcat numeroksi näyttäytyy 1400. 

Seuraavaksi laitoin komennon `hashcat -m 1400 sha256.txt /usr/share/wordlists/rockyou.txt`

<img width="532" height="382" alt="image" src="https://github.com/user-attachments/assets/a0e06695-05b8-4d7d-9cce-ab95e99b688c" />

Odotin että hascat suorittaa, ja tulokseksi tuli taas `Cracked`. Komento `hashcat -m 1400 sha256.txt --show` ja tuloksessa näkyy `mustang`.

<img width="371" height="233" alt="image" src="https://github.com/user-attachments/assets/9203c502-c625-4151-ba75-9361ac179588" />


## g) Demonstroi oman sanakirjan tekemistä Hashcatille tai Johnille.

Aloitin tehtävän luomalla sanakirjan komennolla `nano mironsanakirja.txt`, lisäsin sinne sanoja omille riveille ja tallensin. Nyt sanakirjaa voidaan käyttää samalla tavalla kuin rockyou.txt:tä aiemmissa tehtävissä.

<img width="179" height="152" alt="image" src="https://github.com/user-attachments/assets/6e23b6eb-5441-4893-9808-303717548456" />




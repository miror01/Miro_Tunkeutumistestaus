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




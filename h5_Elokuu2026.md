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


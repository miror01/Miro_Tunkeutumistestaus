## x) Tiivistelmä

### Karvinen 2022: Cracking Passwords with Hashcat
* Järjestelmät ei tallenna alkuperäisiä salasanoja, sen sijaan järjestelmät tallentaa salasanat hasheina, eli tiivisteinä.
* Tiivisteet ovat yksisuuntaisia, eli kun tiiviste on tehty, sitä ei voi suoraan kääntää takaisin tiivisteen perusteella.
* Löytyy kuitenkin ohjelmia kuten hashcat, joilla voidaan hakea miljoonia eri sana, numero, ja kirjainyhdistelmiä, ja näistä palautuvaa tiivistettä voidaan verrata tiivisteeseen joka halutaan murtaa.
* Hashcatin kaltaisia ohjelmia kannattaa käyttää yhdessä suurien sanakirjojen kanssa, joissa voi olla miljoonia eri yhdistelmiä, kuten Rockyou.txt, jossa on 14 miljoonaa eri sanaa.
* Jotta tiiviste voidaan murtaa, täytyy tietää tiivisteen tyyppi. Tiivistetyypin selvittämistä varten löytyy ohjelmia kuten `hashid`

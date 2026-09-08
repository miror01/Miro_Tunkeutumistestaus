## x) Jaswal 2020: Mastering Metasploit - 4ed: Chapter 1: Approaching a Penetration Test Using Metasploit

Artikkeli käsittelee penetraatiotestauksen vaiheita Metasploittia hyödyntäen. Luvussa selitetään selväksi perussanastoa kuten mitä ovat exploitit ja payloadit, sekä Metasploitista löytyvät auxiliary-apuohjelmat, 
jolla voi yhdistää monta vaihetta yhteen. Metasploit on todella hyödyllinen työkalu manuaalin porttiskannauksen sijaan, se mahdollistaa muun muassa suurien verkkojen skannaamisen kerralla.


## x) mitä nmap -sn tekee?

**nmap -sn** on komento, jonka hyödyllisyys piilee siinä, kun halutaan vaan selvitttää muun muassa verkon laajuus ja alue. Komento skannaa päällä olevia laitteita, ilman varsinaista porttiskannausta. Sille määritetään IP-osoitteita
ja sen tehtävä on tarkistaa mitkä osoitteista ovat päällä. Portteja ei tarkisteta.

Omassa lähiverkossa pyynnöt ovat ARP-pyyntöjä, joka on luotettava ja tehokas pyyntö löytämään aktiivset laitteet.

Selityksen löysin nmap:in opaskirjasta, merkattu lopuss lähteisiin.

Luotettavuus on pääteltävissä siitä, että nmap on yli 25 vuotta vanha maailmanlaajuisesti käytetty ja hyväksytty avoimen lähdekoodin työkalu asiantuntijoiden keskellä, ja opaskirjan on kirjoittanutt Nmapin alkuperäinen kehittäjä.


## b)

Aivan ensiksi otin ylös metasploitable-koneen IP-osoitteen. Kirjautuminen käyttäjättunnuksella sekä salanalla: **msfadmin**. Seuraavaksi komento **ip addr**. 

Siiryin Kalille, varmistin että verkko on Host-only, ja käynnistin Metasploitin tietokannan ja Konsolin. Siirryin consoleen komennolla **sudo msfconsole**, ja kokeilin **db_status**.


<img width="465" height="333" alt="h3_1" src="https://github.com/user-attachments/assets/62ccd3f0-7b52-4169-849d-0c564f6d53dd" />


Ajoin seuraavaksi skannauksen msf:ssä Metasploitin IP-osoitteeseen. Komentona **db_nmap -sV 192.168.60.4** näemme Metasploitablessa avoinna olevat portit.


<img width="571" height="305" alt="h3_2" src="https://github.com/user-attachments/assets/137484f3-15bb-4a7b-ae8f-add79b59d1da" />


## c)


Koska db_nmap ehtiin msf:ssä, sen tulokset tallentui itsestään tietokantaan. Tarkastellaan tuloksia. komento **hosts**.


<img width="514" height="113" alt="h3_3" src="https://github.com/user-attachments/assets/c9f5d88e-70c3-4b63-9257-8cf0d7c48599" />

Näemme että tietokannassa on vain yksi kone, itse Metasploitable. Komento **services** näyttää avoinna olevat portit ja palvelut, sekä tarkempi parametri, kuten **"-p 21"**, näyttää portissa 21 toimivan palvelun tietoja.


<img width="454" height="315" alt="h3_4" src="https://github.com/user-attachments/assets/3cd9cbd8-2908-4f9e-813e-c348f9b510b4" />


**services -S http** komennolla voimme tarkastella http:llä toimivia palvelimia.


<img width="410" height="97" alt="h3_5" src="https://github.com/user-attachments/assets/61081163-6f91-44cc-ace2-26d7186ddf7d" />




## d)

Ekana suoritin Kalissa yksinkertaisen **search vsfptd** komennon. Se löysi auxiliaryn ja exploitin, jotka liittyvät vsfptd-palveluun.


Seuraavaksi komento **use (exploitin nimi)**.


<img width="470" height="136" alt="h3_6" src="https://github.com/user-attachments/assets/66d38de0-7a9f-46c2-b6c1-e30e1325b978" />


Seuraavaksi määritin kohde IP-osoitteeksi Metasploitablen osoitteen, ja tarkistin että kaikki on kunnossa,


<img width="577" height="89" alt="h3_7" src="https://github.com/user-attachments/assets/cf5a635a-fb98-4918-9c8d-655696fed91f" />




Sitten asetetaaan LHOST eli Local Host Kalin IP-osoitteeseen, suoritetaan hyökkäys. Komentona **exploit**.


<img width="523" height="139" alt="h3_8" src="https://github.com/user-attachments/assets/7b5debbf-e8bd-4f78-9673-806bb2b99cd9" />


Näemme että olemme oikeassa järjestelmässä ja käytössämme ovat root-oikeudet.


## e)

Suoritetaan skannaus nyt nmap:n omalla tiedoston tallennusformaatilla. Komentona käytin **nmap -sV -oA skannaus (ip-osoite)**. Skannaus tallentuu nmapin oletusasetusten mukaan kolmeen eri tiedostoon näillä flageilla.

Nämä ovat skannaus.xml, skannaus.nmap, skannaus.gnmap. Tiedostomuodot ovat hyviä siitä, että ne eivät riipu tietokannoistta ja niitä on hyvin helppo varmuuskopioida, jakaa, ja säilöä.

Itse komento db_nmap ei tee tätä, vaan tulokset tallentuu automaattisesti PostgreSQL-tietokantaan. Tämä on hyvä koska se voi helpoittaa ja nopeuttaa työskentelyä.

nmap-skannaustulokset tiedostosta voi importata msf:ään, siirtymällä takaisin terminaaliin jossa metasploit on kalissa auki, ja kirjoittamalla **db_import skannaus.xml**. Tuloksen voi tarkistaa taas **hosts** ja **services**.


<img width="613" height="380" alt="h3_9" src="https://github.com/user-attachments/assets/06da286c-6aa1-43ca-9d32-0191c4373184" />




## f) 

Murtauduin ja demonstroin toimintaa jo aiemmissa vaiheissa käyttäen vsftpd-palvelua. käytin Moduulia **vsftpd_234_backdoor** kuten kuvista näkyy. Raportissa löytyy kuva siitä, kun sessio päästiin avaamaan, ja Root-oikeuksien toimivuus todisttettiin.


## g)

Aktivoidaan uusi sessio, ja hyödynnetään komentoja kuten **sysinfo**, **ifconfig**, **netstat**, **route**. Nämä auttaa levittäytymisessä. 

**sysinfo** kertoo järjestelmän version. Hyödyllistä haavoitttuvuuksien etsimisessä.
**ifconfig** ja **route** kertoo tärkeää sekä hyödyllistä tietoa verkkokorteistta ja verkoista. Tilanteessa, jossa verkkoja on useampi, voidaan niitä käyttää hyödyksi päästäkseen verkkoon, jotai Kali itse ei suoraan nähnyt.

**netstat** näytttää meille aktiivisia yhteyksiä, sekä auki olevia portteja. Paljastaa palveluita ja laitteita.




































## Lähteet

Nmap Reference Guide, Host Discovery: https://nmap.org/book/man-host-discovery.html

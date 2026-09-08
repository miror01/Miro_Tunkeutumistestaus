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





























## Lähteet

Nmap Reference Guide, Host Discovery: https://nmap.org/book/man-host-discovery.html

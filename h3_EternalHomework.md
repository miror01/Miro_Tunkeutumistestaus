## x) Jaswal 2020: Mastering Metasploit - 4ed: Chapter 1: Approaching a Penetration Test Using Metasploit

Artikkeli käsittelee penetraatiotestauksen vaiheita Metasploittia hyödyntäen. Luvussa selitetään selväksi perussanastoa kuten mitä ovat exploitit ja payloadit, sekä Metasploitista löytyvät auxiliary-apuohjelmat, 
jolla voi yhdistää monta vaihetta yhteen. Metasploit on todella hyödyllinen työkalu manuaalin porttiskannauksen sijaan, se mahdollistaa muun muassa suurien verkkojen skannaamisen kerralla.


## x) mitä nmap -sn tekee?

**nmap -sn** on komento, jonka hyödyllisyys piilee siinä, kun halutaan vaan selvitttää muun muassa verkon laajuus ja alue. Komento skannaa päällä olevia laitteita, ilman varsinaista porttiskannausta. Sille määritetään IP-osoitteita
ja sen tehtävä on tarkistaa mitkä osoitteista ovat päällä. Portteja ei tarkisteta.

Omassa lähiverkossa pyynnöt ovat ARP-pyyntöjä, joka on luotettava ja tehokas pyyntö löytämään aktiivset laitteet.

Selityksen löysin nmap:in opaskirjasta, merkattu lopuss lähteisiin.

Luotettavuus on pääteltävissä siitä, että nmap on yli 25 vuotta vanha maailmanlaajuisesti käytetty ja hyväksytty avoimen lähdekoodin työkalu asiantuntijoiden keskellä, ja opaskirjan on kirjoittanutt Nmapin alkuperäinen kehittäjä.
































## Lähteet

Nmap Reference Guide, Host Discovery: https://nmap.org/book/man-host-discovery.html

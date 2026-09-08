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


Koska db_nmap ehtiin msf:ssä, sen tulokset tallentui itsestään tiettokantaan. Tarkastellaan tuloksia. komento **hosts**.


<img width="514" height="113" alt="h3_3" src="https://github.com/user-attachments/assets/c9f5d88e-70c3-4b63-9257-8cf0d7c48599" />

Näemme että tietokannassa on vain yksi kone, itse Metasploitable. Komento **services** näyttää avoinna olevat portit ja palvelut, sekä tarkempi parametri, kuten **"-p 21"**, näyttää portissa 21 toimivan palvelun tietoja.


<img width="454" height="315" alt="h3_4" src="https://github.com/user-attachments/assets/3cd9cbd8-2908-4f9e-813e-c348f9b510b4" />






















## Lähteet

Nmap Reference Guide, Host Discovery: https://nmap.org/book/man-host-discovery.html

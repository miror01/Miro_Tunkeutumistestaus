## x) Tiivistelmä
### OWASP TOP 10 2021: Broken Access Control
* OWASP on järjestö jonka missio on parantaa verkkoturvalisuutta.
* OWASP:in Top 10 on hyvin tunnettu lista verkkosoovellusten tietoturvan 110 suurinta ja yleisintä riskiä.
* Broken Access Control on verkkosovellusten yleisin riski. Artikkelin mukaan 94% sovelluksista löytyi jonkin näköinen haavoittuvaisuus pääsynhallinnassa.
* Riskiluokka on erittäin laaja, ja se mahdollistaa monia eri hyökkäystapoja, kuten Path Traversal ja IDOR.


### Insecure Direct Object References (IDOR)

* IDOR on osa Broken Access Control-luokkaa, joka tapahtuu kun sovellus ei tarkista käyttöoikeuksia käyttäjän syötteestä, vaan käyttää syötettä suoraan.
* Luvattomiin tietoihin pääsy tulee helpoksi, kun käyttäjä voi yksinkertaisesti vaan muokata syötteen tunnusta niin, että se tulisi joltain muulta.


### Path Traversal

* Path Traversalin avulla hyökkääjä pystyy lukemaan hänelle kuulumattomia tiedostoja suoraan palvelimen järjestelmästä.
* Hyökkäyksen ideana on hakemistojen kanssa vääränlaisten toimivien navigointimerkkien syöttämiseen, joiden ansiosta ohjelmia voidaan avata niille suunniteltujen kansioiden ulkopuolella, ja päästään käsiksi järjestelmän tietoihin.


### Cross-Site Scripting (XSS)
* Verkkosovelluksella on puutteellinen syötteentarkistus, ja se ottaa vastaan kaikkea käyttäjän syötettä, ja täten voidaan käyttää hyväksi.
* Mahdollistaa sen, että hyökkääjä voi päästä käsiksi uhrin tietoihin, kun verkkosovellus ajaa haitallista koodia uhrien selaimissa.



## a)

Ensiksi asensin Kaliin OWASP ZAP:in ja käynnistin sen komennolla **zaproxy**.

<img width="1252" height="613" alt="image" src="https://github.com/user-attachments/assets/f3f2afc2-2ff9-4407-bb92-75b323f55470" />

Ekaksi ZAP:ssa ohjeiden mukaan muutin asetuksia, niin että kuvat näkyy. Tools -> Options -> Display -> Process images in HTTP requests/responses.

Seuraavaksi, Tools -> Options -> Network -> Server Certificates ja sieltä Save.

<img width="713" height="344" alt="image" src="https://github.com/user-attachments/assets/3d5b206f-3346-4100-ad0b-f6a786180195" />


Sitten lisäsin sertifikaatin firefoxiin. Firefoxissa asetuksista kohta Certificates, tällä View Certificates ja Importtataan meidän uusia äsken luoma sertifikaaattii firefoxiiin.
 

<img width="616" height="322" alt="image" src="https://github.com/user-attachments/assets/d453c694-9fae-4ecf-8f53-983dc34582e1" />






## Lähteet)

OWASP TOP10 2021 Broken Access Control: https://top10.owasp.org/2021/A01_2021-Broken_Access_Control/

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


Sitten lisäsin sertifikaatin firefoxiin. Firefoxissa asetuksista kohta Certificates, tällä View Certificates ja Importtataan meidän uusia äsken luoma sertifikaatti firefoxiiin.
 

<img width="616" height="322" alt="image" src="https://github.com/user-attachments/assets/d453c694-9fae-4ecf-8f53-983dc34582e1" />

Sitten muutetaan ohjeiden mukaan firefoxista proxy-asetuksia. Laiton hakuriviin about:config ja sitten network.proxy.allow_hijacking_localhost, vaihdoin sen arvoon True.

<img width="702" height="184" alt="image" src="https://github.com/user-attachments/assets/2517a64f-2740-4d06-b06e-a4cc57ec4d27" />

Seuraavaksi muutetaan proxy-asetuksista uusi manuaalinen ZAP proxy. Osoitteena local host 127.0.0.1 ja porttina 8080. Tarkistin toimivuuden avaamalla uuden välilehden firefoxissa ja tarkastamalla ZAP:i, kaikki toimii kuten pitää. 

<img width="491" height="415" alt="image" src="https://github.com/user-attachments/assets/f8116e7f-324f-4dd4-bf64-01681c1f74c4" />


## b)

Lisäsin ensiksi firefoxiin FoxyProxy Standard-lisäosan. Sitten avasin FoxyProxyn firefoxissa, ja lisäsin Options:in Proxy-kohdasta uuden proxy-profiilin.


<img width="860" height="375" alt="h4" src="https://github.com/user-attachments/assets/a170c756-548d-4dba-81a3-92781ed342b5" />


Asetin Proxylle sääntöjä (patterns), eli määritin osoitteet jotka haluan kulkevan ZAP:in läpi. Lisäsin osoitteet http://localhost*, https://portswigger.net*, sekä *web-security-academy.net*


<img width="632" height="376" alt="h4-1" src="https://github.com/user-attachments/assets/35bcb56c-5cde-4a1c-a778-d0243df31a56" />


Testasin toimivuuden. FoxyProxy "Proxy by patterns"- tilaan. Kävin wikipediassa, ja siitä ei tullut ZAP:iin mitään, localhostista tuli, toimii.


<img width="401" height="342" alt="h4_2" src="https://github.com/user-attachments/assets/1728a561-c560-494e-a8e6-09abb09e2ff1" />



## c) Reflected XSS into HTML context with nothing encoded

### Reflected XSS into HTML context with nothing encoded

Laitoin labrassa testisyötteen. Sain videoiden ja vinkkien kautta nähdä, että ZAP:ssä tarkastellen verkkopyyntöjä ja niiden vastauksia, kyseisessä labrassa kaikki pyynnöt menevät otsikkoelementtiin.

<img width="583" height="238" alt="h4_3" src="https://github.com/user-attachments/assets/cd336f45-00e5-4ef9-a2af-ea7037b1aab8" />

Tämä tarkoittaa, että kaikki käyttäjän syötteet menevät suoraan osaksi sivun ajamaa koodia, ja tätä voidaan käyttää pahoihin tarkoituksiin. [PortSwiggerin Cross Site Scripting](https://portswigger.net/web-security/cross-site-scripting/preventing)- sivulta löysin, että normaalitapauksessa, ilman haavoittuvuuksia, verkkosovellus käsittelee syötteen ja enkoodaa sen turvalliseksi ennen sen näyttämistä, jotta selain ei luule syötettä koodiksi.


<img width="919" height="417" alt="image" src="https://github.com/user-attachments/assets/e9adaf79-3a13-4ea1-915c-7b8296e86ef8" />


Tätä voidaan käyttää hyväksi lähettämällä syötteeseen koodia. Koska sitä ei tarkisteta labrassa kunnolla, se menee läpi sivustolle, ja sivu ajaa koodin.

Laitoin nyt syötteeseen `<script>alert (1)</script>`. Sivusto ajaa koodin suodattamatta sitä, ja alert ilmestyy etusivulle.

<img width="532" height="171" alt="image" src="https://github.com/user-attachments/assets/a3e8c984-401a-49c3-95c3-e6ed257d6396" />

## d) Stored XSS into HTML context with nothing encoded

Avasin labran ja huomasin että se on hyvin samanlainen. Kuvauksessa sanottiin, että tehtävä on muuten aikalailla sama, mutta nyt hyödynnetään kommenttikenttää. Menin labran blogissa postaukseen johon voi kommentoida, ja käytin kommenttina samaa `<script>alert (1)</script>` kuin äskenkin.


<img width="434" height="286" alt="image" src="https://github.com/user-attachments/assets/ad97293a-6632-4c1c-be2a-5f8082d43c51" />


<img width="511" height="180" alt="image" src="https://github.com/user-attachments/assets/44af3977-a6af-4517-85f0-414ba0445385" />



Tehtävä oli oikein, mutta alerttia ei tullut, ja syy tähän piilee siinä, että

















## Lähteet)

OWASP TOP10 2021 Broken Access Control: https://top10.owasp.org/2021/A01_2021-Broken_Access_Control/

Terokarvinen.com https://terokarvinen.com/tunkeutumistestaus/#laksyt

Cross-site scripting: https://portswigger.net/web-security/cross-site-scripting

Path traversal: https://portswigger.net/web-security/file-path-traversal

IDOR: https://portswigger.net/web-security/access-control/idor

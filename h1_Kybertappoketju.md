# Kybertappoketju

## x)

Valitsin jaksoksi **Erikoistilanteiden asiantuntija, vieraana Juhani Mäkinen | 0x45**, jossa erikoistilanteiden asiantuntija Juhani Mäkinen keskustelee juontajien kanssa muun muassa kriisivalmiudesta sekä kriisialueiden kokemuksistaan Afghanistanissa, Applen laitteiston tietoturvaa sekä haavoittuvuuksia.


Artikkelissa **Hutchins et al 2011: Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains** käydään läpi Kybertappoketjua, joka on Lockheed Martin:in kehittämä malli jossa hyökkääjien toiminta jaetaan 7 eri vaiheeseen. Mallin vaiheet ovat:

**Reconnaissance**: Kohdetta tutkitaan saadaakseen tietoa haavoittuvuuksista.

**Weaponization**: Haittaohjelma aseistetaan muotoon jossa se voidaan toimittaa uhrille.

**Delivery**: Haittaohjelma toimitetaan uhrille.

**Exploitation**: Haavoittuvuutta hyödynnetään jotta haittaohjelman koodia voidaan suorittaa.

**Installation**: Haittaohjelma ja mahdollinen takaovi (backdoor) hallinnan säilyttämistä varten asennetaan.

**Command & Control**: Hyökkääjä ottaa yhteyden altistuneelle koneelle.

**Actions on Objectives**: Hyökkääjän tavoitteen toteuttaminen, kuten tärkeiden tietojen varastaminen tai tuhoaminen.

Keskeinen ajatus artikkelissa on, että hyökkääjän täytyy onnistua aivan jokaisessa vaiheessa, jotta hyökkäys onnistuu. Hyökkääjän estäminen missä tahansa vaiheessa estää koko hyökkäyksen, ja tämä malli auttaa tietoturvatiimejä analysoimaan ja keskustelemaan uhista mallin avulla.


**€ Santos et al: The Art of Hacking (Video Collection): 4.3 Surveying Essential Tools for Active Reconnaissance.**- Videosarjassa käsitellään tiedustelua ja sen työkaluja. Aiheessa syvennytään muun muassa porttiskannaukseen. Sen avulla voidaan selvittää kohteen avoimet portit, joita voi esim. hyödyntää hyökkäyksissä.

Aktiivisessa skannauksessa on hyökkääjälläkin paljon huolehdittavaa. Siinä ollaan suorassa yhteydessä kohteeseen, joten skannauksesta jää aina jälki kohdejärjestelmän lokitietoihin.


KKO 2003:36 on Korkeimman oikeuden ennakkoratkaisu tapauksesta jossa oltiin tunkeuduttu tietojärjestelmään ja aiheutettu vahinkoa. Päätös on määritelty Suomen rikoslain mukaan, ja se kertoo, että pelkkä murto järjestelmiin täyttää rikoksen merkit, vaikkei vahinkoa tehtäisikään.

## a)

Latasin ensiksi ISO-tiedoston sijaan valmiiksi konfiguroidun virtuaalikonekuvan Kalista, säästyäkseni pitkältä asennusvaiheelta. Löysin virtuaalikonekuvan nettisivulta: **kali.org/get-kali/#kali-virtual-machines**.

<img width="530" height="122" alt="image" src="https://github.com/user-attachments/assets/fee1f40e-bc15-416b-b3a5-6a747755f23a" />


Kuvassa olin jo purrut paketin, ja seuraavaksi avasin Kali-tiedoston, se aukesi automaattisesti konfiguraatiovaiheeseen VirtualBoxissa. Säädin muistia 4GB asti kuten vinkeissä neuvottiin.

<img width="380" height="256" alt="image" src="https://github.com/user-attachments/assets/8d0128cd-2bdf-45ed-8c4b-5be536b47b67" />



## b)

Irrotin Kali-koneen verkosta. Tästä täytyy pitää huolta jotta mitään laitonta ei vahingossakaan tehdä.

VirtualBoxin Kali-koneen verkkoasetuksista kohdasta **Attached to**, valitaan vaihtoehto **Not attached**.

<img width="358" height="203" alt="image" src="https://github.com/user-attachments/assets/4d6787e0-5b5e-4980-992d-e409214dc788" />


Testasin verkkoyhteyttä Kalin terminaalissa komennolla **ping 8.8.8.8**, josta vastauksena tuli "100% packet loss". Tästä voin todeta, että verkkoyhteyden katkaiseminen onnistui.

<img width="263" height="93" alt="image" src="https://github.com/user-attachments/assets/2f8a6ce1-2b04-4935-9c92-d5e6fbe9fced" />


## c)

Porttiskannasin omaa konettani komennolla **sudo nmap -T4 -A localhost**.

Komennossa on paljon parametrejä, jotka on hyvä tietää. **sudo** suorittaa skannauksen root-oikeuksilla, ja **nmap** on työkalu, jolla skannaus suoritetaan. **-T4** on asetus, joka käyttää eri tasoja. Taso 4 tarkoittaa agressiivista skannausta, eli skannaus suorittuu nopeammin. **localhost** tarkoittaa omaa tietokonetta, eli skannaus suoritetaan itselle.

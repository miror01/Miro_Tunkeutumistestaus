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

<img width="328" height="164" alt="image" src="https://github.com/user-attachments/assets/0966e175-6c07-434a-9b90-23c19c7eca32" />


Komennossa on paljon parametrejä, jotka on hyvä tietää. **sudo** suorittaa skannauksen root-oikeuksilla, ja **nmap** on työkalu, jolla skannaus suoritetaan. **-T4** on asetus, joka käyttää eri tasoja. Taso 4 tarkoittaa agressiivista skannausta, eli skannaus suorittuu nopeammin. **localhost** tarkoittaa omaa tietokonetta, eli skannaus suoritetaan itselle.

Kuvassa näkyy, että kaikki 1000 skannattua porttia ovat kiinni. Portteja skannataan aina vain 1000, ellei toisin määritetä.


## d)

Seuraavaksi yhdistin hetkeksi koneen takaisin verkkoon ja asensin 2 demonia. Ajoin terminaalissa komennot: **sudo apt update**, sekä **sudo apt install -y apache2 openssh-server**. Päädyin apache2:een ja ssh-palvelimeen, yhdistin molemmat demonit samaan komentoon.

<img width="307" height="194" alt="image" src="https://github.com/user-attachments/assets/601a09c3-1192-47e8-991b-28b29a2d07a3" />

Palvelut päälle komennolla **sudo systemctl start apache2** ja **ssh**, ja uudelleenskannaus:

<img width="357" height="179" alt="image" src="https://github.com/user-attachments/assets/6515e236-9d21-4aef-b064-52bcfd5472fd" />

Kuvasta näkee, että portit 80, ja 22 ovat nyt auki. Portti 22 vastaa ssh-palvelinta ja Portti 80 on HTTP-portti Apachelle.

## e)

Valitsin tehtäväkseni Hack The Box:sta Starting Pointin "Fawn". tehtävän. Tehtävän suoritus alkoi OpenVPN-yhteyden muodostamisella.

<img width="220" height="290" alt="image" src="https://github.com/user-attachments/assets/5ff3373c-a616-4db8-89f2-f1cd6fedd808" />

Kuvan näkymästä latasin oman .ovpn-tiedostoni.

Terminaalissa siirryin ekana kansioon jonne ovpn-tiedosto latautui, komennolla **cd Downloads**, ja sieltä loin VPN-yhteyden komennolla **sudo openvpn starting_points_eu-starting-point-2-dhcp.ovpn**

<img width="848" height="245" alt="image" src="https://github.com/user-attachments/assets/11529eb6-ad9e-4d1b-9b31-ef7649a528bc" />

Tarkistin vielä VPN-yhteyden toimivuuden, vinkeissä mainitaan, että komento **ping 8.8.8.8** ei pitäisi toimia, jos VPN-yhteys on luotu onnistuneesti, joten testasin sitä.

<img width="263" height="89" alt="image" src="https://github.com/user-attachments/assets/417743a4-fafa-4950-80f1-3ebb4c7c4ba2" />

Kaikki näyttäisi toimivan. Hienoa.

HTB:stä käynnistetään Fawn-kone. Testataan yhteys koneeseen komennolla **ping**.

<img width="269" height="160" alt="image" src="https://github.com/user-attachments/assets/273a8d7f-fc39-4da2-b050-8a76b1e2c7f2" />

Yhteys toimii hyvin. Seuraavaksi haluan ottaa selvää, mitä palveluita kohdekoneessa pyörii. Tämä toteutetaan nmap-porttiskannerin avulla komennolla **nmap -sV <KOHDE>**. lippu **-sV** auttaa porteissa käynnissä olevien ohjelmistojen ja versioiden tunnistamisessa.

<img width="386" height="146" alt="image" src="https://github.com/user-attachments/assets/78421d1b-8158-4af3-bd4b-92cf77c1ed79" />

Portti 21 auki FTP-palvelua varten.

Seuraavaksi seurasin HTB:n tehtäviä ja otin FTP-yhteyden kohdekoneeseen.

<img width="184" height="86" alt="image" src="https://github.com/user-attachments/assets/76bb2a01-dc14-4635-bd81-b360468a9449" />

Ilman annettua käyttäjätunnusta oletus on aina **anonymous**, ja salasanaa ei ollut.

**ls**-komennolla löysin FTP-yhteyden kautta kohdekoneelta tiedoston flaf.txt, ja vedin tämän omalle koneelle komennolla **get flag.txt**. suljin yhteyden komennolla **exit**, ja viimeiseksi paljastin flagin omalta koneeltani komennolla **cat flag.txt**.

<img width="490" height="202" alt="image" src="https://github.com/user-attachments/assets/3f31d6f6-5b7f-4e90-84d4-9cbb0ea9e805" />

Ja täten Starting Point-tehtävä Fawn on valmis.












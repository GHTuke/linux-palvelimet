# H3-Hello Web Server

Tekijä: Tuukka Huovilainen

Tässä työkirjassa suoritetaan Linux palvelimet -kurssin tehtävää "Hello web server", jossa asennetaan Apachen webbipalvelin ja otetaan se käyttöön Linuxissa. Käyttöönoton jälkeen muokataan hieman etusivua ja käydään läpi muutamia logeja matkanvarrelta. Tehtävän pohjana on käytetty Tero Karvisen webbikirjoitusta https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/. Alkuun pieni kooste kyseisestä webbikirjoituksesta sekä Apache Software Foundationin kirjoituksesta https://httpd.apache.org/docs/2.4/vhosts/name-based.html.

## x - Tiivistelmä

Apache Software Foundation - Name Based Virtual Host Support

  * Nimipohjaisilla palvelimilla voidaan säästää rajallista IP-osoitteiden määrää ohjaamalla usealla nimellä olevia sivuja samoihin IP-osoitteisiin
  * Tietyt laitteet vaativat IP pohjaisia palvelimia, muuten pitäisi käyttää nimipohjaisia palvelimia

Tero Karvinen - Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address

  * Muokkaamalla sivun conf-tiedostoa voidaan palvelimelle asettaa asetuksia
  * $ sudo systemctl restart apache2 on hyödyllinen komento syöttää tehdessään muutoksia palvelimen asetuksiin

## Webbipalvelin
Localtime: 1.02.2025 12.10

Host specs:

    Windows 11 Home v. 24H2
    AMD Ryzen 7 6800H
    RAM 16 Gt
    VirtualBox v. 7.1.4

Virtual specs:

    Debian amd64 12(bookworm)
    VRAM 128MB
    RAM 4 Gt
    VHarddisk 40GB

Koska olin jo aiemmin vaihtanut Apachen default sivun palvelimelta, palautin alkuperäisen default sivun käyttöön tätä harjoitusta varten.\
Helpompi esittää tapahtuvat muutokset tätä kautta.

Alkuun komennot apachen asennuksesta index sivun luontiin asti, jonka jälkeen käydään tarkemmin läpi eri askeleet.

```
$ sudo apt-get update && sudo apt-get install apache2
$ sudoedit /etc/apache2/sites-available/hattu.example.com.conf
$ sudo a2ensite hattu.example.com
$ sudo a2dissite 000-default.conf
$ sudo systemctl restart apache2
$ cd
$ mkdir public_sites/hattu.example.com
$ cd public_sites/hattu.example.com
$ micro index.html
```

### a - Webbipalvelin
Localtime: 12.15

Ensin syötin terminaalista komennon:
```
$ sudo apt-get update && sudo apt-get install apache2
```
Komento päivittää asennettavien sovellusten listan ja jos tämä onnistuu, yrittää sen jälkeen asentaa Apache2 ohjelman. Tämän jälkeen selaimen kautta kirjoittamalla osoitekenttään "localhost" aukesi Apachen default sivu. Tämän auetessa normaalisti tietää Apachen asennuksen onnistuneen.
![apacheDefault](https://github.com/user-attachments/assets/02bc5452-a0b0-49be-9e0a-a727a49f4ef4)

### b - Logit

Komennolla:
```
$ sudo tail -5 /var/log/apache2/access.log
```
Sain näkyviin Apachen logeista viimeiset 5 riviä, tätä kautta pystyin tarkistamaan mitä tapahtui kun avasi selaimen kautta "localhost" sivun Apachen asennuksen jälkeen.\
![accessLogs](https://github.com/user-attachments/assets/d2dbe394-ef88-4153-840a-359ceb98876b)

Viimeiset 3 riviä näyttäisi liittyvän samaan avaukseen kellonaikojen perusteella. Kaikki 3 riviä ovat "GET" pyyntöjä palvelimelle. Kaikki pyynnöt on myös lähetetty osoitteesta 127.0.0.1 koneen omasta IP-osoitteesta.\
Ensimmäisellä avaukseen liittyvällä rivillä on GET pyyntö palvelimen juureen "/" käyttäen protokollaa "http/1.1", joka palautui koodilla 200 eli pyyntö onnistui, perässä oleva numero kertoo palautetun pyynnön suuruus bitteinä. Eli haki sivun. Mozilla osio kertoi vain millä selaimella pyyntö suoritettiin, tässä tapauksessa Firefox.\
Toisella rivillä GET pyyntö hakea juuresta osoitteen kautta kuva "/icons/openlogo-75.png HTTP/1.1" Ja taas tuli vastaus 200 ja tiedoston koko bitteinä.\
Kolmannella rivillä on taas GET pyyntö hakea ikoni osoitteesta "/favicon.ico" tällä kertaa vastaus on 404 eli ei löytynyt resurssia.

Login rivien selitykset löytyvät osoitteesta: https://httpd.apache.org/docs/2.4/logs.html.

### c - Etusivu
Localtime: 13.20

Lähdin luomaan uutta etusivua palvelimelle, Apachen default sivun tilalle.\
Ensin syötin komennon:
```
$ sudoedit /etc/apache2/sites-available/hattu.example.com.conf
```
Tämä avasi tyhjän conf-tiedoston pohjan, johon pystyi syöttämään palvelimen tietoja.\
![sudoeditSitesAvailable](https://github.com/user-attachments/assets/1fcd811b-7526-49d9-b48d-5d66652dad77)

```
VirtualHost *:80>
        ServerName hattu.example.com
        ServerAlias www.hattu.example.com
        DocumentRoot /home/tuke/public_sites/hattu.example.com

                <Directory /home/tuke/public_sites/hattu.example.com>
                Require all granted
                </Directory>
</VirtualHost>
```

Tiedostoon syötin <VirtualHost *:80> Tietueeseen tiedot palvelimen nimestä, sen aliaksesta sekä mistä sivun dokumentit löytyvät </VirtualHost>\
Tietueeseen <Directory /home/tuke/public_sites/hattu.example.com> Require all granted. Tämä tarkoittaa, että palvelin hyväksyy kaikki pyynnöt, tällä ei tässä vaiheessa niin väliä sillä kyseessä on lokaali palvelin, eli ei sinne pääse muita kuin lokaalikäyttäjä, mutta ilman kyseistä asetusta ei sinne pääsisi itsekkään. Komennolla "Require not *IP-osoite* voisi myös blokata tiettyjä osoitteita. Tästä lisää osoitteesta: https://httpd.apache.org/docs/2.4/howto/access.html.

Tämän jälkeen piti asettaa luotu palvelinosoite käyttösivuksi. Syöttämällä komennot:
```
$ sudo a2ensite hattu.example.com
$ sudo a2dissite 000-default.conf
$ sudo systemctl restart apache2
```
Apache enabloi sivun hattu.example.com ja disenabloi sivun 000-default.conf, joka oli aiemmin Apachen oletussivu osoittelle "localhost".\
Tässä vaiheessa viimeistään oli tärkeää käynnistää apache uudestaan restart komennolla, jotta palvelimelle tehdyt muutokset päivittyvät.

Tämän jälkeen siirryin kotihakemistoon komennolla:
```
$ cd
```
Loin uuden kansion, jonka olin määrittänyt palvelimen poluksi aiemmin conf-tiedostoon.
```
$ mkdir public_sites/hattu.example.com
```
Siirryin uuteen kansioon ja loin sinne uuden tiedoston index.html micro-ohjelmalla ja kirjasin sinne uuden otsikon "Hattu sivu:
```
$ cd public_sites/hattu.example.com
$ micro index.html
```
Tässä sitten toimiva etusivu testattuna:
![hattuSivu](https://github.com/user-attachments/assets/199ab97b-c1b4-461e-acf7-f262675f108f)


### e - Validi HTML5
Localtime 14.00

Muokkaamaan etusivua pääsee mistä vain komennolla:
```
$ micro /home/tuke/public_sites/hattu.example.com/index.html
```
Yksinkertaisuuden nimissä, en tässä vaiheessa muokannut alkuperäistä testisivua muuten kuin asetin validoinnin vaatimat tai suosittelemat asetukset sivulle.\
Tässä tapauksessa niitä olivat:
```
!DOCTYPE
<html lang=en> //Kieli tässä vaiheessa valinnainen, mutta suositeltava
<title>

```
![microHattusivu](https://github.com/user-attachments/assets/29ea327c-ca47-4bc0-a58f-2af3ee345cf7)

Tässä vielä sivun koko koodi tekstimuodossa:
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hattu sivu</title>
</head>
<body>
    <h1>Hattu sivu</h1>
    <div>
    	<p>Tämä sivu on luotu esimerkkinä Linux palvelimet kurssia varten</p>
    </div>
</body>
</html>

```

Validointi testattu sivulla https://validator.w3.org/.

### f - curl

Komennoilla:
```
$ curl localhost
$ curl -I localhost   // HUOM! iso i ei pieni L
```
![curlHattu](https://github.com/user-attachments/assets/e5783dc3-5eb7-4c7f-a802-e5a6a09046e2)

Perus curl komento näyttää siis sivun raakadatan terminaalissa tekstimuodossa.\
curl -I näyttää avatusta sivusta sen metatietoja. Tästä tärkeimpiä poimintoja:
* HTTP/1.1 200 OK - Mikä protokolla ja vastauskoodi (200 OK)
* Date: Sat, 01 Feb 2025 12:22:22 GMT - Päivämäärä ja kellonaika perus GMT muodossa
* Server: Apache/2.4.62 (Debian) - Palvelimen käyttöjärjestelmä
* Last-Modified: Sat, 01 Feb 2025 12:05:58 GMT - Koska viimeksi muokattu
* Content-Type: text/html - Minkä tyyppistä tietoa on sivulla tässä tapauksessa tekstiä ja html notaatioita.

### o - Kaksi eri sivua


## Lähteet

Karvinen, T. 10-04-2018. Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address. https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/.

The Apache Software Foundation. s.a. Name-based Virtual Host Support. https://httpd.apache.org/docs/2.4/vhosts/name-based.html.

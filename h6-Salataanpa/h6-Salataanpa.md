# H6 - Salataanpa
Tekijä: Tuukka Huovilainen

* Pohjana Tero Karvinen 2025: Linux Palvelimet kurssi, http://terokarvinen.com

Tässä työkirjassa suoritetaan Linux Palvelimet -kurssin tehtävää Salataanpa, jossa asetetaan TLS-sertifikaatti aiemmissa työkirjoissa työstetylle palvelimelle (huovilainen.com).
Alkuun lyhyet tiivistelmät kolmesta artikkelista.

## x - Tiivistelmä

### Let's Encrypt - How it works
https://letsencrypt.org/how-it-works/\

* Certificate management agent (CMA), joka on asennettu palvelimelle varmistaa Let's encryptille, että juuri tämä palvelin hallinnoi domainia.
* Tunnistuksen jälkeen CMA pystyy pyytämään sertifikaattia, sekä uusimaan tai poistamaan niitä.
* Toimii julkisen ja yksityisen avaimen kautta joita CMA käyttää varmistamaan palvelimen oikeellisuuden.

### Lego - Obtain a Certificate
https://go-acme.github.io/lego/usage/cli/obtain-a-certificate/index.html#using-an-existing-running-web-server\

* Jos on käynnissä oleva julkinen http palvelin portissa 80 Lego kirjoittaa Acme challenge tokenin
* Jos julkisen http palvelimen kansio on saatavilla domainin juuresta `/`, voidaan varmistus suorittaa tätä kautta
* Pystyy myös luomaan Acme challenge tokenin ajamalla komentosarjan (tätä käytettiin myös myöhemmässä tehtävässä)
```
lego --accept-tos --email you@example.com --http --http.webroot /path/to/webroot --domains example.com run
```

### Apache Documentation - SSL/TLS Strong Encryption: How-To
https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html#configexample\

* Palvelimelle tarvitsee verkkosivun conf-tiedostoon lisätä seuraavanlainen koodi
```
LoadModule ssl_module modules/mod_ssl.so

Listen 443
<VirtualHost *:443>
    ServerName www.example.com
    SSLEngine on
    SSLCertificateFile "/path/to/www.example.com.cert"
    SSLCertificateKeyFile "/path/to/www.example.com.key"
</VirtualHost>
```
* Tässä tapauksessa SSL moduliian ei ole erikseen apachesta asetettu käyntiin vaan se ladataan "LoadModule" komennolla conf-tiedostossa

## a - Let's
Localtime: 26.02.2025\
Start: 11.00\
Finish: 11.26

Aloitin TLS sertifikaatin järjestämisen huovilainen.com palvelimelle tarkistamalla, että nykyisellään sellaista ei ole vielä asetettu. Ensin varmistin apachen olevan ajantasalla tiedostojen kanssa ajamalla komennon `sudo systemctl restart apache2`. Sitten avasin verkkoselaimesta osoitteen huovilainen.com.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h6-Salataanpa/httpAloitusOikea.png width=600>

Kuten kuvasta näkyy, ei sivulla ole vielä sertifikaattia. (Lukko on ruksittu yli osoitekentän vieressä) Testasin myös avata https://huovilainen.com osoitteen, mutta koska palomuurin portti oli kiinni jäi sivu loputtomaksi aikaa lataamaan.

Siirryin siis hakemaan sertifikaattia ja alkuun asensin legon komennolla `sudo apt-get install lego`. Lego toimii Certificate Managemnt Agentina joka varmistaa Let's Encryptille eli sertifikaatin tarjoajalle, että domain kuuluu tälle palvelimelle. Loin myös tulevia lego tiedostoja varten kansion `mkdir lego` omaan kotikansiooni.

Tämän jälkeen kokeilin Let's encryptin Staging Serverin kautta, että syntaksi jonka kirjoitan toimii eikä sertifikaatin asettamisessa ole muitakaan ongelmia.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h6-Salataanpa/legoKomento.png width=600>

Alla myös koodina kyseinen kuva.
```
lego
--server=https://acme-staging-v02.api.letsencrypt.org/directory #Tämä on harjoitusserver sertifikaattihakua varten. Tämä rivi pois oikeassa haussa.
--accept-tos --email="Esimerkki@sahkoposti.fi"
--domains="huovilainen.com" --domains="www.huovilainen.com"
--http --http.webroot="/home/tuke/public_sites/huovilainen.com"
--path="/home/tuke/lego"
--pem run
```
Ei tullut harjoitushaun kanssa ongelmia ja lego oli luonut aiemmin luomaani lego kansioon tarvittavat tiedostot sertifikaattivarmennusta varten.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h6-Salataanpa/legoStagingKansiot.png width=600>

Koska kaikki toimi, muutin lego kansion nimen komennolla `mv -n lego DISlego` ja loin uuden lego kansion oikeita kansioita ja tiedostoja varten. `mkdir lego`.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h6-Salataanpa/legoDis.png width=600>

Ja tässä vaiheessa poistin aiemmasta legon harjoitusajokoodista osan `--server=https://acme-staging-v02.api.letsencrypt.org/directory` ja ajoin sen uudestaan. Nyt avaimet ja sertifikaattivarmenteet oli luotu virallisesti. Siirryin siis muokkaamaan verkkosivuni conf-tiedostoa.
```
sudoedit /etc/apache2/sites-available/huovilainen.com.conf

# Lisäsin sinne uuden virtualhost dependencyn
<VirtualHost *:443>
        ServerName huovilainen.com
        ServerAlias www.huovilainen.com
        DocumentRoot /home/tuke/public_sites/huovilainen.com
	      SSLEngine On
	      SSLCertificateFile "/home/tuke/lego/certificates/huovilainen.com.crt"
	      SSLCertificateKeyFile "/home/tuke/lego/certificates/huovilainen.com.key"
        <Directory /home/tuke/public_sites/huovilainen.com>
                Require all granted
        </Directory>
</VirtualHost>
```

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h6-Salataanpa/confMuokattu.png width=600>

Tässä vaiheessa koitin käynnistää apachen uudestaan komennolla `sudo systemctl restart apache2`, mutta tuli virhettä. Ajoin perään apachen configtestin `sudo apche2ctl configtest`, sekään ei toiminut, mutta antoi ilmoituksen "perhaps misspelled or defined by a module not included in the server configuration".

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h6-Salataanpa/apacheConfigtestFailed.png width=600>

Tässä vaiheessa muistin, että en ollut ottanut apachen ssl moduulia käyttöön.
```
sudo a2enmod ssl
sudo apache2ctl configtest
sudo systemctl restart apache2
```
Nyt apache lähti toimimaan ja configtest antoi ilmoituksen "Syntax OK".

Piti siis avata vielä palomuuriin portti https varten, eli kuten aikaisemmin conf tiedostossa oli määritelty Portti 443.
```
sudo ufw allow 443/tcp
sudo ufw status
```

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h6-Salataanpa/ufwAllow.png width=600>

Nyt ilmestyi sivun osoitekenttään jo ihan oikea lukon kuva ja https://huovilainen.com aukesi.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h6-Salataanpa/httpsToimii.png width=600>

## b - A-rating
Start: \
Finish:



<img src= width=600>

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




## Lähteet

Apache. SSL/TLS Strong Encryption: How-To. https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html#configexample.

Lego. Obtain a Certificate. https://go-acme.github.io/lego/usage/cli/obtain-a-certificate/index.html#using-an-existing-running-web-server.

Let's Encrypt. How it works. https://letsencrypt.org/how-it-works/.

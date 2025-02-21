# H5 - Nimekäs
Tekijä: Tuukka Huovilainen

* Pohjana Tero Karvinen 2025: Linux Palvelimet kurssi, http://terokarvinen.com

Tässä työkirjassa suoritetaan Linux Palvelimet -kurssin tehtävää Nimekäs, jossa vuokrataan nimipalvelusta domain ja asetetaan se osoittamaan aikasemmin tehtävässä h4 järjestettyyn palvelimeen. Aikaisempia tehtäviä h3 ja h4 mukaillen myös muokataan webpalvelimen näyttämää sivua ja tehdään siitä muutaman eri sivun kokonaisuus. Nimipalveluna käytetään NameCheapia.

## a - Nimi
Localtime: 21.02.2025\
Start: 11.00\
Finish: 11.10

Alkuun avasin Namecheapin verkkosivut osoitteesta https://www.namecheap.com/. Heti etusivulla oli kohta johon pystyi kirjoitaamaan haluamansa domain nimen hakuun.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/NamecheapDomainsearch.png width=600>

Valittuani haluamani nimen ja lisättyäni sen ostoskoriin siirryin maksamaan. Maksun jälkeen käyttäjätiedoistani sai auki kohdan Domain list.\
Täällä näkyi äsken ostamani domain.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/NamechepDomainlist.png width=600>

Oikeassa reunassa olevasta "Manage" napista pääsi kiinni asetuksiin. Sieltä valitsin "Advanced DNS", jotta pääsin muokkaamaan sivun linkityksiä.\
Alkuun sivu näytti tältä.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/NamecheapAdvanced.png width=600>

Poistin olemassa olevat Recordit ja lisäsin kaksi uutta "a-record":ia. Host kohtaan valitsin @ ja wwww. Value on upCloud palvelusta aiemmassa tehtävässä ostetun palvelimen IP-osoite (ainakin identtisen, jouduin tekemään uuden palvelimen tehtävien välissä).

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/NameCheapAdvancedChanged.png width=600>

Pienen odottelun jälkeen jo sitten näkyikin palvelimelleni tehty porkkanasivu.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/PorkkanaToimii.png width=600>

## b - Based
Start: 12.15\
Finish: 12.17

Lähdin luomaan name based virtual hostia tuttuun tapaan luomalla uuden conf teksti tiedoston apachen sites available listaan.
```
$ sudoedit /etc/apache2/sites-available/huovilainen.com

#Avautuvaan tekstieditoriin#
<VirtualHost *:80>
        ServerName huovilainen.com
        ServerAlias huovilainen.com
        DocumentRoot /home/tuke/public_sites/huovilainen.com

                <Directory /home/tuke/public_sites/huovilainen.com>
                Require all granted
                </Directory>
</VirtualHost>
```
<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/Sudoedit.png width=600>

Tämän jälkeen loin kansion, jonne conf tiedosto osoittaa ja loin sinne index.html tiedoston.

```
$ mkdir -p /home/tuke/public_sites/huovilainen.com
$ cd /home/tuke/public_sites/huovilainen.com
$ micro index.html
```
Sitten asetin sivun apachen käyttöön ja poistin vanhan porkkana sivun käytöstä. Loppuun vielä testastin curlilla nopeasti mitä sivu palauttaa.

```
$ sudo a2ensite huovilainen.com.conf
$ sudo a2dissite porkkana.example.com.conf
$ sudo systemctl restart apache2
$ curl localhost
```
Tässä vaiheessa sivu toimi aivan normaalisti sen domain nimen kautta mistä tahansa laitteesta.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/microToimivaTesti.png width=800>

## c - Kotisivu
Start: 12.50\
Finish: 13.13

Aloitin tehtävän kirjoittamalla kolmeen eri html tiedostoon verkkosivujen koodin. Koodi on hyvin yksinkertaista aikapaineiden takia ja tulen sitä tulevaisuudessa muokkaamaan lisää, mutta alustavasti sivujen oli vain tarkoitus toimia.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/SivujenKoodit.png width=800>

Koodien kirjoittamisen jälkeen lähdin siirtämään niitä virtuaalipalvelimelle käyttäen scp komentoa (https://linuxblog.io/linux-securely-copy-files-using-scp/).\
Tein aluksi vain yhden tiedoston siirron testinä, jonka jälkeen siirsin loput kaksi erillisinä tiedostoina. Siirron olisi voinut tehdä myös yhdellä komennolla, mutta tällä kertaa hoidin asian tällä tavalla.
```
$ scp /home/tuke/esimerkkisivuja/index.html tuke@94.237.113.82:/home/tuke/public_sites/huovilainen.com
```
Syntaksi avattuna\
`scp` secure copy protocol, on vain toiminnon nimi, jolla sitä kutsutaan\
`/path/index.html` on koko polku tiedostoon, pystyisi kopioimaan myös kansion, mutta testasin vasta toimivuutta\
`tuke@94.237.113.82` käyttäjänimi ja palvelin, jonne siirto suoritetaan\
`:/path/` polku minne kansioon tiedostot kopioidaan

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/scpKopiointi.png width=600>

Kun kaikki tiedostot oli kopioitu, pystyi ne avamaan suoraan huovilainen.com osoitteesta.
<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/Main.png width=600>
<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/About.png width=600>
<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/Projects.png width=600>

## d - Alidomain
Start: 13.45\
Finish: 13.47

Subdomainien luontiin ohjeet https://www.namecheap.com/support/knowledgebase/article.aspx/9776/2237/how-to-create-a-subdomain-for-my-domain/.\
Uusien subdomainien luonnin aloitin siirtymällä Namecheapiin.\
Accout ->\
Domain list ->\
Manage ->\
Advanced DNS

Lisäsin Host records kohtaan kaksi uutta A recordia, toiseen asetin host:iksi tuukka ja toiseen blog.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/NameCheapSubdomains.png width=600>

Nyt aikaisemmin luomat sivuni aukeavat myös osoitteista: tuukka.huovilainen.com sekä blog.huovilainen.com.

## e - Host ja Dig

<img src= width=800>


## Lähteet

LinuxBlog. SCP Linux – Securely Copy Files Using SCP examples. https://linuxblog.io/linux-securely-copy-files-using-scp/.

NameCheap. How to Create a Subdomain for my Domain. https://www.namecheap.com/support/knowledgebase/article.aspx/9776/2237/how-to-create-a-subdomain-for-my-domain/.


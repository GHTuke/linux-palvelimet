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

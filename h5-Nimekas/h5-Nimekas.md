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

Alkuun ei omalta linux koneelta löytynyt `host` tai `dig` toimintoja. Tämän pystyi tarkistamaan helposti esim. `host -V`, joka listaisi version paketista.\
Sivulta https://superuser.com/questions/141623/installing-dig-on-debian löytyi ohjeet dnsutils paketin asentamiseen, jonka sisällä molemmat tulevat.\
```
$ sudo apt-cache search dnsutils
$ sudo apt-get install dnsutils
$ host -V
```
### Jatkoin tehtävän tekoa parin päivän päästä aikaongelmien takia
Localtime: 23.02.2025\
Start: 19.08\
Finish: 19.20

Alkuun tarkistin host ja dig komentojen toimintoja `man host|less` ja `man dig|less`. Molemmilta löytyi hyvät kuvaukset mitä komennot tekevät ja kattavat listaukset komentojen parametreistä. Ongelmaksi nousi pian ANY parametrin lisääminen. Jotkin domainien tarjoajat ovat estäneet sen toiminnan ja tulee virheilmoitusta tai vaan osittainen vastaus. Ensin siis oli oman sivun kanssa ongelmia, myöhempien sivujen kanssa näkyi sitten paremmin.\
Tutkitut sivut olivat:
* huovilainen.com
* terokarvinen.com
* google.com

### huovilainen.com

Ensin suoritin host komennon `host huovilainen.com` vastaus oli odotettu, eli näytti nimetyn osoitteen IPv4 osoitteen ja listasi mailia varten forward serverit jotka niitä hallinnoivat. Tässä tapauksessa ei ollut edes asetettu NameCheapin kautta maili asetuksia, joten oletan näiden olevan automaattisia lisäyksiä, jotka eivät ns. tee mitään ennen kuin niihin kiinnittää oman mailitoiminnan.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/HostOma.png width=800>

Mailin forward tiedot myös osuvat yhteen Namecheapin Domain managerin kautta näkyvään email forwarding asetukseen.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/MailOma.png width=800>

Tämän jälkeen suoritin `dig huovilainen.com` komennon, ja käydään vähän tarkemmin läpi mitä mikäkin kohta tarkoittaa.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/DigOma.png width=800>

Ensimmäinen rivi kertoo miltä järjestelmä versiolta komento tuli. tässä tapauksessa DiG versio 9.18.33 ja debi12 eli Debian 12.

Sen jälkeen tulee HEADER osio. Tästä tärkeimpiä:\
`status: NOERROR` ei ongelmia pyynnössä.\
`Query: 1, ANSWER: 1` kuinka monta pyyntöä ja kuinka monta vastausta komennolla suoritettiin ja saatiin.\
`QUESTION SECTION:`\
`huovilainen.com   IN    A` kertoo mihin pyyntö tehtiin. IN kertoo internetin välityksellä ja A kertoo, että haettiin A record eli address record (dig defaulttina suorittaa A record haun ilman parametrejä).\
`ANSWER SECTION:`
`huovilainen.com    99   IN    A    94.237.113.82` Samat tiedot kun äsken, mutta 99 on ns. Time to Live eli kaunko kestää kunnes record päivitetään. Lopussa tietenkin IP osoite, joka saatiin vastaukseksi.

Tämän jälkeen tulee vain lisätietoa hausta eli kauan kesti `Query time` ja koska haku suoritettiin `WHEN` ja vastuksen koko `MSG SIZE rcvd`.

Sitten suoritin ´dig huovilainen.com ANY´ haun, jonka pitäisi näyttää kaikki mahdolliset hakukentät ei vain A record hakua.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/DigAnyOma.png width=800>

Tässä vastauksessa näkyy `HINFO "RFC8482"`, joka on vastaus kun ANY haku on blokattu palveluntarjoajan puolelta. Tässä tuli sitten vastaan se, että välillä ANY haku antoi kuitenkin jonkun verran tietoa, mutta ei koskaan suorittanut täyttä ANY hakua.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/DigOmaANYOsittainen.png width=800>

Tässä kuvassa näkyy, että haku toi vastauksena nimipalvelun tiedot eli `NS   dns2.registrar.servers.com`, sekä myös maili tietoja jotka myös host näytti aiemmin. Mutta esimerkiksi A recordia ei näkynyt haussa vaikka sen pitäisi.

### google.com

Parempi onni hakujen kanssa oli google.com:in kanssa hieman yllättäenkin. Googlella tietenkin on oma nimipalvelimensa, joten se näkyy monessa vastauksessa.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/HostGoogle.png width=800>

Perus `host google.com` haussa näkyy vain google.comin IPv4, IPv6 soitteet sekä google.comin mail record.\
Laajemassa `host -a google.com` haussa näkyikin sitten jo google.comin oma nimipalvelin ja sen serverit joilta IP haettiin `ns2.google.com`.

Komento `dig google.com ANY` näytti oikeastaan kaikki samat tiedot kuin aikasempikin laajennettu host komento.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/DigGoogleANY.png width=800>

### terokarvinen.com

Sitten tutkin Tero Karvisen omien sivujen tietoja.\
Perus `host terokarvinen.com` palautti tiedon osoitteen IPv4 osoitteesta sekä sen maili hallinnoinnin serveristä.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/HostKarvinen.png width=800>

Myös terokarvinen.com sivun kanssa oli `dig terokarvinen.com ANY` haun kanssa ongelmia ja palautti aina välillä vain osittaisia tietoja, muutaman haun jälkeen vastaus oli tämän näköinen.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/DigKarvinenANY.png width=800>

`SOA ns1.gandi.net` eli SOA recordin kautta näkyy kyllä domain palveluntarjoaja gandi, vaikka ANY haku ei hakenutkaan NS tietuetta niin kuin normaalisti.\
Pienenä hauskana lisänä tähän se, että `dig SOA terokarvinen.com` ei palauta SOA recordia ollenkaan, aivan kuin sitä ei olisi ja ANY haku kuitenkin sen näyttää. Sain kuitenkin sen näkyviin uudestaan ANY haulla ja hieman laajennettuna komennolla `dig ANY +multiline terokarvinen.com`.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h5-Nimekas/DigKarvinenMulti.png width=800>

Nyt näkyi myös lisätiedot siitä koska nimipalvelimen tiedot päivittyvät jne (Refresh, retry, expire, minimum).

## Lähteet

LinuxBlog. SCP Linux – Securely Copy Files Using SCP examples. https://linuxblog.io/linux-securely-copy-files-using-scp/.

NameCheap. How to Create a Subdomain for my Domain. https://www.namecheap.com/support/knowledgebase/article.aspx/9776/2237/how-to-create-a-subdomain-for-my-domain/.

Zivanov, S. phoenixNAP. dig Command in Linux with Examples. https://phoenixnap.com/kb/linux-dig-command-examples.

SuperUser. Installing dig on Debian. https://superuser.com/questions/141623/installing-dig-on-debian.


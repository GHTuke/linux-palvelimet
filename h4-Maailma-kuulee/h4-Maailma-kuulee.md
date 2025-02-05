# H4 - Maailma kuulee
Tekijä: Tuukka Huovilainen

* Pohjana Tero Karvinen 2025: Linux Palvelimet kurssi, http://terokarvinen.com

Tässä työkirjassa suoritetaan Linux Palvelimet -kurssin tehtävää Maailma kuulee, jossa vuokrataan virtuaalipalvelin ja asennetaan sille Apache webpalvelin.
Aikasempaa tehtävää h3 mukaillen myös muokataan webpalvelimen näyttämää sivua. Virtuaalipalvelimen tarjoana käytetään upCloud tarjoajaa. Viimeisessä osiossa myös korjataan hieman ongelmaa, joka syntyi Virtual Name Based Hostingin yhteydessä.

Alkuun koostetaan tiivistelmät webkirjoituksista Susanna Lehto Teoriasta käytäntöön pilvipalvelimen avulla(h4) (https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/) sekä Tero Karvinen First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS (https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/).

## Tiivistelmä

Susanna Lehto
* DigitalOceanin virtuaalipalvelimesta pystyi laittamaan päälle maksumuistutuksen, jolla muistutetaan maksun lisäämisestä tai lopettamisesta
* DigitalOceanin ympäristössä virtuaalipalvelimet kulkevat termillä "Droplet"
* Suositeltavaa valita datakeskus, jonne palvelimen asettaa läheltä itseään tai havittelemiaan markkinoita, mutta niin että lakiasiat pysyisivät kohtalaisen helppoina
* Vaihtoehtoina autentikoinnissa salasana tai SSH-avain
* $sudo ufw allow 22/tcp # tekee palomuuriin reiän virtuaalipalvelimen käsittelyä varten
* $sudo apt-get dist-upgrade päivittää käyttöjärjestelmän

Tero Karvinen
* Jos joudut käyttämään salasanaa kirjautumiseen, muista käyttää hyvää ja turvallista salasanaa
* Käyttäjät pitäisi aina olla henkilökohtaisia, eikä yleiskäyttäjiä kuten admin
* Käyttäjät voi sitten ryhmittää lisäämällä niitä eri ryhmiin $sudo adduser *käyttäjänimi* *ryhmänimi*
* Muista lukita root käyttäjä, kun on luotu toinen käyttäjä, jolla on on sudo oikeudet ja ne on todettu toimiviksi
* Webpalvelinta varten palomuuriin reikä $sudo ufw allow 80/tcp

## a - Virtuaalipalvelimet vuokraus
Localtime: 05.02.2025\
Start: 12.10\
Finish: 12.30

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

Alkuun avasin upCloud palveluntarjoajan sivut osoitteesta: https://upcloud.com/. Etusivulta löytyikin ilmaisen trial version mainos, painoin tästä kokeilemaan.\
Aukesi sivu, josta pystyi aloittamaan käyttäjän luomisen ilmaista trial versiota varten.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/upCloudTrial.png width=600>

Tässä vaiheessa siirryin eteenpäin ja piti syöttää käyttäjätunnus, salasana, puhelinnumero sekä maa. Kaikki näistä olivat pakollisia.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/upCloudFinalize.png width=600>

Tämän jälkeen pääsin käyttäjällä sisälle, tuli vahvistusviesti sähköpostiin ja sen avattuani pystyin varmentamaan käyttäjäni painamalla omalta etusivultani "Verify account".

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/upCloudVerify.png width=600>

Varmentamista varten piti syöttää pankkikortin tiedot ja kun ne oli syötetty siirryin takaisin etusivulle, josta aloitin uuden serverin deploy prosessin valitsemalla "Deploy now".\
Vaihtoehtoisesti voi vasemmanpuoleisesta valikosta valita "Servers" -> "Server list" ja avautuvalta sivulta "Deploy now".

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/upCloudServerDeploy.png width=600>

Aukesi serverin luomista varten valikko. Ensimmäisenä piti valita lokaatio minne haluaa serverin sijoittaa. Itse valitsin FI-HEL1, lähinnä sen vuoksi, että kyseessä on vain harjoittelupalvelin. Parempi, että se sijaitsee lähellä latenssin vuoksi.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/upCloudArea.png width=600>

Sitten piti valita Plan, jolla edetään. Eri tasoilla eri hinnoittelu ja tasojen sisällä eri RAM ja kovalevy kokoja tarjolla. Itse valitsin halvimman 1CPU, 1GB RAM ja 10GB kovalevy, koska siinä kaikki tähän harjoitukseen tarvittava.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/upCloudPlan.png width=600>

Seuraavaksi olisi voinut vaikuttaa virtuaalikovalevyihin, luoda uusia jakoja tai valita salausta. Menin tässä vaiheessa perusasetuksilla, kun ei muuta tarvita.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/upCloudStorage.png width=600>

Sitten tuli ensimmäiset pelkät lisämyynnit, eli varmuuskopiointi. En ottanut tässä vaiheessa, kun ei sitä harjoituspalvelimelle tarvita ja muitakin ratkaisuja tarvittaessa löytyy.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/upCloudBackup.png width=600>

Sitten OS(Operating system) valinta, kohtalaisen helppo kun oma virtuaalikone, jolta tätä tulen käyttämään ja jota eniten käyttänyt on Debian 12. Valitsin siis saman kun oli valmiina tarjolla. Myös custom imagea olisi voinut käyttää luomiseen, mutta koska oma image olisi ollut myös Debian 12, mutta pienellä säädöllä jo niin puhdaspöytä oli parempi vaihtoehto.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/upCloudOS.png width=600>

Seuraavaksi verkkovalintoihin, automaattisesti oli valittuina ipv4 ja ipv6 joista ipv4 osoitetta ainakin tarvitaan harjoituksessa, mutta myös ipv6 on hyvä olla. Utility network oli myös suoraan valittuna, jos olisin asettanut virtuaalipalvelimen useammalle lokaatiolle, tämä helpottaisi ja nopeuttaisi niiden välistä kommunikaatiota (https://upcloud.com/docs/products/networking/features/utility-network/). Tässä tapauksessa se on turha, mutta toisaalta ei aiheuta ongelmia joten jätin sen päälle kun oletusasetuksena oli.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/upCloudNetwork.png width=600>

Sitten tuli lisävaihtoehtoja mm. metadata service jonka kautta pystyisi init templateilla luomaan suoraan asetuksia virtuaalipalvelimelle, jätin oletusasetukset päälle.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/upCloudOptionals.png width=600>

Tässä vaiheessa tuli autentikointi, vaihtoehtoina oli omien valintojen takia auki ainoastaan SSH avaimella kirjautuminen. Kävin siis omalta virtuaalikoneelta hakemassa julkisen avaimen.
```
$sudo apt-get update # aina päivitys ohjelmalistaan ennen kuin hakee uusia ohjelmia
$sudo apt-get install openssh-client # asentaa openssh käyttäjäversion, ei ylimääräisiä käyttötapoja ohjelmalle kun niitä ei tarvita
$ssh-keygen # tämä luo avaimen, kysyy sen jälkeen minne avain luodaan. Default on hyvä ja käytin sitä itse. Sen jälkeen turvakysymyksien luontia, voi jättää tyhjäksi eli Enter Enter Enter
$cd /home/tuke # hyppäsin omaan kotivalikkoon
$cd .ssh # avasi kansion, jossa avaimet ovat
$micro id_rsa.pub # avasin micro tekstieditorissa JULKISEN avaimen eli .pub päätteisen, josta kopioin avaimen upcloud SSH salausta varten. HUOM! ei ikinä privaattiavainta muille.
```

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/sshPublic.png width=850>

Avaimen syötettyä upCloudin autentikointi kohta näytti tältä.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/upCloudSsh.png width=600>

Enää jäljellä oli initialization scriptit ja server configuration. Scriptejä ei tarvinnut tässä vaiheessa, joten jätin tyhjäksi.\
Server Configuroinnista vaihdoin Hostname kohtaan "porkkana", jos vaikka joskus tämä tulisi muualla näkyviin niin ei likaa tietoa koneesta.\
Sitten painoin Deploy. Oikeassa reunassa näkyi vielä kooste virtuaalipalvelimen tiedoista.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/upCloudDeploy.png width=600>

Tämän jälkeen siirryin automaattisesti Server list välilehdellä, jossa alkuun näkyi teksti "Deploying", hetken päästä palvelimen kohdalle tuli vihreä pallo ja se oli valmiina käyttöön.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/upCloudValmis.png width=600>

## b - Alkutoimet virtuaalipalvelimella
Start: 13.15\
Finish: 13.39

Ensin yhdistin uuteen virtuaalipalvelimeen(tästä eteenpäin porkkanapalvelin) olemassa olevalta virtuaalikoneeltani(DebianTuke) komennolla:
```
$ ssh root@94.237.118.113
```
Piti ensin hyväksyä kirjautuminen, kirjoittamalla "yes". Palvelin automaattisesti luki privaattiavaimeni virtuaalikoneeltani ja vertasi sitä aikaisemmin antaamani julkiseen avaimeen (tämä kohta ei oikein näy kun se tapahtuu).\
Päästyäni sisälle root käyttäjänä lähdin asentamaan palomuuria ja asettamaan kulkua porkkanapalvelimelle. Tässä käyttämäni komennot:
```
$ sudo apt-get update            # Aina hyvä päivittää ohjelmalista ensin
$ sudo apt-get install ufw       # Asentaa ufw palomuurin
$ sudo ufw status                # tarkistaa palomuurin statuksen, tässä vaiheessa luki "inactive"
$ sudo ufw allow 22/tcp          # sallii portin 22 käytön, tätä kautta porkkanapalvelimelle yhdistetään. Lukee "Rules updated" kun suoritettu
$ sudo ufw enable                # Käynnistää palomuurin, varmistaa vielä että tämä voi vaikuttaa porkkanapalvelimelle kirjautumiseen, mutta avasimme äsken portin 22, joten "y"
$ sudo ufw status                # Näyttää taas palomuurin statuksen, nyt "Active" ja listassa näkyy "22/tcp ALLOW"
```

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/sshFirewall.png width=850>

Tämän jälkeen lähdin luomaan uutta käyttäjää, jotta myöhemmin voidaan sulkea root käyttäjä. Käytän tässä vaiheessa "sudo" komentojen alussa, vaikka periaatteessa root käyttäjän tätä ei tarvitsisi, mutta näin jää eri logeihin merkintä siitä, että komennot on suoritettu. Komennot:
```
$ sudo adduser tuke          # luo käyttäjän tuke, pyytää lisätietoja kuten salasana ja koko nimi. Näihin annoin kunnon tiedot, muut pyynnöt voi naputella Enter:illä läpi ja lopuksi "y + Enter"
$ sudo adduser tuke sudo     # lisää käyttäjän tuke ryhmään sudo

```

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/sshAdduser.png width=850>

Tässä vaiheessa yritin kirjautua uudelle käyttäjälle toisesta terminaalista.
```
ssh tuke@94.237.118.113
```

Tuli viesti "Permission denied (publickey)". Ei siis ollut pääsyä porkkanapalvelimelle uudella käyttäjällä, koska uudella käyttäjällä ei ollut tiedossa julkista avainta johon verrata privaattiavainta. (Tässä vaiheessahan ne ovat samat kuin root käyttäjällä, mutta porkkanapalvelin ei voi sitä tietää)

Joten lähdin kopiomaan porkkanapalvelimen sisällä root käyttäjänä ssh avaimen tunnistinta tuke käyttäjälle. Tämä tapahtui komennolla:
```
$ sudo cp -rvn /root/.ssh/ /home/tuke/
Kommentteja:
# cp on kansion kopiontiin komento
# -rvn on kolme eri komentoa
# -r eli rekursiivinen kopio kaiken kyseisen kansion sisältä
# -v eli verbose näyttää kopionnin jälkeen mistä kopioitiin ja mihin
# -n eli No overwrite ei päällekirjoita, jos kohdekansiossa olisi jo vastaavaa
```
Tämän jälkeen tarkistin löytyykö siirretty .ssh kansio käyttäjän tuke kotikansiosta.
```
$ cd /home/tuke
$ ls -la            # Näyttää kansion sisällön laajemmin ja eri sisällön oikeudet
```
Koska .ssh kansio oli siirtynyt mutta oikeudet sen käyttöön oli vieläkin root käyttäjällä ei tuke käyttäjällä piti ne siirtää.
```
$ sudo chown -R tuke:tuke /home/tuke/      # -R taas rekursiivinen eli kaikki kansion sisältä ja niiden sisältä löytyvä tieto siirretään käyttäjälle tuke
$ ls -la                                   # uusi tarkistus, että oikeudet ovat muuttuneet
```
Nyt pystyin tarkistamaan pääsikö tuke käyttäjällä kirjautumaan jo sisälle porkkanapalvelimelle. Avasin toisessa terminaalissa yhteyden uudestaan. Ja tarkistin, että sudo oikeudet toimivat.
```
DEBIANTUKE
$ ssh tuke@94.237.118.113
PORKKANA
$ sudo echo moikka          # Tämä varmisti sudo toimivuuden, kun kysyi salasanaa ja toimi salasanan syötön jälkeen
```

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/sshUserAccess.png width=850>

Nyt olin sisällä tuke käyttäjänä ja lähdin sulkemaan root käyttäjän pois. Syötin komennot:
```
$ sudo usermod --lock root      # lukitsee salasana kirjautumisen root käyttäjältä HUOM!! ei ssh-avaimella kirjautumista
$ sudo mv -nv /root/.ssh /root/DISABLE-ssh/  # vaihtaa .ssh kansion nimen DISABLE-ssh, jolloin kirjautuessa sitä ei osata yhdistää
```
Tämän jälkeen yritin kirjautua root käyttäjälle sisälle porkkanapalvelimelle ja se ei enää onnistunut.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/sshRootLock.png width=850>

Nyt lähdin päivittämään porkkanapalvelimen käyttöjärjestelmän ja asentamaan muutamia ohjelmia.
```
$ sudo apt-get update      # Päivittää ohjelmalistan
$ sudo apt-get dist-upgrade  # päivittää käyttöjärjestelmän tarvittaessa ja kaikki sen sisältämät paketit, poistaa tarvittaessa turhia dependencyjä
$ sudo apt-get update
$ sudo apt-get -y install micro bash-completion curl    # asentaa ohjelmat micro bach-completion ja curl. Painaa tarvittaessa "y" kysymyksiin
```
Tupla updatea ei olis tarvinnut, mutta automaatiolla kirjasin läpi ennen ohjelmien asennuksia.

## c - Webpalvelimen asennus (Apache)
Start: 14.47\
Finish: 14.54

Lähdin asentamaan porkkanapalvelimelle webpalvelinta (Apache), tämä toistaa osittain aikasempaa tehtävää, mutta muutamia eroja on. Alkuun asensin Apachen komennolla:
```
$ sudo apt-get install apache2
```
Tämän jälkeen ajoin tarkistuksen läpi.
```
$ sudo systemctl statu apache2
```

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/apacheStatus.png width=850>

Eli Apache oli asentunut onnistuneesti. En kuitenkaan päässyt vielä Apachen default sivulle julkisen IP:n(94.237.118.113) kautta. Piti siis tehdä palomuurin aukko.
```
$ sudo ufw allow 80/tcp    # luo reiän palomuuriin portille 80
$ sudo ufw status          # tarkistaa palomuurin statuksen
```

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/apacheFirewall.png width=850>

Nyt aukesi jo Apachen default sivu julkisen IP:n(94.237.118.113) kautta. Siirryin siis vaihtamaan olemassa olevan index tiedoston sisällön uuteen.
```
$ echo Tuken uudet nettisivut porkkanapalvelimella | sudo tee /var/www/html/index.html
```
Päivitti index.html tiedoston, jota Apache käyttää default sivupohjanaan, vain uudella tekstillä. Muutoksen jälkeen näytti tältä.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/apacheUusiIndex.png width=850>

Kaikki tässä vaiheessa toimi siis kuten pitääkin.

## d - Vapaaehtoinen Name Based Virtual Host
Start: 15.12\
Finish: 16.00

Lähdin asettamaan Name Based Virtual Hostia aikaisemman työkirjan h3 (https://github.com/GHTuke/linux-palvelimet/blob/main/h3-Hello-Web-Server/h3-Hello-Web-Server.md) mukaisesti.
```
$ sudoedit /etc/apache2/sites-available/porkkana.example.com.conf

#Avautuvaan tekstieditoriin#
<VirtualHost *:80>
        ServerName porkkana.example.com
        ServerAlias www.porkkana.example.com
        DocumentRoot /home/tuke/public_sites/porkkana.example.com

                <Directory /home/tuke/public_sites/porkkana.example.com>
                Require all granted
                </Directory>
</VirtualHost>
```

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/porkkanaExampleConf.png width=850>

Alkuun tarkistin vain demotakseni, että onko kansiota /home/tuke/public_sites/porkkana.example.com olemassa.
```
$ ls /home/tuke/public_sites/porkkana.example.com
```
Ei löytynyt niin kun ei pitänytkään, joten yritin luoda kansion.
```
$ mkdir /home/tuke/public_sites/porkkana.example.com
```
Jostain syystä tässä tuli ongelma ja tuli virheilmoitus "Cannot access /home/tuke/public_sites/porkkana.example.com No such file or directory".\
Päätin hypätä kotikansioon ja luoda kansion sieltä käsin, jos vaikka kirjasin jotain väärin.
```
$ cd
$ pwd     # tarkistin vain, että olen oikeassa kansiossa
$ mkdir public_sites/porkkana.example.com
```
Ei vieläkään mennyt läpi, päätin kokeilla osissa.
```
$ mkdir public_sites            # tässä vaiheessa uusi kansio luotiin
$ ls                            # näkyi uusi kansio
$ cd public_sites               # siirryin uuteen kansioon
$ mkdir porkkana.example.com    # taas onnistui uuden kansion luonti
```

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/porkkanaMkdir.png width=850>

Tässä vaiheessa ajattelin, että olin vain tehnyt kirjoitusvirheen jossain aiemmin, enkä sen enempää miettinyt asiaa, koska kansio oli nyt luotu. Lähdin siis työstämään seuraavaa vaihetta ja enabloimaan luotua sivua sekä sulkemaan vanhaa default sivua.
```
$ sudo a2ensite porkkana.example.com.conf        # enabloi sivun apachen webpalvelimelle
$ sudo systemctl restart apache2                 # käynnistää apachen uudestaan, että muutokset tulevat voimaan
$ sudo a2dissite 000-default.conf                # poistaa vanhan default sivun käytöstä
$ sudo systemctl restart apache2
```

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/porkkanaEnsite.png width=850>

Kaikki oli mielestäni tässä vaiheessa niin kuin piti joten lähdin luomaan testisivua kansioon.
```
$ cd
$ ls    # oli jäänyt tässä vaiheessa joku epäilys takaraivoon ja tarkistelin kokoajan kansiota missä olin
$ cd public_sites
$ ls
$ cd porkkana.example.com
$ ls    # oli kansio tyhjä joten indexin luontiin
$ micro index.html    # Kirjasin alkuun vaan tekstin "Tuken uusi porkkanasivu TÄLLÄ KERTAA EXTRA NAME BASED HOSTINGILLA"
$ curl localhost
```
Tässä vaiheessa tuli ongelma, sivu ei auennut ja syöte oli 403 Forbidden.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/porkkanaOngelma.png width=850>

Lähdin selvittämään logeista.
```
$ sudo tail -1 /var/log/apache2/error.log    # avaa viimeisen rivin apache2 ohjelman error logeista
```
Kuten ylempänä olevasta kuvasta näkyy tärkeä osa virheilmoitusta oli `filesystem path /home/tuke/public_sites because search permissions are missing on a component of the path`.\
Eli siis jossain on ongelmia oikeuksien kanssa. Muistelin aiemmin lukemaani Susanna Lehdon webkirjoitusta, jossa mainittiin komento, jota en ollut käyttänyt joka liittyi oikeuksiin. Katselin dokumentaatiosta, että ei pitäisi todennäköisesti olla tästä kiinni oma ongelmani koska se mahdollistaaa suoraan /home kansioon luotujen kansioden pathin käytön apachella. Päätin kuitenkin kokeilla ja ajaa sitten testit uudestaan.
```
$ sudo a2enmod userdir
$ sudo systemctl restart apache2
$ curl localhost
$ sudo tail -1 /var/log/apache2/error.log
```
Odotetusti mikään ei ollut muuttunut, mutta ainakin suljin sen vaihtoehdon pois. Löysin vielä tavan tarkistaa eri kansioiden tarkemmat oikeudet (löytyi ls komennon help osioista`ls --help | less`). Joten päätin tarkastaa kaikki kansiot väliltä, jossa aiemmin oli luonnin kanssa ongelmia.
```
$ ls -ld /home /home/tuke /home/tuke/public_sites
```
Tässä vaiheessa löytyi virhe.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/porkkanaAenModUserDir.png width=850>

Alhaalla näkyy, että kansiossa /home/tuke ei ole xr oikeuksia. x = execute ja r = read. Lähdin etsimään alkuperäisellä virheilmoituksella tapaa korjata ongelma, ja löysin StackOverflow:sta vastaavan ongelman. (https://stackoverflow.com/questions/25190043/apache-permissions-are-missing-on-a-component-of-the-path). Sivulla oli yksi ohje chmod käytöstä, jota päätin kokeilla. Tässä vaiheessa tuli myös pari kirjoitusvirhettä, jotka näkyvät kuvassa, mutta lopulta pääsin etenemään.
```
$ sudo chmod +x /home /home/tuke    # /home oli tässä vaiheessa turha, koska kyseisessä kansiossa oli execute oikeudet
$ ls -ld /home /home/tuke /home/tuke/public_sites
$ curl localhost
```
Tässä vaiheessa ls -ld näytti oikeat oikeudet ja curl localhost näytti oikean sivun tekstiversion. Sivu jopa aukesi julkisen IP:n kautta niin kuin pitikin.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/porkkanaKuulee.png width=850>

Muutin vielä tässä vaiheessa toimivan sivun index.html tiedostoa vähän siistimpään muotoon. Tässä sen html koodi ja toimiva sivu.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/porkkanaValmis.png width=850>

## Lähteet

Huovilainen, T. H3-Hello Web Server. https://github.com/GHTuke/linux-palvelimet/blob/main/h3-Hello-Web-Server/h3-Hello-Web-Server.md.

Karvinen, T. First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS. https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/.

Lehto, S. Teoriasta käytäntöön pilvipalvelimen avulla (h4). https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/.

UpCloud. Utility Network Documentation. https://upcloud.com/docs/products/networking/features/utility-network/.

StackOverflow. Apache - Permissions are missing on a component of the path. https://stackoverflow.com/questions/25190043/apache-permissions-are-missing-on-a-component-of-the-path.

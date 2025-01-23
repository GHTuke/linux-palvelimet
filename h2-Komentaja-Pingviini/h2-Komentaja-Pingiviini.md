# H2 - Komentaja Pingviini

Tekijä: Tuukka Huovilainen

Tässä työkirjassa toteutan kurssin "Linux palvelimet" tehtävää H2-Komentaja Pingviini, jossa suoritetaan erilaisia komentokehote komentoja aiemmin toteutetussa virtuaalikoneelle asennetussa Debian Linuxissa (https://github.com/GHTuke/linux-palvelimet/blob/main/h1-Oma-linux/h1-raportti.md).\
Alkuun tiivistetään pääpiirteitä Tero Karvisen webbikirjoituksesta "Command Line Basics Revisited".

## x - Tiivistelmä
Kerättynä muutamia pääpirteitä Tero Karvisen webbikirjoituksesta https://terokarvinen.com/2020/command-line-basics-revisited/.

-Komentokehote on kestänyt ajantestin ja on ollut olemassa jo kauemmin kuin internet tai eri käyttöjärjestelmät.\
-/home/USERNAME/ tärkeä hakemisto, koska se on ainut minne käyttäjä USERNAME voi tallentaa pysyvästi dataa.\
-/etc/ alaisuudesta löytyy kaikki systeemin asetukset plain tekstinä.\
-Suositeltavaa toteuttaa minimum privilege-mallia ja käyttää "sudo" komentoja ainoastaan kun niitä tarvitaan kyseisen komennon ajamiseen.\
       * Ohjelmistojen asennukset\
       * Ohjelmistojen poistaminen\
       * Käyttäjien luonti\
       * Oikeuksien hallinointi

## Komentokehote

Localtime: 22-01-2025 12.35
Muokattu: Localtime: 23-01-2025 12.10

Host specs:

    Windows 11 Home v. 23H2
    AMD Ryzen 7 6800H
    RAM 16 Gt
    VirtualBox v. 7.1.4

Virtual specs:

    Debian amd64 12(bookworm)
    VRAM 128MB
    RAM 4 Gt
    VHarddisk 40GB

Alkuun varmistin pakettihallinnan ajankohtaisuuden ajamalla Terminaalissa komennon:
```
   $ sudo apt-get update
```

### a - Micro
Localtime: 12.35

Micro on terminaalipohjainen tekstikäsittelijä, se oli helppo ladata komennoilla:
```
   $ sudo apt-get install micro
```
Jonka jälkeen sitä pystyi alkaa käyttämään suoraan terminaalista komennolla:
```
   $ micro
```
![Micro_asennus](https://github.com/user-attachments/assets/e537e729-6ece-4d75-9027-3b07df2a58f3)

### b - Apt
Localtime: 13.00

Valitsin asennettaviksi työkaluiksi:\
   *htop - prosessi seurantaa\
   *git - versio hallintaa\
   *vim - teksti editori\
   *cowsay - turha, mutta miksi ei. Lehmä sanoo mitä pyydät.\
Asennukset linkittyivät helposti yhdeksi komennoksi:
```
   $ sudo apt-get install -y htop git vim cowsay
```
Kuten monen muunkin terminaali komennon kanssa pystyi useamman ohjelman nimeämään peräkkäin välilyönnillä erottaen, jolloin ne erotellaan eri "tiedostoiksi".\
-y vastaa tarvittaessa automaattisesti kyllä asennuksen aikana tuleviin kysymyksiin.\
Täältä löytyy asentamista varten kaikkea muutakin hyödyllistä: https://linuxize.com/post/how-to-use-apt-command/.\
![Vim](https://github.com/user-attachments/assets/ca01f249-7d6f-4a22-bdfb-0c86a43fc7c9)
Poistuminen Vim:istä komennolla: :q+ENTER. Muistakaa painaa ESC, jotta pääsee antamaan komentoja.

### c - FHS
Localtime: 13.20

Root:iin pääsi käsiksi komennolla:
```
   $ cd /
```
Sieltä listaamalla sisällön:
```
   $ ls -F (-F lisää kansioiden perään / merkin selkeyden vuoksi)
```
Tärkeitä kansiota joihin pääsee käsiksi cd /*Polku, jos lähtee Root:ista liikkeelle:
```
   /home/username/ - käyttäjän omat kansiot
   /etc/ - Systeemin asetukset
   /media/ - Kiinni olevat mediat esim USB-tikut jne
   /var/log - Systeemin logit
```
![hakemistoja](https://github.com/user-attachments/assets/118c53ad-f4c0-4f17-8792-a99a4b336b4c)

### d - The friendly M
Localtime: 13.50

Grep:illä pystyy hakemaan tiedostoista rivejä tai ympäröiviä rivejä merkkijonojen perusteella.\
Käytin grep:iä tarkentamaan komentoa:
```
   $ apt-cache search dungeon
   $ apt-cache search dungeon|grep nethack
```
Tällä tavalla putkitettuna suorittaa grep sille syötetyn tiedon perusteella rivihaun, etsien sanaa "nethack". Poistaa tässä tapauksessa hausta kaikki ohjelmistot joiden nimessä ei ole kyseistä sanaa.

Loin uuden tekstitiedoston polulla /home/tuke/jokutesti/tamatesti, johon kirjoitin "etsi tämä teksti" käyttäen grep:iä komennolla:
```
   $ grep -r "etsi tämä teksti" /home/tuke
```
grep suoritti etsinnän tarkistaen kaikki /home/tuke kansiosta löytyvät alikansiot ja tekstitiedostot etsien vain riviä, jolta löytyy "etsi tämä teksti".\
grep printtaa polun kyseiseen tiedostoon ja mitä kyseiseltä riviltä löytyy.
Lisäsin vielä komentoon:
```
   $ grep -r -C 2 "etsi tämä teksti" /home/tuke
```
Näin grep hakee myös etsittyä riviä ylempänä sekä alempana olevat 2 riviä hakuun.
![grep](https://github.com/user-attachments/assets/809fd69d-5b18-40b4-9b4c-a3e6e093db8c)

### e - Pipe

Putkilla pystyy yhdistämään komentoja järjestyksessä syöttämällä | komentojen väliin.\
Komento:
```
   $ apt-cache search python3
```
Palauttaa esimerkiksi niin paljon ladattavia ohjelmistoja, että tällä ei tee itsessään mitään. Muokkamalla komentoa voidaan selkeyttää määrää ja tehdä siitä helpommin läpikäytävä.
```
   $ apt-cache search python3|nl|less
```
Suorittaa saman haun, jonka jälkeen asettaa riveille numeroinnin (nl) ja sen jälkeen avaa ne muodossa, jossa sivuja on helppo käydä läpi (less).

### f - Rauta
Localtime: 14.00

![lshwShortSanitize](https://github.com/user-attachments/assets/fa366356-8f9d-497f-84e1-0c77296c8a40)

Asentamalla lshw komennolla $ sudo apt-get install lshw, sai käyttöön lshw työkalun, joka listaa koneen tietoja.\
Kysessää virtuaalikone, joka on luotu virtualboxilla, ja tämä näkyi jonkun verran syötteessä mm. disk (tallennustilaa) on VBOX HARDDISK sekä hiiren input on VirtualBox mouse integration.\
Jonkun verran löytyi Host koneen tietoja myös tätä kautta. Esimerkkinä Prosessori sekä network, jotka ottavat tietonsa suoraan Host koneesta.

### g - Lokit
Localtime: 14.15

![lokit](https://github.com/user-attachments/assets/acbede54-5c8e-4d98-9e1d-2069b4756750)

Tässä tapauksessa avasin lokeja komennolla $ journalctl, eli ilman suurempia oikeuksia. Näyttää siis yleisempiä tapahtumia koneelta.\
Ympyröitynä aikaisemmin suoritetun lshw asennus sekä koneen tietojen tarkastelu sen kautta. Lokit näyttävät, missä komento suoritettiin (PWD), käyttäjä tässä tapauksessa "root", koska suoritettiin sudo oikeuksilla. Sekä komento, joka on suoritettu. Näkyy myös kellonajat suorituksen aloitukselle, sekä lopettamiselle.

### h - Micron plugin
Localtime: 14.30

Päätin asentaa Palettero pluginin Microon.
$ man micro komennon kautta oli helppo tarkistaa kuinka plugineja pystyy microon asentamaan. Micron avattua ctrl+E avaa micron komentosyötteen, johon saa komennolla "plugin available" näkyviin saatavilla olevat pluginit. Komento "plugin install palettero" asentaa pluginin ja "plugin list" näytti mitä plugineja oli asennettuna.\
![pluginAvailable](https://github.com/user-attachments/assets/54ee1a64-93e9-49b2-84e4-8070cb66edb6)

Ongelmia tuli ja palettero ei tunnistanut versiotaan, väitti että päivityksiä ei tarvita ja versio oli merkattuna 0.0.0.\
![paletteroOngelma](https://github.com/user-attachments/assets/a3336c6e-022c-4df5-9137-03e292fb7712)

Muutaman asennuksen, päivityksen, terminaalin uudelleen käynnistyksen jälkeen päädyin poistamaan pluginin.
```
   $ cd home/tuke/.config/micro/plug
   $ rm -rf palettero
```
Asennettua paletteron uudestaan ja micron uudelleenkäynnistyksen jälkeen ilmestyy komentosyötteeseen teksti "First run, Palettero running 'updatemenu'...".\
Kyseinen updatemenu ei 10 minuutin ajon jälkeen ollut päivittynyt, joten lähdin etsimään uutta ratkaisua.

Paused: Localtime 15.15\
Continued: Localtime 19.30

Pienen tauon jälkeen tulin käsittelemään ongelmaa uudelleen ja tarkistettua paletteron github (https://github.com/terokarvinen/palettero) tutun nimiseltä henkilöltä tuli vastaan tarve ohjelman "fzf" asentamiselle. Tämän asennuksen suoritettua onnistui micron sisällä avata "palettero --help" komennolla komentopankki, josta pystyi katsomaan toimivia komentoja.

Säädin Paletteron kautta komennolla "set colorscheme simple" micron taustavärin mustaksi ja tekstin valkoiseksi. Samalla vaihtui rivinumerointi punaiseksi, mistä ei lukenut erikseen komentopankissa.\
![paletteroKorjattu](https://github.com/user-attachments/assets/e07a2ff5-12ef-432c-a696-3451ebb64c03)

Myös "replaceall foo BAR" komentoa kokeiltu ja muutti jokaisen tiedostosta löytyvän foo:n BAR:iksi, toimii siis sananvaihtajana.

## Nethack

Ja koska Nethack:iä on tullut lapsena pelattuna niin paljon oli pakko tilaisuuden tullen vielä asentaa se.
```
   $ apt-cache search nethack   etsii nethack nimellä ohjelmia
   $ sudo apt-get -y install nethack-console
   $ dpkg --listfiles nethack-console    listaa nethack-console kansiot ja niiden polut
   $ cd usr/lib/games/nethack
   $ nethack
```
![NetHack](https://github.com/user-attachments/assets/3902eed8-9f76-4059-9572-989931e58b75)


## Lähteet

Karvinen, Tero. 2020-02-03. Command Line Basics Revisited. https://terokarvinen.com/2020/command-line-basics-revisited/.

# Maalisuora
Tekijä: Tuukka Huovilainen

* Pohjana Tero Karvinen 2025: Linux Palvelimet kurssi, http://terokarvinen.com

Tässä työkirjassa suoritetaan Linux Palvelimet -kurssin tehtävää Maalisuora. Alkuun luodaan muutama perus "Hello world" koodin pätkä kolmella eri kielellä. Sen jälkeen luodaan Linuxiin uusi komento shellscriptillä ja asetetaan se kaikkien käyttäjien käytettäväksi. Sen jälkeen suoritetaan yksi vanha lopputehtävä aiemmalta kurssilta.

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

## a - Hello, world
Localtime: 05.03.2025\
Start: 12.35\
Finish: 12.40

Alkuun päivitin paketin hallinta listan ja lähdin asentamaan javaa ja luaa. Kolmantena ohjelmointikielenä tehtävää varten käytän Pythonia, mutta se löytyy jo valmiiksi asennettuna Debianista.
```
sudo apt-get update
sudo apt-get install -y openjdk-17-jdk lua5.4
```
 Tämän jälkeen lähdin kirjoittamaan print lauseita kaikille kolmelle kielelle. Loin alkuun `mkdir helloWorld` komennolla kansion, jonna sitten loin micro:lla tekstitiedostot kaikille.
```
micro HelloPython.py
micro HelloJava.java
micro HelloLua.lua
```

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/helloKansio.png width=800>

Kuva otettu Javan kokoamis komennon käytön jälkeen, sen takia kansiosta löytyy kaksi eri tiedostoa päätteillä .java ja .class. .class tiedosto on se, jonka java sitten ajaa komennossa ja .java sisältää lähdekoodin.

Sitten kirjoitin koodit kaikille ja testasin niiden ajon. Jokaisesta kielestä on erikseen merkattu cat komento, jotta näkyy tiedoston sisältö, sekä ajettu koodinpätkät.
### Python
```
print(f'Hello World!')
```
<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/pythonHello.png width=800>

### Java

```
public class HelloJava {
	public static void main(String[] Args) {
		System.out.println("Hello World!");
	}
	
}
```
Javan kanssa täytyi koodi vielä erikseen koota sen kirjoittamisen jälkeen. `javac HelloPython.java` luo .java tiedostosta kootun .class tiedoston, jonka voi sitten ajaa komennolla `java HelloJava`

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/javaHello.png width=800>

### Lua

Luaa en ollut aiemmin käyttänyt, joten tarkistin varmuuden vuoksi syntaksin https://terokarvinen.com/2018/hello-python3-bash-c-c-go-lua-ruby-java-programming-languages-on-ubuntu-18-04/.\
Oli tosin melkeinpä identtinen Pythonin kanssa tässä tapauksessa.

```
print("Hello World!")
```
<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/luaHello.png width=800>

## c - Uusi linux komento
Localtime: 05.03.2025\
Start: 13.15\
Finish: 13.25

Aloitin tehtävän luomalla komennolla `micro morning.sh` uuden shellscript tiedoston. Kirjasin sinne seuraavanlaisen komennon.
```
#!/usr/bin/bash		# "shebang" komento joka kertoo millä ohjelmalla tiedosto ajetaan https://www.baeldung.com/linux/shebang

cowsay -e'@@' Good Morning!	# Luo lehmän kuvan, joka sanoo "Good Morning!" -e'@@' vaihtaa lehmän silmät annetuksi merkkijonoksi
date +'Today is %d.%m.%Y'	# Printtaa päivämäärän
echo Todos for today:		# Elämää helpottava print, joka kertoo kaiken tarvittavan päivälle
echo 1. Nothing
```
<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/microMorning.png width=600>

Tiedostolla ei alkuun ollut execute oikeuksia, joten jotta myöhemmin myös muut voisivat tiedostoa ajaa lisäsin siihen käyttäjäoikeuksia `chmod ugo+x morning.sh`. Komento lisää execute oikeudet tiedoston omistaja userille(u), tiedoston omistajaryhmälle groupille(g) sekä othersille(o) eli muille (tämän vuoksi ugo).

Koska lisäsin heti alkuun shebang komennon tiedostoon, pystyin ajamaan sen heti ilman `bash` etuliitettä komennolla `./morning.sh`.

Tämän jälkeen poistin tiedoston nimestä .sh päätteen komennolla `mv morning.sh morning`. Sitten siirsin sen kaikkien käytettäväksi `sudo cp morning /usr/local/bin`.

Testatakseni, että komento toimii nyt myös muilla käyttäjillä loin uuden käyttäjän `sudo adduser testeri`. Ja kirjauduin sisälle käyttäjälle `su testeri`.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/testeriMorningUusi.png width=800>

Näyttääkseni, että uudella käyttäjälle ei ollut oikeuksia tuke käyttäjän kotikansioon ajoin `ls` komennon tuken kotikansiossa, jonka jälkeen siirryin testerin omaan kotikansioon ja varmistin, että se on myös tyhjä. Sen jälkeen ajoin luomani komennon `morning` ja se toimi.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/testeriMorning.png width=800>

Siirryin vielä takaisin tuke käyttäjälle `su tuke` ja ajoin uudestaan `morning` komennon.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/tukeMorning.png width=600>

## d - Vanha labra
Localtime: 08.03.2025\
Start: 15.40\
Finish: 16.35

Valitsin laboratorioharjoituksekseni ehdotetulta listalta. Kevään 2024 Linux-palvelimet kurssin loppulaboratorion https://terokarvinen.com/2024/arvioitava-laboratorioharjoitus-2024-linux-palvelimet/.

Siitä soveltuvin osin suoritin osat d/e/g. Jätin siis pois itseni esittelyt ja lopusta Djangon, jota ei tällä kurssilla ole käsitelty.

### g Salattua hallintaa

* Asenna ssh-palvelin
* Tee uusi käyttäjä omalla nimelläsi, esim. minä tekisin "Tero Karvinen test", login name: "terote01"
* Automatisoi ssh-kirjautuminen julkisen avaimen menetelmällä, niin että et tarvitse salasanoja, kun kirjaudut sisään. Voit käyttää kirjautumiseen localhost-osoitetta

Tarkempia yksityiskohtia tehtävässä suoritettuihin toimenpiteisiin löytyy https://github.com/GHTuke/linux-palvelimet/blob/main/h4-Maailma-kuulee/h4-Maailma-kuulee.md.

Aloitin tehtävän toteuttamisen kohdasta g, koska joka tapauksessa olin suorittamassa tehtävää uuden palvelimen kautta, jonka loin upCloudiin. Suoritin siis loputkin tehtävät sitten tämän kyseisen palvelimen sisällä. Kun olin luonut uuden palvelimen upCloudille kirjauduin sille komennolla `ssh root@80.69.173.203`. Koska kyseisellä palvelimella ei ollut aiemmin tehty mitään suoritin liudan komentoja saadakseni palvelimen ajantasalle sekä käynnistääkseni palomuurin.
```
$ sudo apt-get dist-upgrade
$ sudo apt-get update
$ sudo apt-get install ufw
$ ufw status
$ sudo ufw enable 22/tcp
$ ufw status 
```
<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/ufwStatusOK.png width=800>

Tämän jälkeen, kun oli palomuurissa reikä tulevia yhteyksiä varten, lähdin luomaan uutta käyttäjää ja siirtämään tälle ssh kirjautumis oikeuksia.
```
$ sudo adduser tukete01
$ sudo cp -rvn /root/.ssh/ /home/tukete01/
$ sudo chown -R tukete01:tukete01 /home/tukete01/
$ sudo adduser tukete01 sudo
```
Lisäsin tulevia tehtäviä varten uudelle käyttäjälle sudo oikeudet. Nyt pystyin jo kirjautumaan palvelimelle ilman salasanaa uudella käyttäjällä komennolla ´ssh tukete01@80.69.173.203´ ja testasin vielä sudon toiminnan komennolla `sudo echo moikka` ja syöttämällä salasanan.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/uusikayttajaSSH.png width=800>

### d 'howdy'

* Tee kaikkien käyttäjien käyttöön komento 'howdy'
        Tulosta haluamaasi ajankohtaista tietoa, esim päivämäärä, koneen osoite tms\
        Pelkkä "hei maailma" ei riitä
* Komennon tulee toimia kaikilla käyttäjillä työhakemistosta riippumatta

Tarkempia ohjeita tehtävän suorittamiseen löytyy tämän työkirjan aiemmista osista.

Asensin alkuun micron tekstieditoriksi komennolla `sudo apt-get install micro`. Lähdin sen jälkeen luomaan uutta komentoa luomalla shell script tiedoston `micro howdy.sh`. Sen sisälle kirjoitin seuraavanlaisen koodin.
```
#!/usr/bin/bash

echo Howdy
whoami
date +'Today is %d.%m.&Y'
```
Tämän jälkeen muutin howdy.sh käyttäjäoikeudet niin, että kaikki saivat execute oikeudet siihen. `chmod ugo+x howdy.sh`. Nyt komento toimi jo ilman bash komentoa edessä sillä tiedostossa oli shebang toiminto alussa ohjaamaan bashin käyttöä. Komennon pystyi siis jo ajamaan komennolla `./howdy.sh`.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/valiHowdy.png width=800>

Sitten vaihdoin tiedoston nimen komennolla `mv howdy.sh howdy` ja siirsin sen local biniin kaikille käyttäjille komennolla `sudo cp howdy /usr/local/bin`. Nyt komento toimi kaikilla käyttäjillä komennolla `howdy`.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/HowdyToimii.png width=800>

### e Etusivu uusiksi

* Asenna Apache-weppipalvelin
* Tee yrityksellemme "AI Kakone" kotisivu
* Kotisivu tulee näkyä koneesi IP-osoitteella suoraan etusivulla
* Sivua pitää päästä muokkaamaan normaalin käyttäjän oikeuksin (ilman sudoa). Liitä raporttiisi listaus tarvittavien tiedostojen ja kansioiden oikeuksista.

Tarkempia askeleita vastaavan tekemiseen löytyy https://github.com/GHTuke/linux-palvelimet/blob/main/h3-Hello-Web-Server/h3-Hello-Web-Server.md.

Aloitin tehtävän asentamalla apache webpalvelimen komennolla `sudo apt-get install apache2`. Sen jälkeen testasin sen toiminnan komennolla `curl localhost` ja apachen default sivu oli tullut näkyviin.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/curlLocalhost.png width=800>

Kuvassa lyhyt versio mitä sivulla näkyy, tärkeimpänä kuitenkin kohta `<title>Apache2 Debian Default Page: It works</title>`.

Lähdin siis luomaan uutta Al Kakone sivua komennolla `sudoedit /etc/apache2/sites-available/alkakone.com.conf`. HOX alkakone kirjoitetaan pienellä L kirjaimella.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/sudoEdit.png width=800>

Kirjoitin vastaavanlaisen sisällön conf tiedostolle. Kuvan versiossa lukee virheellisesti <VirtualHost *80>, korjasin pian kuvan ottamisen jälkeen sen muotoon <VirtualHost *:80>.\
Tämän jälkeen suoritin muutaman eri komennon avatakseni reiän palomuuriin ja ottaakseni sivun käyttöön.
```
$ sudo ufw allow 80/tcp
$ ufw status
$ sudo a2ensite alkakone.com.conf
$ sudo a2dissite 000-default.conf
$ sudo systemctl restart apache2
```
Nyt sivu oli asetettu toimintaan, mutta vielä ei ollut index sivua, mitä näyttää. Loin siis uuden sivun ja polun.
```
$ cd
$ mkdir sites
$ cd sites/
$ mkdir alkakone.com
$ cd alkakone.com
$ micro index.html
```
Kirjasin sinne seuraavanlaisen html koodin.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/alKakoneHTML.png width=800>

Sen jälkeen käynnistin apachen uudestaan komennolla `sudo systemctl restart apache2`. Sivu ei lähtenyt toimimaan, tarkistin apachen error logit komennolla `sudo tail -1 /var/log/apache2/error.log` ja huomasin, että käyttäjäoikeuksissa polun varrella oli ongelmia. Sama ongelma oli ollut aiemmin kun olin tehnyt vastaavaa ja muistin tarkistaa polun oikeudet komennolla `ls -la`. tukete01 käyttäjän kotikansiosta puuttui execute oikeudet muille, joten apache ei päässyt suorittamaan sen sisältä index sivua, joten lisäsin tarvittavat oikeudet `chmod ugo+x /home/tukete01` ja taas `sudo systemctl restart apache2`. Nyt sivu käynnistyi normaalisti.

<img src=https://github.com/GHTuke/linux-palvelimet/blob/main/h7-Maalisuora/alkakoneToimii.png width=800>

<img src= width=800>

## Lähteet

chmod. Manual chmod. man chmod.

Dąbrowski, M. Baeldung. Using Shebang #! in Linux. https://www.baeldung.com/linux/shebang. 

Karvinen, T. Linux palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/.



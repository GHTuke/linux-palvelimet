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

<img src= width=800>

## Lähteet

chmod. Manual chmod. man chmod.

Dąbrowski, M. Baeldung. Using Shebang #! in Linux. https://www.baeldung.com/linux/shebang. 

Karvinen, T. Linux palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/.



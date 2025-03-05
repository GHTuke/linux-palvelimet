# Maalisuora
Tekijä: Tuukka Huovilainen

* Pohjana Tero Karvinen 2025: Linux Palvelimet kurssi, http://terokarvinen.com

Tässä työkirjassa suoritetaan Linux Palvelimet -kurssin tehtävää Maalisuora. Alkuun luodaan muutama perus "Hello world" koodin pätkä kolmella eri kielellä. Sen jälkeen luodaan Linuxiin uusi komento shellscriptillä ja asetetaan se kaikkien käyttäjien käytettäväksi. Sen jälkeen suoritetaan yksi vanha lopputehtävä aiemmalta kurssilta.

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

<img src= width=800>

## Lähteet

Karvinen, T. Linux palvelimet 2025 alkukevät. https://terokarvinen.com/linux-palvelimet/.





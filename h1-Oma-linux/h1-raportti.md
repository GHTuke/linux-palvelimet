# Tehtävä h1 - Oma linux

## x - Tiivistelmä

### Raportin kirjoittaminen
Pohjateksti: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/

- Toistettavuus

    Suositeltavaa merkitä mahdollisimman tarkat tiedot laitteesta ja ajasta jolloin harjoituksen suorittaa. Tällä tavalla vianselvittäminen on mielekkäämpää ja virheiden toistettavuutta voidaan tarkistaa tarvittaessa. (Karvinen, T. 2006)

- Täsmällisyys

    Merkkaa käytetyt komennot ja napinpainallukset, ja mitä näistä tapahtui (esim. onnistuiko vai eikö). Kirjaa ylös miten testasit, jos jokin ei toiminut. Johtuuko vika työkaluista kuten ohjelmista vai raudasta, vaikuttaa virheilmoitusten edistämiseen. Kirjoita "mitä teit" älä "mitä tulen tekemään", eli imperfektissä. (Karvinen, T. 2006)

- Helppolukuisuus ja lähdeviittaukset

    Väliotsikot luovat rakennetta. Kirjoita huolellista tekstiä ja pyri minimoimaan kirjoitusvirheet. Merkkaa lähteet, osoittaa perehtyneisyyttä ja kreditoi alkuperäisen tekstin luojia. (Karvinen, T. 2006)

### Free software
Pohjateksti: https://www.gnu.org/philosophy/free-sw.html

Free Software Foundation (FSF) (Free Software Foundation, Inc. 2024) määrittelemät 4 vapautta ohjelmistoille.

  0) Vapaus käyttää ohjelmistoa miten haluaa, mihin vain käyttötapaukseen.
  1) Vapaus opiskella miten ohjelmisto toimii, ja sen muokkaus jotta se toimii niin kuin itse haluat.
  2) Vapaus jakaa kopioita toisille.
  3) Vapaus jakaa kopiota omasta muokkaamastasi versiostasi.

- Vapaa ohjelmisto (Free software) ei tarkoita ettei sitä voi käyttää kaupallisiin tarkoituksiin.
- Lähdekoodin opiskelu ja muokkaus ovat vapaan ohjelmiston peruspilareita.
- Vapaa ohjelmisto (Free software) ja avoin lähde (Open source) eivät tarkoita samaa.

## a - Oman linuxin asennus virtuaalikoneelle
Harjoitus seuraa Tero Karvisen ohjeita: https://terokarvinen.com/2021/install-debian-on-virtualbox/
Harjoitus ajankohta: 2025-01-15

Ympäristö: 
- Windows 11 Home v. 23H2
- AMD Ryzen 7 6800H
- RAM 16 Gt
- VirtualBox v. 7.1.4

### Debian live ISO
Aluksi latasin viimeisimmän vakaan (Stable) Debianin live imagen sivulta https://www.debian.org/CD/live/ .
Versio 12.9.0 Amd64 Xfce

### VirtualBox virtuaalikoneen luonti

VirtualBoxissa valitsin ylävalikosta Machine -> New
TÄHÄN KUVA VM-luonti
Aunneeseen ikkunaan "Virtual machine Name and Operating System" kirjasin nimen, valitsin tallennuskansion sekä valitsin tyypiksi Linux ja Debian 64-bit.
Tästä eteenpäin "Next".

Aukesi "Hardware" ikkuna johon kirjasin Base memory kohtaan 4000 Mb, kertoo siis RAMin määrän. Tähän olisi voinut kirjata 4096 Mb, meni itseltä hieman liian automaatiolla.
TÄHÄN KUVA VM-Hardware
Tästä taas eteenpäin "Next".

Seuraavaksi siirryttiin "Virtual Hard disk" ikkunaan.
TÄHÄN KUVA VM-harddisk
Valitsin Create a Virtual Hard disk now ja asetin levykkeen kooksi 40 Gb.
Muut asetukset Defaultilla.
Eteenpäin taas "Next".

Tässä vaiheessa tuli tietojen koonti ikkuna, josta VM image luotiin painamalla "Finish"

Nyt VirtualBoxissa näkyi vasemmassa reunassa uusi Virtuaalikone ja sitä painamalla saa näkyviin koneen tiedot.
TÄHÄN KUVA VM-tiedot
Yleisiä tietoja asioista, joita aiemmin asetin virtuaalikonetta luodessa.
Vasemmassa laidassa myös näkyy "Powered Off", mikä kertoo virtuaalikoneen nykytilan. Eli tällä hetkellä kone oli poissa päältä.
Virtuaalikoneen tietojen yläosassa kohta "Settings", josta päästiin eteenpäin tutkimaan asetuksia.


## Lähteet

Free Software Foundation, Inc(FSF). 2024-01-01. What is Free Software?. https://www.gnu.org/philosophy/free-sw.html

Karvinen, Tero. 2006-06-04. Raportin kirjoittaminen. https://terokarvinen.com/2006/raportin-kirjoittaminen-4/

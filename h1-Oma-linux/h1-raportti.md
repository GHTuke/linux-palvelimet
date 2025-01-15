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

Setting -> Storage kohdasta sai auki tallennuslaitteiden asetukset.
TÄHÄN KUVA VM-settings-storage
Valitsin Controller: IDE kohdasta "Empty" merkatun kohdan.
Oikeassa reunassa kohtaan "Optical Drive" IDE secondary device.
Pienestä levyn kuvasta painaessa aukesi valikko, josta pystyi valitsemaan Virtuaali levyasemalle aiemmin ladatun Debian live ISOn.
TÄHÄN KUVA VM-storage-valinta
Kun Debian Live ISO oli valittu alhaalta "Ok" tallensi ja palasi takaisin Virtuaalikoneen tietoihin.

Seuraavaksi tuplaklikkaamalla vasemmassa reunassa olevaa Virtuaalikonetta kone boottaa.
TÄHÄN KUVA VM-tiedot

Virtuaalikone käynnistyi ja aukesi valikko, josta voi valita Boot tavan.
TÄHÄN KUVA VM-bootmenu
Valitsin Live system (amd64).
Käynnistyminen alkoi ja hetken päästä oli Debianin työpöytä auki.
TÄHÄN KUVA VM-desktop

Vasemmasta ylälaidasta "Applications" kohdasta voi avata valmiina olevia ohjelmia.
TÄHÄN KUVA VM-applications
Tässä tapauksessa testain avaamalla "Web browser" ja Firefoxin auettua siirryin osoitteeseen terokarvinen.com.
TÄHÄN KUVA VM-firefoxTeroKarvinenCom

Siirrytään asentamaan Debian levyltä, jonka aiemmin lisäsin Virtuaaliseen levyasemaan.
TÄHÄN kuva VM-Desktop
Työpöydältä valitaan "Install Debian" tuplaklikkaamalla.

Aukeaa Debian installer, default kielenä "American English" ei tarvinnut vaihtaa.
TÄHÄN KUVA VM-debianInstaller
Eteenpäin valitsemalla "Next".

Seuraavksi valitaan sijainti tiedot kellonaikaa sekä numerointia varten.
TÄHÄN KUVA VM-debianLocation
Eteenpäin kohdasta "Next".

Aukeaa Näppäimistön asetusten valinta.
Defaulttina näppäinten asettelu "American english" muodossa.
Vaihdoin "Finnish" ja testasin alhaalla olevaan laatikkoon, että ääkköset toimivat.
TÄHÄN KUVA VM-debianKeyboard
Eteenpäin kohdasta "Next".

Aukeaa Partitions asetukset.
Ylhäältä voisi valita mitä kovalevyä käsitellään, mutta kyseisessä virtuaalikoneessa vain yksi, joten automaattisesti oikea levy kyseessä.
Tästä valitsin "Erase Disk", jotta vanhan käyttöjärjestelmän tiedot poistetaan kokonaan.
Muihin asetuksiin ei tarvinnut koskea. Defaultina "Boot loader location" oli Master boot record.
TÄHÄN KUVA VM-partitions
Eteenpäin kohdasta "Next".

Aukeaa Users asetukset.
Nimeksi asetin oman nimen.
Login nimi pienellä "tuke".
Koneen nimeksi "DebianTest".
Defaulttina automaatti kirjautuminen poissa päältä.
TÄHÄN KUVA VM-debianUser
Eteenpäin kohdasta "Next".

Aukeaa Summary ikkuna.
Tästä näkyi asennuksen tiedot.
TÄHÄN KUVA VM-kooste
"Install" oikealta alhaalta lähtee asentamaan käyttöjärjestelmää uudestaan.


## Lähteet

Free Software Foundation, Inc(FSF). 2024-01-01. What is Free Software?. https://www.gnu.org/philosophy/free-sw.html

Karvinen, Tero. 2006-06-04. Raportin kirjoittaminen. https://terokarvinen.com/2006/raportin-kirjoittaminen-4/

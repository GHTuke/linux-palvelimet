# Tehtävä h1 - Oma linux

Harjoituksessa ensin pieni tiivistelmä verkkoteksteistä Tero Karvisen Raportin kirjoittaminen sekä Free Software Foundationin määritelmä Free Softwaresta.
Pääosa harjoituksesta kuitenkin käsittelee Debianin asennusta virtuaalikoneelle noudattaen Tero Karvisen verkkotekstiä https://terokarvinen.com/2021/install-debian-on-virtualbox/.
Harjoituksen aikana ei noussut ongelmia Debiania asentaessa ja lopulta virtuaalikoneelle oli asennettu päivitetty Debianin versio toimivalla palomuurilla.

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

Free Software Foundation (FSF) (Free Software Foundation, Inc. 2024) määrittelemät 4 (0-3) vapautta ohjelmistoille.

  0) Vapaus käyttää ohjelmistoa miten haluaa, mihin vain käyttötapaukseen.\
         Tämä kattaa myös vapaan ohjelmiston käytön niin, että siitä muokataan kaupallinen versio.\
         Vapaan ohjelmiston pitää olla saatavilla ilman maksua tai muita käyttäjiä rajoittavia asetuksia.
     
  1) Vapaus opiskella miten ohjelmisto toimii, ja sen muokkaus jotta se toimii niin kuin itse haluat.\
         Vapaa ohjelmisto vaatii muutakin kuin avoimen lähdekoodin (Open source). Avoin lähdekoodi ei välttämättä vielä takaa oikeutta tehdä muokatuilla versioilla mitä haluaa.\
         Oikeus muokata ohjelmistoa ilman, että siitä tehdyistä henkilökohtaisista tai kaupallisista versioista tarvitsee ilmoittaa ohjelmiston luojalle.
     
  2) Vapaus jakaa kopioita toisille.\
         Oikeus jakaa alkuperäisestä ohjelmistosta kopiota kenelle tahansa missä tahansa.
     
  3) Vapaus jakaa kopiota omasta muokkaamastasi versiostasi.\
         Jos muokattujen versioiden jakaminen eteenpäin vaatisi, että ne eivät ole kaupallisia olisi se vastoin vapaan ohjelmiston perussääntöjä eli pitää pystyä jakamaan kaupallisia versiota.

Jotta nämä vapaudet olisivat todellisia pitää niiden olla pysyviä ja muuttumattomia tulevaisuudessakin, muuten ohjelmisto ei kata määritelmän vaatimuksia.

## a - Oman linuxin asennus virtuaalikoneelle
Harjoitus seuraa Tero Karvisen ohjeita: https://terokarvinen.com/2021/install-debian-on-virtualbox/
Harjoitus ajankohta: 2025-01-15

Ympäristö: 
- Windows 11 Home v. 23H2
- AMD Ryzen 7 6800H
- RAM 16 Gt
- VirtualBox v. 7.1.4

### Debian live ISO

Aluksi latasin viimeisimmän vakaan (Stable) Debianin live imagen sivulta https://www.debian.org/CD/live/.

Versio 12.9.0 Amd64 Xfce

### VirtualBox virtuaalikoneen luonti
Local Time: 12.40

VirtualBoxissa valitsin ylävalikosta Machine -> New

Aunneeseen ikkunaan "Virtual machine Name and Operating System" kirjasin nimen, valitsin tallennuskansion sekä valitsin tyypiksi Linux ja Debian 64-bit.\
Tästä eteenpäin "Next".

![VM-luonti](https://github.com/user-attachments/assets/7c834e7f-8b04-4442-885b-edcbc9649b23)

Aukesi "Hardware" ikkuna johon kirjasin Base memory kohtaan 4000 Mb, kertoo siis RAMin määrän. Tähän olisi voinut kirjata 4096 Mb, meni itseltä hieman liian automaatiolla.

![VM-hardware](https://github.com/user-attachments/assets/4a01ec1a-8683-442c-bdd8-97fb20982396)

Tästä taas eteenpäin "Next".

Seuraavaksi siirryttiin "Virtual Hard disk" ikkunaan.

![VM-harddisk](https://github.com/user-attachments/assets/845fd691-af87-4eff-bda7-ad0c4d8034bd)

Valitsin Create a Virtual Hard disk now ja asetin levykkeen kooksi 40 Gb.\
Muut asetukset Defaultilla.\
Eteenpäin taas "Next".

Tässä vaiheessa tuli tietojen koonti ikkuna, josta VM image luotiin painamalla "Finish"

Nyt VirtualBoxissa näkyi vasemmassa reunassa uusi Virtuaalikone ja sitä painamalla saa näkyviin koneen tiedot.

![VM-tiedot](https://github.com/user-attachments/assets/ee2dbb88-99ed-4a03-9ced-659204d8b57f)

Yleisiä tietoja asioista, joita aiemmin asetin virtuaalikonetta luodessa.\
Vasemmassa laidassa myös näkyy "Powered Off", mikä kertoo virtuaalikoneen nykytilan. Eli tällä hetkellä kone oli poissa päältä.\
Virtuaalikoneen tietojen yläosassa kohta "Settings", josta päästiin eteenpäin tutkimaan asetuksia.

Setting -> Storage kohdasta sai auki tallennuslaitteiden asetukset.

![VM-settings-storage](https://github.com/user-attachments/assets/f4e037f1-e651-4327-a1e3-9f916e8edeba)

Valitsin Controller: IDE kohdasta "Empty" merkatun kohdan.\
Oikeassa reunassa kohtaan "Optical Drive" IDE secondary device.\
Pienestä levyn kuvasta painaessa aukesi valikko, josta pystyi valitsemaan Virtuaali levyasemalle aiemmin ladatun Debian live ISOn.

![VM-storage-valinta](https://github.com/user-attachments/assets/8bf1a0bf-7089-4987-9b41-57c5742309fc)

Kun Debian Live ISO oli valittu alhaalta "Ok" tallensi ja palasi takaisin Virtuaalikoneen tietoihin.

Seuraavaksi tuplaklikkaamalla vasemmassa reunassa olevaa Virtuaalikonetta kone boottaa.

![VM-tiedot](https://github.com/user-attachments/assets/ef1ae516-88d4-457d-826e-06f48403e640)


### Uuden virtuaalikoneen testaus
Local Time: 13.15

Virtuaalikone käynnistyi ja aukesi valikko, josta voi valita Boot tavan.

![VM-bootmenu](https://github.com/user-attachments/assets/6851da0c-c4d2-4b98-a0ab-ae7edc834e1a)

Valitsin Live system (amd64).\
Käynnistyminen alkoi ja hetken päästä oli Debianin työpöytä auki.

![VM-desktop](https://github.com/user-attachments/assets/a7c372ef-1b06-4072-9e71-8f70aae23fba)


Vasemmasta ylälaidasta "Applications" kohdasta voi avata valmiina olevia ohjelmia.

![VM-applications](https://github.com/user-attachments/assets/888a433b-fa9e-42da-aafc-44113732b791)

Tässä tapauksessa testain avaamalla "Web browser" ja Firefoxin auettua siirryin osoitteeseen terokarvinen.com.

![VM-firefoxTeroKarvinenCom](https://github.com/user-attachments/assets/05e27fbc-3e53-447e-9421-7b0ab1376e76)


### Debianin uudelleenasennus
Local Time: 13.30

Siirrytään asentamaan Debian levyltä, jonka aiemmin lisäsin Virtuaaliseen levyasemaan.

![VM-desktop](https://github.com/user-attachments/assets/74c916e0-2a01-4034-9cbf-86a8f76e8af5)

Työpöydältä valitaan "Install Debian" tuplaklikkaamalla.

Aukeaa Debian installer, default kielenä "American English" ei tarvinnut vaihtaa.

![VM-debianInstaller](https://github.com/user-attachments/assets/de965541-7107-41a8-8720-93c3ce12cfee)

Eteenpäin valitsemalla "Next".

Seuraavksi valitaan sijainti tiedot kellonaikaa sekä numerointia varten.

![VM-debianLocation](https://github.com/user-attachments/assets/9aa719da-924c-4009-b8c5-7a885dec9a7a)

Eteenpäin kohdasta "Next".

Aukeaa Näppäimistön asetusten valinta.\
Defaulttina näppäinten asettelu "American english" muodossa.\
Vaihdoin "Finnish" ja testasin alhaalla olevaan laatikkoon, että ääkköset toimivat.

![VM-debianKeyboard](https://github.com/user-attachments/assets/7d701974-b6ec-4d31-8fda-90d95ef79bd4)

Eteenpäin kohdasta "Next".

Aukeaa Partitions asetukset.\
Ylhäältä voisi valita mitä kovalevyä käsitellään, mutta kyseisessä virtuaalikoneessa vain yksi, joten automaattisesti oikea levy kyseessä.\
Tästä valitsin "Erase Disk", jotta vanhan käyttöjärjestelmän tiedot poistetaan kokonaan.\
Muihin asetuksiin ei tarvinnut koskea. Defaultina "Boot loader location" oli Master boot record.

![VM-partitions](https://github.com/user-attachments/assets/2365ccf4-ca97-45ed-8a72-0eedf9d78a44)

Eteenpäin kohdasta "Next".

Aukeaa Users asetukset.\
Nimeksi asetin oman nimen.\
Login nimi pienellä "tuke".\
Koneen nimeksi "DebianTest".\
Defaulttina automaatti kirjautuminen poissa päältä.

![VM-debianUsers](https://github.com/user-attachments/assets/08634c91-f12c-4b0a-9df7-9478ca379831)

Eteenpäin kohdasta "Next".

Aukeaa Summary ikkuna.\
Tästä näkyi asennuksen tiedot.\

"Install" oikealta alhaalta lähtee asentamaan käyttöjärjestelmää uudestaan.

![VM-debiankooste](https://github.com/user-attachments/assets/9379c947-0fa4-4ed6-b762-4e75d4ebf5ee)

Asennuksen jälkeen ikkuna, josta näkyi "All Done" viesti.\
Defaulttina "Restart Now"

![VM-debianInstallRdy](https://github.com/user-attachments/assets/738dda89-f644-4a45-a1ab-66a5d6bbe9e2)

Eteenpäin "Done" oikeasta alareunasta

### Debianin ohjelmistojen päivittäminen ja palomuuri
Local Time: 13.50

Rebootin jälkeen aukeaa "Login" ikkuna, aiemmin asetetuilla käyttäjänimellä ja salasanalla pääsee sisälle.

![VM-debianLogin](https://github.com/user-attachments/assets/acee0411-5de4-4e80-a3f3-74090afe6e03)


Tämän jälkeen testasin avaamalla "Applications" valikon kautta taas Web browserin. Firefox aukesi ja "Google.com" aukesi normaalisti.

Työpöydän alaosasta "Terminal" auki yhdellä klikkauksella.\
Päivitin application managerin listaa mahdollisista ohjelmista.\
"sudo apt-get update" päivitti listan, muutaman kirjoitusvirheellisen salasana syötön jälkeen.

![VM-debianTerminal](https://github.com/user-attachments/assets/08ed4457-8825-4d3a-9c5c-a3d1e6026b2d)


Seuraavaksi päivitin Debianin ohjelmistot.\
"sudo apt-get -y dist-upgrade" aloitti suoraan päivityksen.

![VM-debianDistUpgrage](https://github.com/user-attachments/assets/f8799b5d-c808-4828-a748-7fd6f9e643dc)

Hetken päivittelyn jälkeen kaikki oli päivitetty.\
Päivitysten jälkeen seuravaat komennot asensivat ja käynnistivät palomuurin.\
"sudo apt-get -y install ufw"\
"sudo ufw enable"\

Terminal ilmoittaa, että pitää käynnistää kone uudestaan.

![VM-debianFirewall](https://github.com/user-attachments/assets/06485085-d943-45bc-8ea5-09ea3ce7e4e0)

Työpöydältä "Applications" -> "Log out" -> "Restart"

![VM-debianRestart](https://github.com/user-attachments/assets/4893bc40-8ec0-41c4-911e-c0ddefa2f700)

Virtuaalikone käynnistyi uudelleen ja kun login tiedot oli annettu kaikki toimi normaalisti.

![VM-debianReddit](https://github.com/user-attachments/assets/e2b89af0-83fe-4b42-8f81-816150043a4c)


### Resoluution korjaus

![VM-guestAvaus](https://github.com/user-attachments/assets/155153e3-0f6d-422c-8ad7-aa584c839038)

Avataan kohdasta "Application" -> "File Manager" ja valitaan reunavalikosta VBox, jotta voidaan tarkistaaa polku ylhäältä osoitepalkista.\
"cd /media/tuke/VBox*"\
"ls"\
Komennot avasivat GuestAdditionsia varten Terminaaliin kansiosta löytyvät ohjelmat.\
"sudo bash VBoxLinuxAdditions.run" ajaa VBox asennuksen.

![VM-guestVirhe](https://github.com/user-attachments/assets/48c92e28-00fe-4bd2-849c-f9ade994fc8d)

Ajon jälkeen tuli virheilmoitus, joka ohjeistaa käynnistämään uudelleen.

![VM-displaySettings](https://github.com/user-attachments/assets/a7cc8b77-f75c-438c-8867-5769e97a7122)

Uudelleen käynnistyksen jälkeen "Applications" valikosta "Settings" -> "Display" ja pystyi asettamaan uuden resoluution.\
Aukeaa ikkuna, josta voi valita eri resoluutioita.

![VM-Resolution](https://github.com/user-attachments/assets/326b63fc-66ea-44da-911f-47109e789b51)

Testasin maksimi asetuksella 2560x1600. Aiheutti vain mustan näytön, mutta valintaa en hyväksynyt aikamääreen sisällä joten muutos otettiin takaisin.\
1920x1080 toimi itsellä hyvin.

Tässä valmiin koneen työpöydästä vielä kuva.

![VM-debianValmis](https://github.com/user-attachments/assets/a059a4ec-840a-4f1c-86b8-a013bab6f29d)

Harmillista kyllä kuvassa näkyvä DoomPDF ei toimi kuin Chromium pohjaisilla selaimilla. Joten rikkinäinen PDF, mutta muuten kaikki toimii.

## Lähteet

Free Software Foundation, Inc(FSF). 2024-01-01. What is Free Software?. https://www.gnu.org/philosophy/free-sw.html

Karvinen, Tero. 2006-06-04. Raportin kirjoittaminen. https://terokarvinen.com/2006/raportin-kirjoittaminen-4/

Karvinen, Tero. 2024-09-26. Install Debian on VirtualBox. https://terokarvinen.com/2021/install-debian-on-virtualbox/

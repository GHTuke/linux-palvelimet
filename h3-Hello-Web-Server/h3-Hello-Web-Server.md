# H3-Hello Web Server

Tekijä: Tuukka Huovilainen

Tässä työkirjassa suoritetaan Linux palvelimet -kurssin tehtävää "Hello web server", jossa asennetaan Apachen webbipalvelin ja otetaan se käyttöön Linuxissa. Käyttöönoton jälkeen muokataan hieman etusivua ja käydään läpi muutamia logeja matkanvarrelta. Tehtävän pohjana on käytetty Tero Karvisen webbikirjoitusta https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/. Alkuun pieni kooste kyseisestä webbikirjoituksesta sekä Apache Software Foundationin kirjoituksesta https://httpd.apache.org/docs/2.4/vhosts/name-based.html.

## x - Tiivistelmä

Apache Software Foundation - Name Based Virtual Host Support

  * Nimipohjaisilla palvelimilla voidaan säästää rajallista IP-osoitteiden määrää ohjaamalla usealla nimellä olevia sivuja samoihin IP-osoitteisiin
  * Tietyt laitteet vaativat IP pohjaisia palvelimia, muuten pitäisi käyttää nimipohjaisia palvelimia

Tero Karvinen - Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address

  * Muokkaamalla sivun conf-tiedostoa voidaan palvelimelle asettaa asetuksia
  * $ sudo systemctl restart apache2 on hyödyllinen komento syöttää tehdessään muutoksia palvelimen asetuksiin

## Webbipalvelin

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


### a - Webbipalvelin


### b - Logit


### c - Etusivu


### e - Validi HTML5


### f - curl


### o - Kaksi eri sivua


## Lähteet

Karvinen, T. 10-04-2018. Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address. https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/.

The Apache Software Foundation. s.a. https://httpd.apache.org/docs/2.4/vhosts/name-based.html.

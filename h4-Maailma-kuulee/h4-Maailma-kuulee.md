# H4 - Maailma kuulee
Tekijä: Tuukka Huovilainen

* Pohjana Tero Karvinen 2025: Linux Palvelimet kurssi, http://terokarvinen.com

Tässä työkirjassa suoritetaan Linux Palvelimet -kurssin tehtävää Maailma kuulee, jossa vuokrataan virtuaalipalvelin ja asennetaan sille Apache webpalvelin.
Aikasempaa tehtävää h3 mukaillen myös muokataan webpalvelimen näyttämää sivua. Virtuaalipalvelimen tarjoana käytetään upCloud tarjoajaa.
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


## Alkutoimet virtuaalipalvelimella


## Webpalvelimen asennus (Apache)


## Vapaaehtoinen Name Based Virtual Host


## Lähteet


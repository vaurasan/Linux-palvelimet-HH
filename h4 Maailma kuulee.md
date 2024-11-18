# h4 Maailma kuulee

#### Oma Host kokoonpanoni:

| Komponentti | Kuvaus | Lisätiedot |
| :---        |    :----:   |          ---: |
| Emolevy | MSI B550-A PRO | ATX, AM4 |
| Prosessori   | AMD Ryzen 9 5900X | 12-Core 3.70 GHz |
| RAM   | G.Skill  Ripjaws V |  32GB (4x8GB) DDR4 3600MHz, CL 16, 1.3  |
| Näytönohjain   | Sapphire PULSE AMD Radeon RX 7900 GRE        | 16GB     |
| Kovalevy   | Kingston 1TB        | A2000 NVMe PCIe SSD M.2      |
| Kovalevy   | Crucial 512GB        | MX100 SSD     |
| Kovalevy   | Crucial 256GB        | MX100 SSD     |
| Virtalähde   | Asus 750W TUF Gaming Gold        | ATX 80 Plus      |
| Kotelo   | Phanteks Enthoo Pro       |  Full Tower      |

Käyttöjärjestelmä: Windows 11 Pro 23H2

## x) Lue ja tiivistä. Tiivistelmäksi riittää muutama ranskalainen viiva per artikkeli (ei muuten riittänyt). (Tässä alakohdassa ei tarvitse tehdä testejä tietokoneella)

[Susanna Lehto 2022: Teoriasta käytäntöön pilvipalvelimen avulla (h4) (opiskelijan esimerkkiraportti), kohdat](https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/)

Susanna Lehdon vuonna 2022 tekemä tehtävä:<br>
a) Pilvipalvelimen vuokraus ja asennus
- GitHub Education paketilla voi saada ilmaiseksi palvelimen ja .me-päätteisen domainin väliaikaisesti käyttöön
- DigitalOceaniin täytyy syöttää luottokorttitiedot henkilöllisyyden varmistamiseen 
- Pilvipalvelimen vuokraus alkaa valitsemalla ``Droplets -> Create Droplet``
- Seuraavaksi valitaan käyttöjärjestelmä, tässä tapauksessa mahdollisimman uusi ``Debian``
- Valitaan prosessori, tallennustila, sekä RAM
- Datakeskukseksi valikoitui Amsterdam, koska etäisyys Helsinkiin on 1508km (itse olisin valinnut mieluummin Frankfurtin, jos hinta on sama, sillä merikaapeli kulkee suoraan Helsingistä Saksaan, tämän opin eilen ICT-infra johdantokurssilla. **Cretits:** Pekka Korpi-Tassi)
- Autentikointimenetelmäksi täytyy valita ``salasana`` ellei ``SSH-avaimen`` käyttö ole tuttua. Salasanan tulee olla vahva.
- Mitään ylimääräisiä palveluja ei kannata ottaa, lopuksi annetaan virtuaalikoneelle julkinen nimi, tämän jälkeen ``Create Droplet`` ja odotuksen jälkeen DigitalOcean ilmoittaa pilvikoneen ``IP-osoitteen``

d) Palvelin suojaan palomuurilla

- Aluksi otetaan SSH-yhteys virtuaalipalvelimeen, tässä tapauksessa IP-osoitteessa: 188.166.4.6, käyttäen komentoa: ``$ ssh root@188.166.4.6`` Tässä vaiheessa vahvistuksen jälkeen tulee antaa DigitalOceanissa antama salasana
- Käyttäjä suoritti päivityksen komennolla ``$ sudo apt-get update``, tämän jälkeen palomuurin asennus komennolla<br> ``$ sudo apt-get install ufw``, reikä palomuuriin komennolla ``$ sudo ufw allow 22/tpc``, palomuuri päälle komennolla ``$ sudo ufw enable``

e) Kotisivut palvelimelle

- Luodaan käyttäjä komennolla ``$ sudo adduser suska``, käyttäjälle annetaan vahva salasana
- Käyttäjästä tehdään pääkäyttäjä komennolla ``$ sudo adduser suska sudo``
- Otettiin SSH-yhteys palvelimeen uudella käyttäjällä ``$ ssh suska@188.166.4.6``, jonka jälkeen taas päivittiin ``$ sudo apt-get update``
- Lukitaan juuri komennolla ``$ sudo usermod –lock root``, jonka jälkeen ``$ sudo apt-get update``, ``$ sudo apt-get upgrade``, ja ``$ sudo apt-get dist-upgrade``
- Asennetaan Apache komennolla ``$ sudo apt-get install apache2``, asennuksen jälkeen ``$ sudo systemctl status apache2``, joka näyttää mm. kuinka kauan apache2 on ollut päällä. Tehtiin toinen reikä palomuuriin komennolla ``$ sudo ufw allow 80/tcp``
- Korvataan Apachen testisivu komennolla ``$ echo Hello world! |sudo tee /var/www/html/index.html``, näin kotisivulla lukee nyt ``Hello world!``
- Terminaalissa otettiin userdir-moduuli käyttöön komennolla ``$ sudo a2enmod userdir``, jonka jälkeen demonin potkaisu komennolla ``$ sudo service apache2 restart`` (**HUOM.** nykyään ``sudo systemctl restart apache2``)
- Suska-käyttäjälle luotiin public_html-kansio, jonka jälkeen Firefoxissa näkyi julkinen hakemisto osoitteessa ``susannalehto.me/~suska``, ennen tätä avattiin SSH-yhteys komennolla ``$ sudo systemctl start ssh``, jonka jälkeen asennettiin micro tekstieditori ``$ sudo apt-get install micro``, public_html-kansiossa luotiin tekstitiedosto ``$ micro index.html``, tänne tehtiin nettisivun runko ja tallennus ``ctrl+s``
- Nettisivuja kokeiltiin vielä Windows-tietokoneen Chrome selaimella ja todettiin toimivaksi

f) Palvelimen ohjelmien päivitys

- Päivitetään kaikki palvelimen ohjelmat
- ``$ sudo apt-get update``, ``$ sudo apt-get upgrade``, ``$ sudo apt-get dist-upgrade``

[Karvinen 2012: First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS](https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/)

Tero Karvisen ohje:
- Luodaan DigitalOceaniin uusi virtuaalipalvelin: ``Luo uusi käyttäjä``, ``Lisää luottokorttitedot ja promo-koodi``, ``Valitse datakeskus, joka on lähellä, mieluiten Euroopassa``
- Ensimmäinen kirjautuminen *root*-käyttäjänä (ainut kerta kun kirjaudutaan kyseisenä käyttäjänä) ``$ ssh root@10.0.0.1``, Anna hyvä salasana kysyttäessä, älä koskaan anna huonoa salasanaa, edes hetkeksi
- Tehdään reikä SSH:hon ennen palomuurin käyttöönottoa ``$ sudo ufw allow 22/tcp``, palomuuri päälle ``$ sudo ufw enable``
- Luodaan järjestelmänvalvojakäyttäjä ``$ sudo adduser tero``, ``$ sudo adduser tero sudo``, ``$ sudo adduser tero adm``, ``$ sudo adduser tero admin``. Kokeillaan käyttäjän toiminta uudella terminaalilla ``$ ssh tero@tero.example.com``
- Suljetaan *root*-käyttäjä ``$ sudo usermod --lock root`` <-- tämä "usermod --lock" ainoastaan lukitsee salasanakirjautumisen, ei kaikkia käyttäjän käyttömahdollisuuksia
- Kielletään *root*-käyttäjä ``$ sudoedit /etc/ssh/sshd_config`` <-- tänne kohtaan: "PermitRootLogin" annetaan arvo: "no", jonka jälkeen ``$ sudo service ssh restart``
- Päivitetään ohjelmistopaketit, jotta vältytään tietoturvauhilta ``$ sudo apt-get update``, ja ``$ sudo apt-get upgrade`` (**HUOM.** käsittääkseni tässä alkuvaiheessa vielä hyvä näiden lisäksi ``$ sudo apt-get dist-upgrade``)
- Voidaan aloittaa palvelimen käyttö esimerkiksi Apache:lla. Muista tehdä reikä palomuuriin ``sudo ufw allow 80/tcp``
- Seuraavaksi tarvitaan vielä nimipalvelu. NameCheap ja Gandi ovat hyviä vaihdoehtoja, jos käytät GitHub Education-pakettia, voit saada ilmaiseksi .me-päätteisen nimen.
- Katso [NameCheap ohje](https://www.namecheap.com/support/knowledgebase/article.aspx/319/2237/how-can-i-set-up-an-a-address-record-for-my-domain/)

## a) Vuokraa oma virtuaalipalvelin haluamaltasi palveluntarjoajalta. (Vaihtoehtona voit käyttää ilmaista kokeilujaksoa, GitHub Education krediittejä; tai jos mikään muu ei onnistu, voit kokeilla ilmaiseksi vagrant:ia paikallisesti. Suosittelen kuitenkin harjoittelemaan oikeilla, tuotantoon kelpaavilla julkisilla palveluilla).

*13.9.2024 klo 14:00*

Selailin hetken suomen kielisiä palvelinvuokraajia ja totesin, että turhan kallista näin alkuun. Joissain jopa olisi pitänyt maksaa 24kk maksu suoraan, tosin taisi olla noin 4€ kuukaudessa, mutta silti.

Päätin siis ottaa ``GitHub Educationin`` kautta [DigitalOceanin](https://www.digitalocean.com/) tarjouksen, jolla saa 200$ käyttörahaa vuoden ajaksi palvelimien vuokraukseen.

![DigitalOceanOffer](https://github.com/user-attachments/assets/eb4531c6-da0c-4b49-8e23-955bc23ec95b)

Syötin luottokorttitiedot henkilöllisyyden varmistamiseksi. Aloitan palvelimen vuokraamisen painamalla vihreällä pohjalla olevaa "Create" painiketta, jonka dropdown-menusta valitsen "Droplets *Create cloud servers*"

![DOMenu](https://github.com/user-attachments/assets/c7bf1d6e-8ba5-4580-897e-3f05e867a7e9)

Valitsen palvelimeksi näistä vaihtoehdoista Frankfurt, johtuen siitä, että merikaapeli kulkee suoraan Helsingistä Saksaan, Amsterdam olisi hyvänä kakkosvaihtoehtona. Halutaan, että palvelin on mahdollisimman lähellä. Tässä vaiheessa palvelimen hinta näyttää olevan 32$/kk, mutta ei huolehdita siitä vielä.

![Frankfurt](https://github.com/user-attachments/assets/c7ec728d-dfdd-4bba-8285-3c9f030a7206)

Valitsen Debian V.12 käyttöjärjestelmän ja Basic droplet type - shared CPU

![debianCPU](https://github.com/user-attachments/assets/e52d14be-081a-4236-8f22-cd4b43457927)

CPU optionsiin valitsen ``Regular, 512MB/1CPU, 10GB SSD, 500GB transfer``, näin saadaan hintaa tiputettua 4$:iin

![fourdollars](https://github.com/user-attachments/assets/05392ac5-b0e2-4f9c-954a-adaea4aeb2f4)

Valitsen authentication methodiksi ``Password``, sillä en ole vielä käyttänyt SSH-avainta. Tässä kohtaa on hyvä muistaa Tero Karvisen ohje vahvasta salasanasta, käytetään aina vahvaa salasanaa.

![PassWord](https://github.com/user-attachments/assets/e5790fd3-786b-4f87-85e7-f21e5fcfdbdb)

Tämän jälkeen en valitse mitään ylimääräisiä palveluja. ``1 Droplet`` riittää, ``Hostname`` kohtaan laitan jotain neutraalia, koska tämä tulee olemaan julkinen nimi. Tageta ei tarvita. Näiden jälkeen painetaan sinistä painiketta: ``Create Droplet``

![CreateDrop](https://github.com/user-attachments/assets/bf549f6c-7337-4408-9530-3301ca44ebf0)

*klo 14:27*

Noin minuutti meni, kun Droplet muodostui, nyt sain palvelimen IP-osoitteen

![IPDroplet](https://github.com/user-attachments/assets/9f172d45-f1f3-4c24-a42e-a87a8fc8e2c6)

*valmis klo 14:28, aikaa kului 28min*

## b) Tee alkutoimet omalla virtuaalipalvelimellasi: tulimuuri päälle, root-tunnus kiinni, ohjelmien päivitys.

*klo 14:36*

Aloitetaan alkutoimet, ensin otetaan SSH-yhteys juuri vuokrattuun palvelimeen ``ssh root@167.71.54.154``, vastataan "yes" kysymykseen, annetaan salasana, joka luotiin aiemmin. Tämä on ainoa kerta, kun kirjaudumme *root*-käyttäjällä

![alku](https://github.com/user-attachments/assets/029d2d50-37d3-4f88-870a-ac2f839bc742)

Jostain syystä salasana ei toimi. Yritin vaikka mitä variaatioita, mutta ei päässyt sisään, ainoa vaihtoehdo oli tuhota Droplet ja tehdä uusi samoilla tiedoilla. [3.10.2024, näyttäisi siltä, että SSH-kirjautuminen oli käytössä, vaikka valitsin salasanakirjautumisen]

![UusiD](https://github.com/user-attachments/assets/628c0675-e49d-4ca6-b856-49fd4e1685ed)

*klo 14:51 uusi yritys*

Nyt onnistui, sisällä ollaan

![UusiSisalla](https://github.com/user-attachments/assets/a106efb0-80de-4619-a8b7-a542d262f749)

Nyt voidaan alkaa tekemään ensimmäisiä toimintoja palvelimelle.

- Asennetaan palomuuri ``sudo apt-get install ufw``, tehdään reikä palomuuriin ``sudo ufw allow 22/tcp``, laitetaan palomuuri päälle ``sudo ufw enable``

![ufwinst](https://github.com/user-attachments/assets/5d7560b2-0df2-47ee-9d2a-47546023a0d2)

- Luodaan samalla vielä ihmiskäyttäjä, ennen uudelleenkäynnistystä ``sudo adduser santeri``, taas annetaan vahva salasana, "Full Name":een laitoin vaurasan ja jätin muut tiedot tyhjiksi

![kayttajaLuonti](https://github.com/user-attachments/assets/cacee0ef-667d-455c-b32b-894238d84c74)

- Annetaan käyttäjälle oikeuksia ``sudo adduser santeri sudo``, ``sudo adduser santeri adm``, ``sudo adduser santeri admin``

![imagesudo](https://github.com/user-attachments/assets/0095bd07-480c-496e-a76a-7f069f1afeb5)

![imageadm](https://github.com/user-attachments/assets/3a3e7c9e-c466-4a0a-a670-3fd5318513f3)

![imageadmin](https://github.com/user-attachments/assets/cde76c43-3a01-436e-ba22-2a357ad7defe)

Näistä viimeisin ei toimi, ei ymmärtääkseni tarvitse toimiakaan.

- Bootataan virtuaalikone komennolla ``reboot``

![imagereboot](https://github.com/user-attachments/assets/c605ed5e-3411-4a67-b8ce-b13b2d200c9c)

- Kirjauduin nyt luodulla käyttäjällä sisälle ja ajoin päivityksen ``sudo apt-get update`` kokeillakseni, että oikeudet toimivat

![imagesudoUserjoo](https://github.com/user-attachments/assets/d4a4fed9-6c97-4f00-805a-8e6f3956c306)

- Kuka olen ja missä olen?

![imagewhoamig](https://github.com/user-attachments/assets/70315db0-7c70-4aab-bf1d-7fec5cdbd3d5)


- Nyt voidaan sulkea *root*-käyttäjän SSH-yhteys. ``sudo usermod --lock root`` komennolla saadaan root-salasana pois. Nyt mennään ``sudoedit /etc/ssh/sshd_config``, ja muutetaan ``PermitRootLogin no`` "ctrl+s" tallennetaan tiedosto ja "ctrl+x" exit editor. Näin saadaan root lukittua. Tämän jälkeen vielä ``sudo service ssh restart``

![imagerootloginno](https://github.com/user-attachments/assets/2e159b6d-8efe-4e82-990c-ce4a6b6e2849)

- Nyt päivitellään ohjelmat komennoilla ``sudo apt-get update``, ``sudo apt-get upgrade``, ja ``sudo apt-get dist-upgrade``. Muutaman minuutin kuluttua kaikki ohjelmat ovat päivitetty.

*klo 15:26, yllättäen aikaa kului 50min kaikkine säätöineen*

## c) Asenna weppipalvelin omalle virtuaalipalvelimellesi. Korvaa testisivu. Kokeile, että se näkyy julkisesti. Kokeile myös eri koneelta, esim kännykältä.

*klo 16:59*

- Asennetaan Apache2 komennolla ``sudo apt-get install apache2``, varmuudeksi päivitetään kaikki ``sudo apt-get update``, tarkistan apachen tilan ``sudo systemctl status apache2``

![StatusApache](https://github.com/user-attachments/assets/07aec914-3ab7-4c49-95af-f56162caef7b)

Kello on näemmä väärässä ajassa, joten yritän vaihtaa sen vastaamaan nykyistä Suomen kesäaikaa UTC+3. Löysin ohjeet ensin ChatGPT:n avulla, komento näytti niin hämärältä, että oli pakko etsiä toisesta lähteestä. Debian wiki näytti samaa komentoa, joten uskalsin käyttää ``sudo dpkg-reconfigure tzdata``, sieltä valitsin "Europe", ja "Helsinki"

![Europekello](https://github.com/user-attachments/assets/fe2b9441-2730-4fde-98db-a24d04c91a9e)

![Helsinki](https://github.com/user-attachments/assets/35f9bd70-888f-4917-8a22-21001c2ebe7e)

Aika ei näyttänyt muuttuvan, joten käynnistin palvelimen uusiksi ``sudo reboot``

![reboot](https://github.com/user-attachments/assets/3fa69f84-0ed6-465e-9e2c-a56e547e2c16)

![KelloOikein](https://github.com/user-attachments/assets/147e31ea-95b5-482a-ac8a-267ccd426574)

Nyt kellonaika näyttää oikealta, voidaan jatkaa työskentelyä. Teen reiän palomuuriin ``sudo ufw allow 80/tcp``

![ufw80](https://github.com/user-attachments/assets/ce5ddba0-42e8-488d-bfbd-f2823d6b74e5)

Päällekirjoitetaan apachen testisivu ``echo Testisivu|sudo tee /var/www/html/index.html`` ja potkaistaan demonia ``sudo systemctl restart apache2``<br>
Menin Windowsin Chrome selaimella osoitteeseen ``http://165.232.119.164/``, siellä näkyy nyt kyseinen Testisivu

![Testisivuyks](https://github.com/user-attachments/assets/815561ac-ff51-49a0-88b2-f773ba3d7936)

Tehdään nyt suoraan hieman järkevämmäksi tämä muuttamalla tuo testisivu uuteen muotoon ``sudo micro /var/www/html/index.html`` ja päällekirjoitetaan "Testisivu" teksti

![HtmlJuttu](https://github.com/user-attachments/assets/2e652cef-5a65-4856-a400-43661d1b0979)

Potkaistaan vielä demonia ``sudo systemctl restart apache2``, Chromessa näyttää tältä

![ChromeKaks](https://github.com/user-attachments/assets/f6b2702e-8f1b-4e1f-8a73-96f1724eb66e)

Ja kännykällä tältä

![KannyKuva](https://github.com/user-attachments/assets/82be1324-c9b3-456c-a44b-0f1782938f6d)

*valmis klo 17:46, aikaa kului 47min*<br>
*14.9.2024 klo 14:17 lisätty rivinvaihto tekstin alkuun*

## Lähteet

Debian. wiki. https://wiki.debian.org/DateTime<br>
Debian. wiki. https://wiki.debian.org/TimeZoneChanges#Commit_Change<br>
DigitalOcean. https://www.digitalocean.com/<br>
GitHub Education. https://education.github.com/learner/learn<br>
Karvinen, T. h4 Maailma kuulee. https://terokarvinen.com/linux-palvelimet/#h4-maailma-kuulee<br>
Lehto, S. 2022. Teoriasta Käytäntöön Pilvipalvelimen avulla h4. https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/

---

Tätä dokumenttia saa kopioida ja muokata GNU General Public License (versio 2 tai uudempi) mukaisesti. http://www.gnu.org/licenses/gpl.html<br>
Pohjana Tero Karvinen 2012: Linux kurssi, http://terokarvinen.com<br><br>
Kirjoittanut <em>Santeri Vauramo</em>, 2024

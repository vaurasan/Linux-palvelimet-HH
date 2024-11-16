# h5 Nimekäs

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

## aa-) Tämä ei kuulu tehtävänantoon 

*20.9.2024 klo 16:55*

#### Edellisenä läksynä vuokrasin DigitalOceanista palvelimen, joka sijaitsee Saksassa. Nyt kun viime oppitunnilla käytiin läpi SSH-avaimella kirjautuminen, vuokraan palvelimen UpCloudista, koska haluan kotimaisen palvelimen.

Aluksi käyn DigitalOceanissa poistamassa aiemmin vuokraamani "Dropletin"

![digitalDroplet](https://github.com/user-attachments/assets/461941dc-b62e-4858-a40e-b6fd76662dbf)

Nyt, kun vanha palvelin on tuhottu, on aika käydä luomassa SSH-avainpari virtuaalikoneelleni luennolla tekemieni muistiinpanojen avulla:

![sshohje](https://github.com/user-attachments/assets/45a43c56-d71e-4a39-8db5-55fd8bb6004f)

Luonnin jälkeen käyn kopioimassa julkisen avaimen UpCloud-palvelimen luontia varten.

![historia](https://github.com/user-attachments/assets/fa90b43d-ea7d-44d7-b740-1fd5adfc0b77)

UpCloud haluaa aluksi varmistaa että en ole robotti:

![captcha](https://github.com/user-attachments/assets/88e77d01-bf39-493c-ab9d-33ce5fcf458c)

Aloitetaan palvelimen vuokraaminen: ``+Deploy a new server``, valitsen sijainniksi Helsinki, halvin mahdollinen 3€/kk maksava palvelin riittää mainiosti, sillä se on parempi kuin kalliimpi halvin palvelin DigitalOceanissa.

![halvin](https://github.com/user-attachments/assets/aa565685-5175-4d7c-9c4e-498f260b6ac0)

Luonnollisesti valitaan Debian Linux käyttöjärjestelmäksi. Jätetään varmuuskopiot ja muut ylimääräiset palvelut pois. Lisätessä SSH-avainta ilmeni pieni ongelma, en ollut tajunnut, että koko litania tulee kopioida, eikä ainoastaan hämärän näköinen kirjoitus.

![sshokkey](https://github.com/user-attachments/assets/a7cd5cc2-98d3-4ebe-b684-6ad077e7641b)

Pienen perus-säädön jälkeen sain palvelimen toimintaan, nyt täytyy tehdä pienimuotoinen sijoitus UpCloudille, jotta palvelimeni pysyy hengissä.

![upCloudi](https://github.com/user-attachments/assets/7fa3fc10-3dae-4084-af82-81213623d342)

Nyt laitan palvelimen tulille ja samat alkusäädöt, kuin aiemmassa läksyssä. Mikäli ongelmia ei ilmene, en tähän enää vaihe vaiheelta kirjaa kaikkea.

*klo 17:25, aikaa kului 30min*

*root*-käyttäjänä pääsin sisälle palvelimeen luomaan käyttäjätunnuksen ja antamaan sille tarvittavat oikeudet, mutta yhdistämisessä on ongelmia. Täytyy lukea [täältä](https://upcloud.com/resources/tutorials/use-ssh-keys-authentication) lisää ongelman ratkaisemiseksi.

![failedConnect](https://github.com/user-attachments/assets/06fa17b0-9fca-4a03-9404-7a9d99c54899)

*21.9.2024*

Jätin homman sikseen, kun olin jo väsynyt päivästä. Tänään aamulla kävin säätämässä uudelleen. Menin ensin UpCloudin sivuilta "console" näkymään, kun olin onnistunut vaihtamaan SSH-avaimen lennosta ja lukitsemaan itseni ulos. UpCloudilla on onneksi tämmöinen palvelu, josta pääset selaimella palvelimelle sisään käyttämällä salasanaa, jolla kirjaudutaan UpCloudin nettisivuille.

![Consolee](https://github.com/user-attachments/assets/83904dea-aa39-4698-8c38-839ad50d931d)

Hyödynsin tätä [ohjetta](https://upcloud.com/resources/tutorials/regaining-access-to-a-server-that-was-deployed-using-ssh-keys)

Consolen avulla pystyin muokkaamaan palvelimen SSH-kansion tiedostoja. ``sudo nano /etc/ssh/sshd_config``, sieltä muutin ensin *.conf tiedoston pois käytöstä, laitoin salasanakirjautumisen päälle, sekä sallin SSH-avaimella kirjautumisen, joka oli mystisesti mennyt pois päältä. Tämän jälkeen lähetin palvelimelle vielä oman SSH-public-avaimen komennolla ``ssh-copy-id santeri@94.237.35.220``, kävin muuttamassa *.conf tiedoston takaisin sallituksi, nyt saatoin kirjautua palvelimelle ilman salasanaa. 

![ilmansala](https://github.com/user-attachments/assets/972a582f-90d6-4017-ae72-68912c3da92b)

Tämän jälkeen tein perus asennukset ``sudo apt-get update``, ``sudo apt-get install ufw micro apache2``, tein palomuuriin reiät ``sudo ufw allow 22/tcp``, ``sudo ufw allow 80/tcp``, laitoin palomuurin päälle ``sudo ufw enable``

## a) Kotisivu
#### Tehtävä: Tee vähintään kolmen erillisen weppisivun kotisivu ja kopioi se näkymään palvelimellesi. Jos sinulla on oikea palvelin Internetissä, kannattaa käyttää sitä. Käytä name based virtual hosting tekniikkaa. Sivujen muokkaamisen pitää onnistua ilman pääkäyttäjän oikeuksia, niiden kopioiminen pääkäyttäjänä testisivun paikalle ei käy. Kotisivujen ei tarvitse olla hienoja, mutta niiden tulee olla validia HTML:ää ja linkittää toisiinsa.

*21.9.2024 klo 13:34*

Toisella kurssilla tehdään parhaillaan kotisivuja, jotka myöhemmin lisään tänne. Nyt kuitenkin teen hyvin yksinkertaiset html-testisivut.

Lähden muistelemaan aiemmista tehtävistä, ajoin seuraavat komennot, samalla laitoin micro-editorin oletukseksi

![histapache](https://github.com/user-attachments/assets/cd2d0c4e-90cf-4b08-8f07-719b58964684)

Tässä vaiheessa kirjasin publicsites/santerivauramo.com:iin:

![publicsites](https://github.com/user-attachments/assets/85a46afe-abb1-4e35-bade-a9715d4cde12)

~/publicsites/santerivauramo.com$ kansiossa luon index.html tiedoston komennolla ``micro index.html``, lähden rakentamaan aiemman testisivun päälle, lisään vain linkit kahteen muuhun sivuun samassa kansiossa. Katson vielä ohjeita html:n kirjoittamiseen [täältä](https://www.w3schools.com/html/). Validoin koodin [täällä](https://validator.w3.org/nu/#textarea). Index näyttää tältä:

![indexi](https://github.com/user-attachments/assets/a07ffdc9-e54d-4296-9f1a-4b0cbc7ed0ea)

Tässä vaiheessa sivu ei edes toimi, jotakin on unohtunut. Kokeilen ``sudoedit /etc/hosts`` ja lisään santerivauramo.com:n sinne. (myöhemmin tajusin, että tämä oli nimipalvelua simuloidessa tarpeellinen, ei tässä)

![edithosts](https://github.com/user-attachments/assets/ff631608-49a5-433b-8624-85355bcc8f59)

Kuten kuvassa lukee, täytyy tämän toimiakseen käydä ``/etc/cloud/templates/hosts.debian.tmpl``:ssä lisäämässä tiedot myös sinne. Kävin myös [tämän](https://www.codingcommanders.com/LAMP/apache_default_page.php) ohjeen mukaan poistamassa apachen perus index.html:n ja santerivauramo.com näyttää nyt tältä:

![indexiOn](https://github.com/user-attachments/assets/5a20da86-1f9c-4651-9fe1-02bf2920c010)

Ei ihan vielä se mitä haetaan. Luen asioita [täältä](https://www.veeble.org/kb/how-to-setup-apache-to-host-a-website-in-linux/) ja myös [täältä](https://stackoverflow.com/questions/1484595/how-to-resolve-var-www-copy-write-permission-denied). Kävin luomassa /var/www kansioon santerivauramo.com kansion, vaan eipä ollut oikeuksia, joten annoin itselleni oikeudet [3.10.2024 Tiedän, että chmod 777 oli huono ajatus, muutin sen myöhemmin järkevämmäksi]

![chmod777](https://github.com/user-attachments/assets/3c0628d3-cd2a-487e-b85c-4fc54fc8ab14)

Kävin luomassa public_html kansion ja santerivauramo.com.conf tekstitiedoston

![comconf](https://github.com/user-attachments/assets/0fd3683d-7b1f-40e0-845d-61eda5584140)

Eipä ollut tarkoitus luoda public_html kansiota, vaan tekstitiedosto. ``rm -r public_html/``, eli poistan kansion ja luon public_html tekstitiedoston ``micro public_html``, sinne laitan seuraavaa: (Myöhemmin huomasin, että juuri se kansio piti luoda, en tiedä mistä tämäkin ajatus tuli)

![htmlekatoka](https://github.com/user-attachments/assets/eb4efdbf-aeaf-4147-aeb5-53cdd887f968)

Järjen mukaan tämän pitäisi toimia, kun teen samaan kansioon eka.html ja toka.html tiedostot. Nyt olen saanut apachen kohtalaiseen solmuun:

![imagesolmussa](https://github.com/user-attachments/assets/4e0f2b48-3380-4abc-8d21-afc1c01b0812)

Pistää silmään rivi jossa lukee ``AH00526: Syntax error on line 3 of /etc/apache2/sites-enabled/santerivauramo.com.conf:``, täytynee mennä muokkaamaan ``/etc/apache2/sites-enabled/santerivauramo.com.conf``

Kävin poistamassa tuon koko tiedoston, muuten en saanut apachelle tehtyä mitään, nyt pystyy taas potkimaan demonia. Loin tiedoston uudestaan seuraavilla tiedoilla:

![imnanovirtual](https://github.com/user-attachments/assets/f1570e08-d655-4d52-a901-dc32dcd152dc)

santerivauramo.com sanoo: "404 Not Found". Kello on nyt 15:15, pidän pienen tauon ja katson samalla videon: https://www.youtube.com/watch?v=1CDxpAzvLKY.

*klo 15:45* jatkan vielä hetken ennen ruokailua ja lenkkiä.

https://httpd.apache.org/docs/2.4/vhosts/ Täältä löytyi komento ``apachectl -S``, joka näyttää seuraavaa

![imagerrorias](https://github.com/user-attachments/assets/7c431cd3-d5fa-4e4e-8750-42584a4d3c9b)

*klo 16:05, tauko*

*klo 18:35* lenkin ja suihkun jälkeen sain uutta virtaa ranteisiin. Yritän https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-virtual-hosts-on-ubuntu-20-04 tämän ohjeen avulla tehdä uusiksi tekemättömän.

![consoleoikeus](https://github.com/user-attachments/assets/d8c286a4-b641-4326-8a8a-5d7ef37d33da)

Loin /var/www kansioon omien sivujen kansion, jonne loin public_html-kansion, tämänhän loin jo aiemmin, mutta ymmärsin ohjeet väärin. Seuraavaksi annoin itselle oikeudet muokata kyseistä kansiota ja loin samanlaisen index.html tiedoston minkä aiemmin loin muualle. Muutin ``sudo micro /etc/apache2/sites-available/santerivauramo.com.conf``:sta seuraavanlaisen: [3.10.2024 oikeudet eivät ole menneet perille ylimääräisten merkkien vuoksi "#" ja "$"]

![servernamealias](https://github.com/user-attachments/assets/87cf48ea-10a8-4d7d-a5bd-a3439a3b26fb)

Nyt demonin potkaisun jälkeen pääsin sivuille. Olin jo aiemmin sallinut ``sudo a2ensite santerivauramo.com.conf``:n, ja disabloinut default conffin, joten niitä ei tarvinnut enää nyt tehdä.

![Ekatoimii](https://github.com/user-attachments/assets/20e452a2-a6ab-4571-ba97-fa6d82f601bb)

Nyt vaan kopioin html:t kahteen tiedostoon, joten saan linkit toimimaan. www.santerivauramo.com

Huomasin että vasta ``sudo chmod -R 777 /var/www/`` komento antoi luvan muokata tiedostoja normaalin käyttäjän oikeuksin. [3.10.2024 koska en ollut oikeasti laittanut mitään oikeuksia]

Loin kansioon siis ``index.html``, ``eka.html``, ja ``toka.html``-tiedostot. Sisällöt näyttävät kutakuinkin kaikissa tältä:

![sivuthtml](https://github.com/user-attachments/assets/3dbbd079-1c2e-40fc-ade4-33a66c977845)

Piti vielä muokata linkit näyttämään tältä, jotta ne toimivat:

![linkit](https://github.com/user-attachments/assets/babc40c2-a32d-47b1-b285-3560772bce08)

Jostain syystä windowsin chromella ei nyt toimi tuo sivu, mutta kännykällä pääsin ja kaikki linkit toimivat.

*klo 19:18* ei tullut hienoja ja aikaa kului, paljon tässäkin oppi asioita, parempi tehdä virheet tässä vaiheessa, homma jatkuu aikaisintaan seuraavana iltana

## b) Alidomain

#### Tehtävä: Tee kaksi uutta alidomainia, jotka osoittava omaan koneeseesi. Esimerkiksi palvelu on example.com -> linuxkurssi.example.com. Tee toinen alidomain A-tietueella ja toinen CNAME-tietueella. Alidomainit ovat tyypillisesti ilmaisia, kun sinulla on päädomain (example.com).

Aloitan lukemalla ensin A-tietueesta ja CNAME-tietueesta, ennen kuin laitan kellon juoksemaan. Ensin googlasin mikä on tietue englanniksi, koska sitä ei ole tullut aiemmin vastaan, vastaus: record. Namecheapin domainpalvelun omaavana, hain tietoa hakusanalla "A-record namecheap" ja löysin varsin hyödyllisen ohjesivun: https://www.namecheap.com/support/knowledgebase/article.aspx/319/2237/how-can-i-set-up-an-a-address-record-for-my-domain/.

*22.9.2024 klo 17:44*

Kirjaudun Namecheap palveluun ja menen "Domain List", ja sieltä "Manage", ja "Advanced DNS", sama mistä lisättiin nimipalveluun palvelimen IP-osoite alunperin. En ole ihan varma tämän toiminnasta, mikä voisi mennä pieleen, kokeilen tällaisella nimellä:

![arecord](https://github.com/user-attachments/assets/909b9c38-b6c1-4f09-be23-33c52359e1a3)

Menin palvelimelleni ja tein ``/var/www/linuxkurssi-a/public_html/`` kansion ja sinne ``index.html`` tiedoston.

![linuxatietue](https://github.com/user-attachments/assets/099b6dac-3b44-4ddc-89a0-967553e15171)

Kopioin myös .conf tiedoston ja muutan sieltä tietoja:

![tiedotarecord](https://github.com/user-attachments/assets/f05fde87-cc26-44be-a4c9-35c017524601)

Nyt kun menen https://linuxkurssi-a.santerivauramo.com/ sivulle, tulee sama index.html sivu, joka aiemmin, eli ei toimi näin. Käyn muuttamassa tuon /linuxkurssi-a kansiossa olevan html-tiedoston joksikin muuksi, kuin index.html. ``mv index.html testi-a.html``, nyt kokeilen uudestaan nettisivua. Ei toimi vieläkään, täytyy odotella ja samalla lukea CNAME-tietueesta lisää ja palata asiaan. https://www.namecheap.com/support/knowledgebase/article.aspx/9646/2237/how-to-create-a-cname-record-for-your-domain/

*klo 18:03 tauko* *jatkuu klo 18:15*

Kysyin välissä ChatGPT:ltä neuvoa, miksi tuo ohjaa vieläkin tuolle pääsivulle. Muutin ``sites-available``:sta .conf tiedostoon tiedot niin, että tuo /linuxkurssi-a kansio on ``/var/www/santerivauramo.com/public_html/linuxkurssi-a``, eli kirjaimellisesti tuon pääsivun alla.

![apaasivunalla](https://github.com/user-attachments/assets/64c0f663-3d00-4a0d-95bf-91bc5e907e47)

Toki vielä täytyy siirtää tuo kansio tuonne oikeaan paikkaan.

![kansioOikeaan](https://github.com/user-attachments/assets/e1a20a3e-0d5c-496a-845f-6f2981519cda)

Ja nyt toki täytyy vielä siirtää linuxkurssi-a kansion sisällä olevasta /public_html kansiosta tuo testa-a.html tiedosto oikeaan paikkaan. Kävin vielä lisäämässä .conf tiedostoon ``DocumentRoot`` kohtaan tuon testi-a.html loppuun. Siltikään ei vielä toimi, nyt CNAME:n kimppuun.

![siirtelyjapoisto](https://github.com/user-attachments/assets/ab1e30cc-4365-4c0f-887b-4643a941ca60)

Lähden liikkeelle tämmöisellä nimellä namecheapissa.

![cnamecheap](https://github.com/user-attachments/assets/e667446b-51e2-4fa6-844d-0342f600a02f)

Luon .conf tiedoston näillä tiedoilla:

![cnameconf](https://github.com/user-attachments/assets/31bf8c61-b595-473f-8d76-2d11107a76fe)

*tauko klo 18:45* Jätän nettisivut tekeytymään, työpäivän jälkeen ei jaksa tämän enempää vaivata päätä, huomiseen.

*23.9.2024 klo 8:40*

Aloin lukemaan täältä lisää aiheesta https://httpd.apache.org/docs/2.4/vhosts/examples.html

``systemctl status apache2.service`` kertoo seuraavaa:

![doesnotexist](https://github.com/user-attachments/assets/d89ce23a-f515-480e-926d-2fe51464bb85)

Eli apache ei löydä noita html-tiedostoja. Kävin muokkaamassa .conf tiedostoista DocumentRoot kohdat muotoon ``/var/www/santerivauramo.com/public_html_linuxkurssi-a/``. Nyt pääsen katsomaan a-tietueella luotua sivua internetistä osoittella: http://linuxkurssi-a.santerivauramo.com/testi-a.html, jostain syystä pelkkä http://linuxkurssi-a.santerivauramo.com/ näyttää sivun, josta kyllä pääsee linkin kautta menemään tuolle oikealle sivulle.

![atietueouto](https://github.com/user-attachments/assets/1e6a9ffb-3f8a-4a37-8159-8f3c20b3f823)


![atietuetoimii](https://github.com/user-attachments/assets/4d3f11fd-d889-4f67-92f3-7ae9517c5074)

Tyydyn tähän tällä erää, pitänee tunnilla kysyä lisää tästä. Nyt vielä murhetta aiheuttaa tuo CNAME-tietueen toiminta: "Sivustoon ei saada yhteyttä". Aiemmin linkittämäni namecheapin ohje kertoo, että ei voi olla samaa "Host":a CNAME-tietueen kanssa, joten joudun muuttamaan a-tietueen, jossa on "www" Host, muuksi. Nyt täytyy odotella hetki.

Jostakin mieleen tuli tuo ``chmod 777``, jonka aiemmin asetin, luin lisää siitä täältä: https://linuxhandbook.com/chmod-command/. Kävi ilmi, että tuo 777 antaa kaikille kaikki oikeudet tuohon kansioon, jota en halua. Muutin sen takaisin ``sudo chmod -R 755 /var/www/``, ainakin toistaiseksi pystyn muokata ilman pääkäyttäjänoikeuksia kansion tiedostoja.

*klo 9:17, tauko*

 *klo 9:36*

Nyt pääsin sisälle cnametest.santerivauramo.com Chromen incognito-tilassa. Sama asia, kuin a-tietueella, eli ikään kuin kansionäkymä tuolla osoitteella, mutta sieltä pääsee tuohon .html tiedostoon käsiksi. Tyydyn tähän ratkaisuun toistaiseksi myös CNAME:n osalta, tunnilla selvinnee asiasta lisää.

 ![cnamesuccess](https://github.com/user-attachments/assets/3bb8ed2d-be40-4d21-b5c6-e43296b5f7ba)


## c) Pubkey

#### Automatisoi kirjautuminen julkisella SSH-avaimella

Tämän tein jo vahingossa kohdassa aa). Kerrataanpa kuitenkin, mitä tuli tehtyä. SSH.com:sta lisää tietoa https://www.ssh.com/academy/ssh/keygen.

Aluksi poistutaan palvelimelta ``exit``, ``ssh-keygen`` luo SSH-avainparin, ``ssh-copy-id santeri@94.237.35.220`` lähettää palvelimelle julkisen avaimen. UpCloudin palvelinta luotaessa oli jo syötettävä julkinen avain.

https://upcloud.com/resources/tutorials/use-ssh-keys-authentication UpCloudin ohje. Saatoin aiemmin mennä joissain kohdissa ansaan ja lukitsin itseni pois, komento ``chmod 700 ~/.ssh`` root käyttäjänä saattoi estää julkisen avaimen lähettämisen palvelimeen. Nyt myöhemmin kuitenkin sen tehtyä, pääsen vielä kirjautumaan normaalisti ilman salasanaa.

*valmis klo 9:45*

## d) Tutki jonkin nimen DNS-tietoja 'host' ja 'dig' -komennoilla

#### Käytä kumpaakin komentoa kaikkiin nimiin ja vertaa tuloksia. Katso man-sivulta, miten komennot toimivat - esimerkiksi miten 'dig' näyttää kaikki kentät. Analysoi tulokset. Etsi tarvittaessa uusia lähteitä haastaviin kohtiin. Sähköpostin todentamiseen liittyvät SPF ja DMARC -tietojen yksityiskohdat on jätetty vapaaehtoiseksi lisätehtäväksi. Tutkittavat nimet:
#### - Oma domain-nimesi. Vertaa tuloksia nimen vuokraajan (namecheap.com, name.com...) weppiliittymässä näkyviin asetuksiin.
#### - Jonkin pikkuyrityksen, kerhon tai yksittäisen henkilön weppisivut. (Ei kuitenkaan kurssikaverin tällä viikolla vuokrattua nimeä).
#### - Jonkin suuren ja kaikkien tunteman palvelun tiedot.

En saanut "dig" komennolla tehtyä mitään, enkä saanut sitä myöskään asennettua ``sudo apt-get install dig``. Yritin vielä etsiä ``apt-cache search dig``, mutta ei löytynyt äkkiseltään mitään tehtävänantoon liittyvää ohjelmaa. Ei hätiä, https://www.cyberciti.biz/faq/debian-9-dig-command-not-found-how-to-install-dig-on-debian/ auttoi asentamisessa ja käytössä, ``sudo apt-get install dnsutils`` paketti sisältää "dig":n, sekä "host":n. Sivulta pääsee myös katsomaan [dig](https://www.cyberciti.biz/faq/linux-unix-dig-command-examples-usage-syntax/):n ja [host](https://www.cyberciti.biz/faq/linux-unix-host-command-examples-usage-syntax/):n käytöstä.

Etsin nuo aiemmat linkit, koska ``man dig`` tai ``man host`` ei tuottanut tuloksia. Kokeilin myös ``man grep`` ja monia muita, mikään ei toiminut. Joten kokeilin ``sudo apt-get install man``, ja "man" asentui palvelimelle. Nyt voin käyttää myös mania. Oletuksena "**host**" komennolla voidaan tehdä DNS (Domain Name System) kyselyjä, joko domain-nimellä tai IP-osoitteella. "man host" kertoo myös, että host komennolla voi etsiä myös paljon muitakin asioita käyttäen valinnaisia lisäargumentteja kuten: "-l". CNAME haut pitää tehdä "-t" lisäyksellä.

**Dig**-komennon oletuskysely on "A", eli osoite (Address). Domain-nimellä haettaessa siis oletuksena dig näyttää IP-osoitteen. Kuten "host", myös "dig":llä tehdään DNS kyselyjä. Monet käyttävät "dig":ä DNS ongelmien selvittämiseen.
**Oletuksena "dig" näyttää kaikki kentät**.

#### - Oma domain vs namecheap.com

![omadomainhost](https://github.com/user-attachments/assets/e58d08eb-a8c5-4431-9ab5-9d85278d6001)

![namecheaphost](https://github.com/user-attachments/assets/28999f04-fe36-43e0-bf0b-aa4914d5823b)

- Host-komento kertoo domain-nimeä vastaavan IP-osoitteen, sekä muita tietoja. Oli pakko kysyä GhatGPT:ltä tuosta "mail is handled by...", koska en löytänyt muista lähteistä mitään järkevää. "Puppusanageneraattorin" mukaan "mail is handled by":n jälkeen oleva numero kertoo millä prioriteetilla tuohon osoitteeseen lähetetyt sähköpostit ohjautuvat mihinkin osoitteisiin, mitä pienempi luku, sitä korkeamppi prioriteetti. Minun palvelin ja namecheap käyttää eri MX-tietueita.

![omaiphost](https://github.com/user-attachments/assets/7202357b-76be-4f2d-b658-af82c0007d2f)

![nameciphost](https://github.com/user-attachments/assets/8e5ba3f4-89b2-42f9-bc46-8daa6356031d)

*Tässä vaiheessa muuten vaihdoin palvelimen hostnamen siihen, mikä sen alunperinkin piti olla, ``sudo hostnamectl set-hostname debiathan``, jonka jälkeen ``sudo reboot``, ohjeet löytyy täältä: https://linuxgenie.net/change-hostname-debian-12/#post-10630-_rku68glrm7se.*

IP-osoitteella haettaessa molemmissa näkyy ".in-addr.arpa", eli "Advanced Research Projects Agency", joka liittyy internetin infrastruktuuriin jo ajalta ennen internetiä. Domain name pointer liittyy PTR-recordiin, tästä en kaikkea ihan täysin ymmärtänyt. Jostain syystä namecheapin pointer näyttää ainoastaan "namecheap.com", kun taas minun debiathanissa lukee IP-osoite ja tiedot UpCloudista, tämä lienee ainoastaan nimiasia.

- Seuraavaksi kokeillaan **dig** (Domain Information Groper)-komentoa.

![digsanteriv](https://github.com/user-attachments/assets/0710332f-fb83-4ae4-a451-d5c5826023f4)

Tietoa tulee enemmän, kuin odotin. Alussa kerrotaan dig versiosta ja käyttöjärjestelmästä, sekä mitä haettiin. 
Pseudosectionissa "udp" viittaa käytössä olleeseen protocollaan, joka voi muuttua myös "tcp":ksi tarvittaessa. "512" on bitteinä udp viestin koko. "Answer section" kertoo varsinaisesti varta itse tiedoja mitä haluttiin löytää, näitä pystyy rajaamaan halutessaan erinäisillä lisäkomennoilla.

``dig +short santerivauramo.com`` kertoo ainoastaan IP-osoitteen.

![digipvain](https://github.com/user-attachments/assets/64eb02e5-9173-431c-8ff2-95aee4e55d88)

NS kertoo nimipalvelun, tässä tapauksessa dns2.registrar-servers.com ja dns1.registrar-servers.com

![santerins](https://github.com/user-attachments/assets/d9b58e3f-0245-4b3d-b32f-2e151f40164d)

Namecheapilla nimipalveluja on huomattavasti enemmän:

![namecheapns](https://github.com/user-attachments/assets/dee834c8-2188-4572-9a9a-b99543045998)

3600 numero ennen IN:ä kertoo ChatGPT:n mukaan TTL arvon, tässä tapauksessa DNS-tietue voidaan varastoida 3600 sekunniksi, eli yhden tunnin ajaksi. IN tarkoittaa internetiä. TTL arvo auttaa nopeuttamaan verkon toimintaa, mutta suuri arvo taas voi johtaa vanhan tiedon näkymiseen käyttäjälle.

#### - Jonkin pikkuyrityksen, kerhon tai yksittäisen henkilön weppisivut

![terohost](https://github.com/user-attachments/assets/c81b71be-bec6-4f1d-a7c2-fbf404879f2b)

``host terokarvinen.com`` näyttää IP-osoitteeksi 139.162.131.217, sekä että sähköpostia hallitsee mx.runbox.com.

![terodig](https://github.com/user-attachments/assets/e967e411-95d0-418b-a95d-ba180b9eb8e6)

``dig terokarvinen.com`` tiedot näyttää suhteellisen saman kaltaisilta, kuin aiemmissa kyselyissä. Välimuistissa tietoa säilytetään 10005 sekuntia, mikä on varmasti hyvä jos haluaa minimoida palvelimen kuormitusta.

#### - Jonkin suuren ja kaikkien tunteman palvelun tiedot

![hostyoutube](https://github.com/user-attachments/assets/2499a087-687f-4250-a8d2-745a439963e4)

Ei yllätä, että youtube.com:n mail is handled by google, samaa palvelua kuitenkin molemmat. Domain name pointer näyttää uniikilta verrattuna aiempiin.

![digyoutube](https://github.com/user-attachments/assets/1f24b080-9dbc-461a-b28d-6628b3835d29)

295 sekunnin välimuisti on huomattavan pieni, eli palvelin joutuu kovemmalle rasitukselle, mutta tiedot päivittyvät nopeasti.

---

Tässä meni niin monta tuntia kaiken selvittelyn äärellä, että jätän bonustehtävän välistä. Kaikkineen aikamoinen savotta oli, mutta sitäkin opettavaisempi kokemus.

[30.9.2024 Huom. olen ottanut santerivauramo.com sivut käyttöön toista kurssia varten, joten tämän artikkelin linkit ei enää toimi samalla tavalla]

## Lähteet

Cloudflare. What is a DNS PTR record?. Luettavissa: https://www.cloudflare.com/learning/dns/dns-records/dns-ptr-record/. Luettu 23.9.2024<br>
Gite, V. 2024. How to install dig on Debian Linux 12/11/10. Luettavissa: https://www.cyberciti.biz/faq/debian-9-dig-command-not-found-how-to-install-dig-on-debian/. Luettu 23.9.2024<br>
Gite, V. 2024. Luettavissa: https://www.cyberciti.biz/faq/linux-unix-dig-command-examples-usage-syntax/. Luettu 23.9.2024<br>
Gite, V. 2024. Linux and Unix host Command Examples. Luettavissa: https://www.cyberciti.biz/faq/linux-unix-host-command-examples-usage-syntax/. Luettu 23.9.2024<br>
Nair, N. 2024. How to setup Apache Web Server to host a website in Linux. Luettavissa: https://www.veeble.org/kb/how-to-setup-apache-to-host-a-website-in-linux/. Luettu 21.9.2024<br>
Namecheap. Luettavissa: https://www.namecheap.com/<br>
Stackoverflow. Keskustelu. Luettavissa: https://stackoverflow.com/questions/1484595/how-to-resolve-var-www-copy-write-permission-denied. Luettu 21.9.2024<br>
UpCloud. Faq. Luettavissa: https://upcloud.com/resources/tutorials/use-ssh-keys-authentication. Luettu 20.9.2024<br>
UpCloud. How to regain access to a server that was deployed using SSH keys. Luettavissa: https://upcloud.com/resources/tutorials/regaining-access-to-a-server-that-was-deployed-using-ssh-keys. Luettu 21.9.2024<br>
Validator. W3Scools. Luettavissa: https://validator.w3.org/nu/#textarea. Luettu 21.9.2024<br>
W3Schools. HTML. Luettavissa: https://www.w3schools.com/html/. Luettu 21.9.2024

Tätä dokumenttia saa kopioida ja muokata GNU General Public License (versio 2 tai uudempi) mukaisesti. http://www.gnu.org/licenses/gpl.html<br>
Pohjana Tero Karvinen 2012: Linux kurssi, http://terokarvinen.com<br><br>
Kirjoittanut <em>Santeri Vauramo</em>, 2024

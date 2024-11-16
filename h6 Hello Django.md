# h6 Hello Django

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

#### Virtuaalikone
Oracle VirtualBox 7 - Debian 12 GNU/Linux (bookworm)<br>
6 Prosessoriydintä - 4GB RAM-muistia - 60GB tallennustilaa

#### Palvelin
UpCloud<br>
Debian 12 GNU/Linux (bookworm)<br>
1 Prosessoriydin - 1GB RAM-muistia - 10GB tallennustilaa

## a) Tee yksinkertainen esimerkkiohjelma Djangolla

*27.9.2024 klo 11:30*

Tähän käytän tietenkin apuna Karvisen [Django 4 Instant Customer Database Tutorial](https://terokarvinen.com/2022/django-instant-crm-tutorial/):ia

Kirjauduin palvelimelleni ja aluksi päivitetään pakettilista ``sudo apt-get update``

Asennetaan virtualenv virtuaaliympäristö ``sudo apt-get -y install virtualenv``

Luodaan uusi kansio virtualenville, joka sisältää viimeisimmät paketit ``virtualenv --system-site-packages -p python3 env/``

![Venvpaketit](https://github.com/user-attachments/assets/e335692f-fce8-4ace-a9d4-bf573d87c790)

Otetaan virtuaaliympäristö käyttöön ``source env/bin/activate``

![venvkayttoon](https://github.com/user-attachments/assets/dec76d2f-728a-465a-b3e8-edb49ebc6c80)

Nyt ollaan virtuaaliympäristössä, emme halua käyttää "pip":iä ilman virtualenviä, eikä sitä saa käyttää "sudo":lla. En halua antaa palvelinta muiden käyttöön, joten noudatan ohjetta tarkasti. Teen kyselyn, jotta varmistutaan, että olemme asentamassa virtuaaliympäristöön

![whichpip](https://github.com/user-attachments/assets/4f44af41-382e-46f5-b050-df4b23999c8a)

Luon ohjeen mukaan varmuudeksi "requirements.txt." tiedoston microlla, jotta ei vahingossakaan tule kirjoitusvirheitä kun asennetaan django pip:llä. Tiedoston sisään kirjoitan "django" ja tallennan tiedoston "ctrl+s" ja poistun "ctrl+q"

![requirementstxt](https://github.com/user-attachments/assets/71d45db8-0f8f-446e-88fc-89aecd82f8a1)

Varmuuden vuoksi kopioin requirements.txt tekstin ja katson vielä ``cat requirements.txt`` komennolla, että siellä varmasti lukee oikein "django"

![catrequirements](https://github.com/user-attachments/assets/c652f279-bdc9-4267-b3e3-efec9da6b5e8)

Tämä voi tuntua hätävarjelun liioittelulta, mutta en halua ottaa ylimääräisiä riskejä

Seuraavaksi asennetaan django pip:llä ``pip install -r requirements.txt``, hieman jännittävä toimenpide, mutta siitä selvittiin

![djangoinstalled](https://github.com/user-attachments/assets/fd1553ab-c6d5-4673-937b-04217fa23d98)

Aloitan uuden Django projektin nimeltä "sant" ``django-admin startproject sant``

Seuraavaksi kokeillaan laittaa Django palvelin toimimaan ohjeen mukaan, menen "sant" kansioon, siellä ``.manage.py runserver``

![managerunserver](https://github.com/user-attachments/assets/6a48a957-5590-458a-925e-53254b300ecd)

Nyt on tuotantopalvelin päällä. Tätä palvelinta ei kannata päästää internetiin. Kokeillaan mennä selaimella osoitteeseen "http://127.0.0.1:8000/"

Ei toimi, tuo ``.manage.py runserver`` antoi punaisella tekstillä herjaa "You have 18 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions."              ja korjaukseksi komento ``python manage.py migrate``, joten kokeilen sitä

![migrateisson](https://github.com/user-attachments/assets/17921ee0-3214-4359-a75c-3b49401f9cf5)

Tämän jälkeen uudestaan ``.manage.py runserver`` ja nyt ei tule virheilmoituksia

![djangoserveron](https://github.com/user-attachments/assets/31cb9475-3821-4542-98f6-84648e9884ba)

Kokeillaan uudestaan selaimella "http://127.0.0.1:8000/". Eikä toimi vieläkään, tietenkin tämä johtuu siitä, että olen virtuaalipalvelimella, jossa ei ole muuta kuin CLI käyttöliittymä. Pitää siis tehdä kaikki uudestaan paikallisesti. ``exit`` komennolla takaisin "taulu" virtuaalikoneelle. Teen samat asiat kuin aiemmin

![djangoversion](https://github.com/user-attachments/assets/92a4446a-936e-44fb-b830-3dadb67b8ebf)

Nyt käynnistettyäni palvelimen, pääsen selaimella "http://127.0.0.1:8000/", ja homma toimii, voi taas äiti olla ylpeä pojastaan

![djangoworks](https://github.com/user-attachments/assets/dfc6bd35-1a38-4eee-b540-1e52eb0d6d87)

Hauskana yksityiskohtana huomaan, nyt kun palvelin on päällä, palvelin näyttää käyntini tuolla sivulla suoraan reaaliajassa ja herjaa, että favicon.ico:a ei voitu toimittaa, eli se pieni kuvake mikä näkyy selaimen välilehdellä

![kayntitiedotreaali](https://github.com/user-attachments/assets/de1811d5-71c5-4925-8513-45b5fe780edd)

Seuraavaksi lisätään admin url:lle "http://127.0.0.1:8000/admin/" suoraan Karvisen ohjeilla, kätevä tuo salasananluontityökalu

![historymakeadmin](https://github.com/user-attachments/assets/c39fcfa7-2972-47c3-ac5c-0d9c387a71e7)

Nyt pääsen admin-kirjautumisikkunaan osoitteella "http://127.0.0.1:8000/admin/"

![adminlogin](https://github.com/user-attachments/assets/a7db05f8-1299-4bbc-89a1-68e54cda02bf)

Jätin käyttäjänimikentän tyhjäksi, joten oletuksena käyttäjä on tässä tapauksessa "santeri", olen sisällä

![sisallaadminina](https://github.com/user-attachments/assets/6a367551-e19e-4e77-b90d-341ab1e30fcc)

Loin uuden käyttäjän "sant" kohdasta "users - add"

![santuus](https://github.com/user-attachments/assets/3e72ca96-e34a-4851-b24b-c61df5083e23)

Annoin käyttäjälle "staff", sekä "superuser"-statuksen

![superstaff](https://github.com/user-attachments/assets/a6710da3-e12b-48af-b5d6-b9926e6d7671)

Kirjauduin ulos, ja takaisin sisään, tällä kertaa "sant" nimisenä käyttäjänä

![santsissal](https://github.com/user-attachments/assets/952d81d2-7008-49f4-b8b2-e0b2c9b9ff21)

Kokeilin mennä käyttäjienhallintaan muokkaamaan oikeuksia, otin alkuperäiseltä "santeri":lta "staff"-oikeudet pois, todettakoon, että superkäyttäjän oikeudet toimivat tällä uudella käyttäjällä

![santerisstaffpois](https://github.com/user-attachments/assets/db72c9ad-0a5e-4637-b85e-6f3e2b31846b)

#### Luodaan asiakastietokanta

Luodaan "crm/" kansio CRM-sovellukselle ``./manage.py startapp crm``

Lisätään sovellus asennettuihin sovelluksiin ``micro sant/settings.py``

![crmlisatty](https://github.com/user-attachments/assets/59c60335-15c2-4d5f-a0f5-a4053802b470)

Lisätään modeleja ``micro crm/models.py``. Tiedostossa oli valmiina "from django.db import models". Lisätään sinne vielä ``class Customer(models.Model):
   name = models.CharField(max_length=300)``. Täten tietokantaan tulee "customer"-tietue "name" sarakkeella

![crmcustodata](https://github.com/user-attachments/assets/6501c770-656d-4748-b5ab-01936d0ad724)

Nyt ajetaan samat ``./manage.py makemigrations`` ja ``./manage.py migrate``

Rekisteröidään tietokanta admin/ kansioon ``micro crm/admin.py``, ja sille lisätään tiedot ``from . import models
admin.site.register(models.Customer)``

Nyt pystytään lisäilemään asiakkaita, tosin niiden nimet ovat luokkaa "Customer object (1)"

![asiakkaatcustobject](https://github.com/user-attachments/assets/4f416d17-69f2-4c69-9e19-addcc9565139)

Mennään muokkaamaan models.py tiedostoa ``micro crm/models.py`` ja lisätään sinne pari riviä, jotka saavat asiakkaan nimen näkymään oikein

![asiakasnimetmuok](https://github.com/user-attachments/assets/e4f1acda-c32d-441a-a4c4-f0fe4bbc3f1c)

Nyt kun yrittää käynnistää palvelinta, tulee herja

![pyerrorii](https://github.com/user-attachments/assets/c7531b51-022c-4219-be21-d5546096462b)

Kopioin suoraan Karvisen ohjeesta tuon koko koodin, jolla sain taas palvelimen käyntiin

![koodikopied](https://github.com/user-attachments/assets/0bea835d-df6e-4fab-a006-15b1e4aef09c)

Nyt näkyy asiakkaiden nimet oikein

![asiakkaidennimetoikein](https://github.com/user-attachments/assets/d3f90c61-a727-424e-b708-71982b7bbb5a)

*klo 13:08, aikaa kului 1h 38min*




## b) Tee Djangon tuotantotyyppinen asennus

#### Voit halutessasi tehdä asennuksen omalle, paikalliselle virtuaalikoneelle. Sen ei tarvitse näkyä Internetiin.

*klo 13:30*

Tähän käytän pohjana [Tero Karvisen Deploy Django 4 - Production Install](https://terokarvinen.com/2022/deploy-django/) ohjeita

Teen tämän paikallisella virtuaalikoneellani. Apache2 ja micro on jo asennettu, joten menen suoraan kohtaan "Create some web content as a user"

Luon kansioita tätä varten ``mkdir -p publicwsgi/sant/static/``, sekä .html tiedoston ``echo "Onpas mukava nähdä teitä täällä tänään."|tee publicwsgi/sant/static/index.html``

Käyn varmistamassa, että kyseiset kansiot ja tiedosto tuli luotua oikein

![onpasmukava](https://github.com/user-attachments/assets/fdb6241d-cbd9-4f86-8b70-b08080c13aab)

Looking good. Seuraavaksi lisään uuden virtualhostin ``sudoedit /etc/apache2/sites-available/sant.conf`` seuraavilla tiedoilla

![conffitiedot](https://github.com/user-attachments/assets/83b51b51-b5d9-48ec-b78b-cfd79c57f8a0)

Laitan apachen hyväksymään sivun ``sudo a2ensite sant.conf``, "000-default.conf":n olen jo laittanut pois, mutta lähtötilanteessa ``sudo a2dissite 000-default.conf`` hoitaa sen, laitetaan myös aiemmassa tehtävässä tekemäni "hattu.example.com.conf" ja "tama.juttu.com.conf" pois ``sudo a2dissite`` komennolla

Varmistetaan konfiguraation toimivuus ``/sbin/apache2ctl configtest``

![configtest](https://github.com/user-attachments/assets/47ca6dc0-743e-4655-950b-dc4b8987ed44)

AH00558 herja lienee normaalia, pääasia, että "Syntax OK" ilmestyy lopussa

Potkaistaan demonia, eli käynnistetään apache uudelleen ``sudo systemctl restart apache2`` ja curlataan localhostia ``curl http://localhost/static/``, joten nähdään päästäänkö tietoihin käsiksi

![curllocal](https://github.com/user-attachments/assets/7c34b968-4165-4132-b134-175fbec748b5)

Mennään "~/publicwsgi" kansioon, jossa komennolla ``virtualenv -p python3 --system-site-packages env``. Hypätään virtualenviin ``source env/bin/activate``

``which pip`` "/home/santeri/publicwsgi/env/bin/pip". Tehdään taas requirements.txt ja sinne "django", jonka jälkeen ``pip install -r requirements.txt`` ja ``django-admin --version`` "5.1.1", ok

Kopioin a) kohdassa tekemäni sant/ kansion tuonne publicwsgi/ kansioon home/santeri/ kansiosta ``cp -R sant/ publicwsgi/``

``django-admin startproject sant``, koska sant/ kansio on jo olemassa, joten ei tarvinnut tätä vaihetta näemmä tehdä enää

![santalready](https://github.com/user-attachments/assets/41fd45bc-b8c4-4d04-aab8-97141920bd49)

Seuraavaksi yhdistetään Python Apacheen käyttäen mod_wsgi:tä. Ohjeessa kerrotaan, että kaiken toimiakseen, täytyy tietää kolme absoluuttista polkua, joten ne kannattaa ottaa ylös tekstitiedostoon, josta niitä voi kopioida tarvittaessa. 

Tiedostosijainnit ovat: 

- Django-projektin pääkansio, joka sisältää manage.py tiedoston "/home/santeri/publicwsgi/sant"

![djangosant](https://github.com/user-attachments/assets/04b52122-5df4-42bf-8c95-40c859b40845)

- Polku wsgi.py tiedostoon "/home/santeri/publicwsgi/sant/sant/wsgi.py"

![wsgisant](https://github.com/user-attachments/assets/cf6f096d-74ff-4131-96a9-cba5ec866aca)

- Virtualenvin site-packages kansio "/home/santeri/publicwsgi/env/lib/python3.11/site-packages"

![sitepackansio](https://github.com/user-attachments/assets/b1115ade-97f3-41ed-9556-60acbb93940b)

Käyn muokkaamassa sant.conf tiedostoa ``sudoedit /etc/apache2/sites-available/sant.conf``, nyt ohjeessa on niin pitkälti tietoa, jota tuonne tulee, että kopioin suoraan kaiken tiedostoon ja muokkaan omilla poluilla ja nimillä. Tässä tulee hyötykäyttöön äsken hakemani sijainnit, en lähtenyt muuttamaan muuta kuin nuo sijainnit

![sijainnitsianconffi](https://github.com/user-attachments/assets/31752957-1ea2-4305-98e2-22975b2d7319)

Jatketaan, asennan WSGI moduulin, jotta Apache tietää mitä WSGI komennot tarkoittavat. ``sudo apt-get -y install libapache2-mod-wsgi-py3``

``/sbin/apache2ctl configtest`` antaa "Syntax OK":ta, joten kaikki hyvin. Potkaistaan demonia ``sudo systemctl restart apache2``, kokeillaan ``curl -s localhost|grep title``

![successgrep](https://github.com/user-attachments/assets/c3c5dd1d-00c1-4ca0-a27b-1a0553e548da)

On se helppoa, kun on hyvät ohjeet. Kokeillaan vielä ``curl -sI localhost|grep Server``

![onseapache](https://github.com/user-attachments/assets/fedf2ba2-3b1b-4090-8a77-7de8d7019928)

On se Apache, tässä vielä näkymä selaimella

![selaimellatoimii](https://github.com/user-attachments/assets/d4d81243-8cb5-4e17-9dd5-a16b68597702)

#### Otetaan DEBUG pois käytöstä

Navigoidaan "/home/santeri/publicwsgi/sant/sant" ``micro settings.py``

Sieltä muutetaan 
- DEBUG = False
- ALLOWED_HOSTS = ["localhost"]

Jos menisi oikeaan tuotantoon, laittaisin esim. ["localhost", "testisivu.santerivauramo.com"]

![hosoiri](https://github.com/user-attachments/assets/16c9bae7-6e5c-453f-964d-334b0f0b985b)

Näillä komennoilla ladattiin uudet asetukset, tämä muutos tarvinnee ``sudo systemctl restart apache2`` toimiakseen

``http://localhost/admin`` selaimeen, pääsee kirjautumaan admin näkymään, mutta näyttää karulta. Aiemmassa vaiheessa luotu "santeri" käyttäjällä pääsee generoidulla salasanalla kirjautumaan. Sivu tosin näyttää tältä: 

![djangoapachejuttu](https://github.com/user-attachments/assets/d8faa74e-29a5-4feb-be70-e375514a8972)

#### Korjataan kauneusvirheet

Mennään uudestaan ``micro settings.py`` ja lisätään sinne

- import os
- STATIC_ROOT = os.path.join(BASE_DIR, 'static/')

Nyt ``cd ..`` ja ``./manage.py collectstatic``, promptiin "yes"

![staticitmanage](https://github.com/user-attachments/assets/15a6819f-9380-47de-8e42-88ec6621e81e)

Testataan miltä nyt näyttää "http://localhost/admin", kaunista

![productstyle](https://github.com/user-attachments/assets/eda9af22-ad4e-4b17-8121-66b1547c42ba)

*klo 15:05*

#### Homma kaiketi siinä, jäi vielä mietityttämään kun ohjeessa puhuttiin tästä

![ohjekayttaja](https://github.com/user-attachments/assets/d9ba86d5-07e4-4b14-8135-1c42a9127381)

Eli WSGI moduulin ajamiseen ei kannata käyttää sudo käyttäjää, jotein täytyy tehdä uusi käyttäjä tätä varten

Kävin tämän tiimoilta luomassa uuden käyttäjän ilman kummempia oikeuksia ``sudo adduser djangotyyppi``

Jonka jälkeen muokkasin "sudoedit /etc/apache2/sites-available/sant.conf":sta "TUSER":n "djangotyyppi":ksi

![djangosankari](https://github.com/user-attachments/assets/2395c6be-349e-44a3-b7a9-9a1eeab5db45)

Yritin asentaa "acl" "Access Control List" ohjelmaa käyttäjien hallintaa varten [näillä ohjeilla](https://wiki.debian.org/Permissions), mutta se oli jo asennettu. Piti kuitenkin mountata filesystem acl optiolla, koska ohjelma antoi virheen

![aclasennus](https://github.com/user-attachments/assets/f1db130a-a408-425e-97b0-b628766102c5)

Jostain syystä djangotyyppi oli päässä muuttunut debiantyypiksi, *tarkkuutta mies*

Nyt pystyin antamaan komennolla ``setfacl -R -m user:djangotyyppi:7 /home/santeri/publicwsgi`` djangotyypille oikeudet tuohon kansioon. ``-R`` viittaa tuohon kansioon ja kaikkiin sen alikansioihin ja tiedostoihin

Nyt kokeilen vielä muokata kansioiden sisältöä djangotyypillä. Ensin vaihdetaan käyttäjää komennolla ``su djangotyyppi``

![djangosekoilu](https://github.com/user-attachments/assets/65b8d3aa-27e3-4f66-8f99-54259f4d2f83)

Nyt mennään takaisin santerina ja käydään katsomassa onko tallennettu tiedosto todella tuolla ``su santeri``

![siellon](https://github.com/user-attachments/assets/cd9bc6b1-7d13-47f7-8d3c-5ee156f22509)

Siellä se on

![sielse](https://github.com/user-attachments/assets/ba480324-2fc5-46c7-9936-6b8c272ccb5c)

## Lähteet

Debian Wiki. Permissions. Luettavissa: https://wiki.debian.org/Permissions. Luettu 27.9.2024
Django. django-admin and manage.py. Luettavissa: https://docs.djangoproject.com/en/5.1/ref/django-admin/. Luettu 27.9.2024<br>
Karvinen, T. 2022. Deploy Django 4 - Production Install. Luettavissa: https://terokarvinen.com/2022/deploy-django/. Luettu 27.9.2024<br>
Karvinen, T. 2022. Django 4 Instant Customer Database Tutorial. Luettavissa: https://terokarvinen.com/2022/django-instant-crm-tutorial/. Luettu 27.9.2024<br>
W3Schools. Django Tutorial. Luettavissa: https://www.w3schools.com/django/index.php. Luettu 27.9.2024

---

Tätä dokumenttia saa kopioida ja muokata GNU General Public License (versio 2 tai uudempi) mukaisesti. http://www.gnu.org/licenses/gpl.html<br>
Pohjana Tero Karvinen 2012: Linux kurssi, http://terokarvinen.com<br><br>
Kirjoittanut <em>Santeri Vauramo</em>, 2024

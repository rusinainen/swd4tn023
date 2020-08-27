## Sisällysluettelo

# Ohjelmistokehittäjän infra ja automaatiotyökalut

Ohjelmistokehittäjän työssä on paljon oheistyökaluja ja tekniikoita, jotka helpottavat kehitystyötä ja parantavat kehitystyön laatua. Tässä osiossa on esitelty näitä asioita.

## Linuxin ja komentorivin perusteet

Linux ja muut Unix-pohjaiset käyttöjärjestelmät, kuten MacOS, ovat kehittäjien keskuudessa paljon käytettyjä. [Stackoverflown 2020 kehittäjillä tekemän vuosittaisen tutkimuksen](https://insights.stackoverflow.com/survey/2020) mukaan Linuxia [on edellisen vuoden aikana käyttänyt 55% kehittäjistä](https://insights.stackoverflow.com/survey/2020#technology-platforms). Unix-pohjaiset käyttöjärjestelmät tarjoavat toisaalta paljon työkaluja, jotka helpottavat kehittäjän työtä (kuten automaattiset paketinhallintatyökalut kuten apt) ja niillä on helppo myös työskennellä komentorivin kautta vahvoilla komennoilla, joilla pystyy esimerkiksi käsittelemään ohjelmistojen lokitiedostoja, ajastamaan komentoja jne.

### Unix-pohjaisten järjestelmien komentorivi

Unix-pohjaisten järjestelmien komentorivi on kehittäjille monipuolinen ja voimakas työkalu. Komentorivi mahdollistaa paitsi erilaisten työkalujen nopean asentamisen, tiedostojärjestelmän helpon selaamisen, erilaisten testiympäristöjen pystyttämisen jne. Käydään tässä läpi esimerkkien kautta Linux-järjestelmän kehittäjälle hyödyllisiä perusasioita ja komentoja, suurin osa asioista toimii myös muissa Unix-pohjaisissa järjestelmissä.

Alkuun mainittakoon yleisesti, että Linux-komentojen keskeyttäminen tarvittaessa tapahtuu CTRL^C:llä (tai vielä voimakkaampi yhdistelmä on CTRL^Z, joka ei anna processille mahdollisuutta yrittää sulavaa siistiä lopettamista).

#### Unix-komentoja

Print working directory, tulostaa kansion jossa olet.
```shell
$ pwd
/Users/myusername/Documents/
```
Change directoy komennolla liikutaan kansioissa. Tabulatoria käyttämällä voi automaattitäydentää syötteitään, eli jos aloittaa kirjoittamaan jotain ja painaa tabulaattoria, niin järjestelmä täydentää siihen mahdollisen ainoan vaihtoehdon tai sitten tulostaa syötteen alun perusteella mahdolliset vaihtoehdot. 
```shell
$ cd kansionnimi #siirtyy kansioon kyseisen kansion alla
$ cd /Users/kansionnimi #siirtyy absoluuttiseen kansioon tiedostorakenteen juuressa
$ cd .. #Siirtyy hiearkiassa edelliseen kansioon
$ cd ../kansionnimi #Siirtyy hierarkiassa edelliseen alikansioon ja sen alla olevaan kansioon
$ cd #siirtyy käyttäjän kotikansioon
$ cd -#siirtyy kansioon, jossa olit ennen tätä
```
Listaa kyseisen kansion tiedostot ja kansiot
```shell
$ ls
```
-l-vivulla ls-komento tulostaa myös mm. tiedostojen luontipäivämäärän ja koon
```shell
$ ls -l 
```
Linuxin komentoja voi putkittaa (merkillä |). Putkitus more-komennon kanssa tulostaa kansiorakenteen sivu kerrallaan, josta voi edetä enterillä tiedosto kerrallaan (pagedown/up tai u ja d toimii mahdollisesti hieman unix-järjestelmästä riippuen myös), CTRL^C (tai q) lopettaa.
```shell
$ ls -l | more
```
Linuxissa komennon syötteen voi ajaa myös tiedostoon >-merkillä. 
```shell
$ ls -l > tiedosto.txt
```
Manual komennolla (man) saa esiin tietyn kommennon ohjeen ja näkee mm. mitä lisävipuja komennolle voi antaa.
```shell
$ man ls
```
Tiedoston sisällön voi tulostaa cat-komennolla
```shell
$ cat tiedosto.txt
```
Tiedoston voi luoda tai kaksi tiedostoa yhdistää kolmanteen tiedostoon myös catilla
```shell
$ cat >luouusitiedosto.txt
$ cat tiedosto1.txt tiedosto2.txt >yhdistetty.txt
```
Tiedostoja voi kopioida cp-komennolla. Käyttämällä -R (recursive) -vipua voi kopioida kokonaisia kansiorakenteita
```shell
$ cp tiedosto.txt ../ #kopioi tiedoston hierarkiassa edelliseen kansioon
$ cp -R hakemisto /Users/me/toinenhakemisto #kopioi hakemiston toiseen hakemistoon
```
Tiedostoja voi siirtää tai uudelleennimetä mv-komennolla. 
```shell
$ mv tiedosto.txt tiedostonuusinimi.txt
$ mv tiedosto.txt ../ #siirtää tiedoston hierarkiassa edelliseen kansioon
```
Kansioita voi luoda mkdir-komennolla 
```shell
$ mkdir hienokansio
```
Tiedostoja voi poistaa rm-komennolla -R-vivulla voi poistaa kokonaisia kansiorakenteita. 
```shell
$ rm tiedosto
$ rm -R kansiorakenne
$ yes | rm -R kansiorakenne #jotkin unix-järjestelmät kysyvät joka tiedoston kohalla yes-varmistetta, sen voi automatisoida putkittamalla yes-komennon
```
Tyhjän tiedoston voi luoda touch komennolla
```shell
$ touch tiedosto.txt
```
Tiedostoja voi muokata Unix-järjestelmien lukuisilla eri tekstieditoreilla, kuten hyvin perinteinen vi:llä (komennot pitää muistaa aikalailla ulkoa), nano:lla tai monesta järjestelmästä löytyvällä emacs:lla. Editoreilla on omat shortcut-komennot eri asioille kuten tiedoston muutosten tallentamiselle ja editorin sulkemiselle. 
```shell
$ vi tiedosto.txt
$ nano tiedosto.txt
$ emacs tiedosto.txt
```
Tiedostoja voi etsiä tiedostonimen perusteella find-komennolla. *-merkkiä voi käyttää tarkoittamaan, että mitkä tahansa merkit tähden ja seuraavan merkin välillä kelpaa hakuun.
```shell
$ find -name tiedo*.txt
```
Tiedostojen sisällä olevaan tekstiä voi etsiä (ja tekstin sisältävän rivin tulostaa) grep-komennolla. -R etsii rekursiivisesti koko kansiorakenteesta. Grepille annettava etsintäavain tulkitaan [Regular expression -formaatissa](https://en.wikipedia.org/wiki/Regular_expression), eli sitä voi käyttää hyvin monipuoliseen etsintään. 
```shell
$ grep "kissa" tiedosto.txt #sisältääkö sanan kissa
$ grep -R "kissa" * #etsi tämän kansion kaikista alikansioista ja tiedostoista sanaa kissa
$ grep -E "koira|kissa" tiedosto.txt # Sovella regex-patternia, eli etsi sekä koiraa, että kissaa.
```
Tiedoston lopun voi tulostaa tail-komennolla. Tämä komento on erityisen hyödyllinen logitiedostojen seuraamiseen -F-vivulla, jolloin se jää seuraamaan tiedostojen päivittymistä ja jatkaa muutosten tulostamista ruudulla. Tailin voi myös putkittaa grep:in kanssa ja seurata tiedostossa esimerkiksi vain tietyn patternin mukaisia muutoksia. Komennosta voi poistua CTRL^C:llä
```shell
$ tail -F server.log
$ tail -F server.log | grep "ERROR"
```
Kahden tiedoston eroavaisuudet voi listata diff komennolla.
```shell
$ diff tiedosto1.txt tiedosto2.txt
```
chmod-komennolla voi muokata tiedoston tai kansion oikeuksia, eli kenellä on oikeus tehdä tiedostoon minkälaisia muutoksia. Komennolla voi antaa tietylle tiedostolle oikeuksia kolmella eri tasolla. Eli miten tiedostoa saa käyttää sen luonut käyttäjä, miten käyttäjän kanssa samaan ryhmään kuuluvat käyttäjät ja miten muut. Eli tiedostolle voi antaa omalle käyttäjälleen luku-, suoritus- ja muokkausoikeudet, saman käyttäjäryhmän ihmisille luku- ja suoritusoikeudet ja muille ei mitään oikeuksia (edes tiedoston lukemiseen). Tästä monimutkaisesta komennosta voi lukea lisää vaikka [täältä](https://www.computerhope.com/unix/uchmod.htm).
```shell
$ chmod 750 mysqlscript.sh
$ chmod u+x mysqlscript.sh #annetaan käyttäjälle (u) suoritus (x=exceute) oikeudet

```
chown-komennolla voi siirtää tiedoston omistajuuden tietylle käyttäjälle.
```shell
$ chown mysqlUser mysqlscript.sh
```
Sudo ("Switch User and Do this command") -komennolla vältetään nykyään käyttäjien tarvetta vaihtaa käyttäjätiliä root-käyttäjäksi suorittaakseen jonkin enemmän käyttöoikeuksia vaativan asian kuten vaikka uuden ohjelman asentamisen. Jos käyttäjälle on myönnetty sudo-oikeudet käyttöjärjestelmään, niin hän voi siis vaihtaa itsensä hetkellisesti superkäyttäjäksi ja ajaa jonkin enemmän oikeuksia vaativan komennon.
```shell
$ sudo apt install mysql
```
Sudo ("Switch User and Do this command") -komennolla vältetään nykyään käyttäjien tarvetta vaihtaa käyttäjätiliä root-käyttäjäksi suorittaaksen jonkin enemmän käyttöoikeuksia vaativan asian kuten vaikka uuden ohjelman asentamisen. Jos käyttäjälle on myönnetty sudo-oikeudet käyttöjärjestelmään, niin hän voi siis vaihtaa itsensä hetkellisesti superkäyttäjäksi ja ajaa jonkin enemmän oikeuksia vaativan komennon.
```shell
$ sudo apt install mysql
```
Unix-järjestelmissä voi määritellä ympäristömuuttujia kuten PATH-muuttujan. Käyttöjärjestelmä etsii komentoja PATH-muuttujaan lisätyistä hakemistoista. Path-muuttujan nykyisen sisällön voi tulostaa echo-komennon avulla.
```shell
$ echo $PATH
```
export-komennolla voit lisätä PATH-muuttujaan uusia hakemistoja. Alla oleva komento siis sanoo, että uusi PATH-ympäristömuuttuja on nykyinen PATH-ympäristömuuttujan arvo ja lisäksi siihen lisätään /usr/me/uusihakemisto-polku.
```shell
$ export PATH=$PATH:/usr/me/uusihakemisto
```
Processes (ps) komennolla voi listata esimerkiksi kaikki kyseisen käyttäjän käynnissä olevat prosessit. Niihin liittyen voi tulostaa ps-komennolla monipuolisesti tietoja eri prosesseista, kuten niiden muistin ja prosessorin käyttömäärän. Näiden perusteella voi tehdä johtopäätöksiä, jos joki prosessi (esimerkiksi serveri) on jäänyt jumiin tai esimerkiksi jokin ohjelma ikuiseen silmukkaan. 
```shell
$ ps ux
```
ps-komennon ajamisen jälkeen (jossa näkyy prosessi id, eli PID) voi joskus olla tarpeen tappaa jokin prosessi sen prosessi id:n (PID) perusteella. Tähän käytetään kill-komentoa, jonka käytössä kannattaa olla jossain määrin varovainen, sillä voi myös sammuttaa koko käyttöjärjestelmän.
```shell
$ kill 1234 #annetaan prosessille mahdollisuus vielä ajaa jotain komentoja
$ kill -9 1234 #Jos prosessi ei vielä edellisellä kuollut, niin tällä lähtee.
```
Käyttäjiä voi lisätä Linux-järjestelmissä useradd-komennolla ja salasana lisätään passwd-komennolla
```shell
$ sudo useradd username
$ sudo passwd username
```
Unix-järjestelmissä on yleensä myös monia tietoliikenteeseen liittyviä käteviä komentoja.
```shell
$ ping 123.343.434 #miten nopeasti osoite vastaa, voi testata vaikka omaa tuotantopalvelinta
$ whois haaga-helia.fi #tietoja tunnuksesta
$ ssh host.jotain.fi  #avataan ssh-etäyhteys etäpalvelimeen
$ wget https://osoite.fi/kuva.jpeg  #Ladataan tiedosto internetistä
```
Ohjelmistokehittäjä pystyy ajamaan unix-järjestelmissä lukuisia kehitystyöhön liittyviä ohjelmia. Tässä vielä niistä muutamia esimerkkejä.
```shell
$ node
$ python
$ java
$ mysql
$ mongo
```

#### Unix-skriptit ja chron

Unix-pohjaisissa käyttöjärjestelmissä on mahdollista luoda skriptitiedostoja, joiden avulla voi esimerkiksi yhdistää monta komentoa yhdeksi komennoksi. Myös esimerkiksi yksinkertaisen haarautumislogiikan (if-else) ja silmukoiden toteuttaminen on mahdollista. Tarkemmin skriptien-kirjoittamisesta voi lukea esimerkiksi [tästä perusoppaasta](https://docs.csc.fi/support/tutorials/env-guide/linux-bash-scripts/).
```shell
$ nano skripti.sh #luodaan skriptitiedosto
```
```bash
#!/bin/bash #skriptit avataan yleensä tällä kommentilla joka kertoo käytettävän komentorivitulkin 
echo "Hello World" #tulostetaan huvikseen teksti echo-komennolla
ls -l | grep $1 > tamakansio.txt #ajetaan kyseisen kansion sisällöstä tiedostoon sellaiset rivit, jotka sisätävät käyttäjän skriptille antaman ensimmäisen parametrin sanan.
```
```shell
$ chmod u+x skripti.sh #annetaan käyttäjälle suoritusoikeudet skriptiin, muuten sitä ei voi ajaa.
$ ./skripti.sh hello #ajetaan skripti ja annetaan sille parametrina sana "hello"
```

Unix-pohjaisissa järjestelmissä on komentojen ajastuksen mahdollistava [cron-prosessi](https://opensource.com/article/17/11/how-use-cron-linux). crontab-komennolla voi määritellä ajastettavia komentojan "ajastustauluun" (cron table). crontab-komento luo käyttäjälle ajastustaulutiedoston /var/spool/cron-kansioon. Crontaulun ajastussyntaksi seuraa tiettyä kaavaa. *-merkki tarkoittaa, että kyseinen ajanmääre voi olla mitä vain.
```shell
1 2 3 4 5 /polku/komento argumentit_komennolle
```
jossa:
* 1: Minuutit (0-59)
* 2: Tunnit (0-23)
* 3: Päivä (0-31)
* 4: Kuukausi (0-12 [12 == Joulukuu])
* 5: Viikonpäivä (0-7 [7 tai 0 == sunnuntai])

```shell
crontab -e #Komento avaa oletustekstieditorin, johon voi syöttää haluamansa ajastuksen
5 0 * * * /Users/me/skripti.sh hello #Aja skripti.sh viisi minuuttia keskiyön jälkeen joka päivä
```

## Versionhallinta (Git) ja Github-palvelu

Varsinkin kun ohjelmistoja kehitetään tiimeissä, on tärkeää hallita ohjelmiston versiohistoriaa ja mahdollistaa kehittäjille osallistuminen kaikkien komponenttien kehittämiseen silloinkin, kun joku muu saattaa kehittää kyseistä komponenttia myös. Versionhallintasovellukset toimivat arkkitehtuuriosiossa esitellyn "Event sourcing"-periaatteen mukaan, eli versionhallintaan lisätään ("commitoidaan") aina uusi muutos, mutta siellä olevia asioita ei suoraan muokata. Näin ollen versionhallinnassa voidaan aina palata minä tahansa ajanhetkenä sovelluksessa vallinneeseen tilaan.

[Git-sovellus](https://git-scm.com/) on noussut nykyisin ylivoimaisesti suosituimmaksi versionhallintasovellukseksi. Git:iä käytetään monesti yhdessä [Github-palvelun](https://github.com/) kanssa, joka mahdollistaa Git-versionhallintarepositoryjen tallentamisen julkiseen verkkoon ja tarjoaa lisäksi muita käteviä toimintoja, kuten [pull-requestit](https://yangsu.github.io/pull-request-tutorial/#:~:text=What%20is%20a%20Pull%20Request,follow%2Dup%20commits%20if%20necessary.).

Erona ennen suositumpiin versionhallintajärjestelmiin (SVN ja CVS), git on [hajautettu versionhallintajärjestelmä](https://fi.wikipedia.org/wiki/Hajautettu_versionhallintaj%C3%A4rjestelm%C3%A4). Tämä siis tarkoittaa sitä, että jokaisella käyttäjällä on oma paikallinen "kopio", eli repository sovelluksen versionhallinnasta. Tässä omassa repositoryssä käyttäjällä on täydet oikeudet tehdä muutoksia ja hallita omaa versiohistoriaansa. Paikallisen repositoryn lisäksi yleensä käytössä on myös ns. upstream-repository (hostattuna esimerkiksi juuri githubissa), johon käyttäjä voi yrittää tarjota omia paikallisia muutoksiaan tai hakea sieltä muiden tekemiä muutoksia.

![Git ja Github](img/git_ja_github.png)

[Git-sovelluksen omassa dokumentaatiossa](https://git-scm.com/) kehutaan sen olevan helppokäyttöinen. Rehellisyyden nimissä on kuitenkin sanottava, että varsinkin kun Gittiä käytetään yhdessä remote-repositoryjen kanssa, niin koko paletti on vähintäänkin vaikea, jos ei suorastaan monimutkainen. Gitin monipuoliseen ja tehokkaaseen käyttöön liittyy paljon käsitteitä periaatteita, joiden ymmärtämiseen menee aikaa. Gitin peruskäyttö on kuitenkin melko suoraviivaista.

## Paketinhallinta ja buildaaminen

## Jatkuva integrointi (CI/CD)

## Kontittaminen (docker)

## Github dokumentointi

## Kehitysympäristöt ja IDE:t

## Virtualisointi

## Palvelimet ja deployaaminen

## Tehtävät

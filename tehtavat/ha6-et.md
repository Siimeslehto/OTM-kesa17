# Laskari 6 - Kotitehtävät

## Tehtävä 1

Asiakaspalaverin aikana järjestelmää tilaava asiakas kertoi seuraavaa:

Yrityksellämme on joukko ruokien ja tuotteiden asetteluun erikoistuneita työntekijöitä, joiden toimenkuvana on kerätä ideoita ja kuvia ketjumme kahviloiden ja kauppojen asetteluista.  Tällä hetkellä käytämme sähköpostia digikameralla otettujen kuvien siirtämiseen, mutta haluaisimme jotain nykyaikaisempaa.

Tavoitteenamme on saada teiltä järjestelmä, johon työntekijämme voivat lisätä työpäivien aikana kerättyjä kuvia.  Varustamme työntekijämme iPhone 6s -älypuhelimilla. Järjestelmän tulee tukea kännykälle asennetusta applikaatiosta lähetettyjen kuvien vastaanottamista.

Jokaisella työntekijällä on käyttäjätunnus järjestelmään, jota käytetään myös tietona lähetetyissä kuvissa. Kuvia katsottaessa pitää siis näkyä kenen lähettämä kuva on.

Järjestelmän pitää tarjota mahdollisuus kuvien listaamiseen eri kategorioihin, esimerkiksi sesonkeihin perustuen. Sesonkikategoriaan kuuluvat muunmuassa joulukuvat, pääsiäiskuvat jne.  Jokainen kuva voi olla yhdessä tai useammassa kategoriassa. Kuvia pitää myös pystyä hakemaan kategorioiden perusteella.

Käyttäjät voivat myös kommentoida toisten lisäämiä kuvia, sekä antaa niille suosituksia __like__-tyylisesti.  Normaalien työntekijöiden lisäksi järjestelmää käyttävät myös ns. managerit, jotka eivät itse lähetä kuvia järjestelmään, mutta voivat kommentoida niitä.  Managereiden pitää myös pystyä luomaan raportteja lähetetyistä kuvista. Kerran viikossa sihteerit siirtävät viikon aikana valmistuneet raportit järjestelmästä erilliseen toiminnanohjausjärjestelmäämme.

*Tee ylläolevasta ohjelmakuvauksesta käyttötapausmalli*. Eli etsi käyttäjät (kertaa viikon 1 kalvoilta mikä on käyttötapauksen käyttäjä eli englanniksi actor), listaa käyttötapaukaukset (tarkkaa kuvausta ei tarvita, nimi, käyttäjät sekä esiehdot riittävät) ja piirrä käyttötapausdiagrammi.

## Tehtävä 2

Tee ylläolevasta kuvauksesta määrittelyvaiheen luokkakaavio, joka tarkentaa kuvienhallintajärjestelmän käsitteistön ja käsitteiden suhteet.

HUOM: *älä lisää toiminnallisuutta määrittelyvaiheen luokkakaavioon*, älä mieti kuka tekee ja mitä, keskity luokkien olioiden pysyväluontoisiin yhteyksiin.

## Tehtävä 3

Ohjelmoinnin jatkokurssin kolmannen viikon tehtävä [Numerotiedustelu](http://www.cs.helsinki.fi/group/java/s15-materiaali/viikko10/#151numerotiedustelu) on tullut tutuksi tämänkin kurssin laskareista. Numerotiedustelulla on 7 eri toimintoa, joita käyttäjä käyttää "komentotulkin" tapaan. Perinteinen tapa toteuttaa komentotulkki on tehdä iso valintarakenne, jossa on oma if-haara (tai switch case) jokaista komentoa kohti. Rakenne on hieman ikävä ja laajennettavuudeltaan huono, sillä uuden komennon lisääminen edellyttää muutosten tekemistä useampaan kohtaan valintarakenteen koodia.

Vaihtoehtoinen "olio-orientoitunut" tapa tehdä komentotulkki on tehdä jokaisesta erillisestä komennosta abstraktin luokan <code>Komento</code> aliluokkia.

Hae olio-orientoituneen komentotulkin sisältävä koodi [NumerotiedusteluVko7.zip](/koodi/NumerotiedusteluVko7.zip?raw=true) osoitteesta
[https://github.com/Ooppa/OTM-kesa17/tree/master/koodi/](https://github.com/Ooppa/OTM-kesa17/tree/master/koodi/)

Kokeile ohjelmaa ja selvitä itsellesi sen toimintaperiaate. Ohjelman rakennetta ja toimintaperiaatetta ei välttämättä ole aluksi ihan helppo ymmärtää.

**Kuvaa ohjelman rakenne pakkauskaaviona.** Tässä tehtävässä ei ole tarvetta kuvata pakkauskaavion tasolla pakkausten sisällä olevia luokkia.

## Tehtävä 4

Kuvaa edellisen tehtävän ohjelman rakenne luokkakaaviona.

Huom: oliolla olevan HashMap:in voi ajatella luokkakaavion suhteen olevan samanlainen kuin ArrayList, eli sitä ei kannattane luokkakaavioon merkitä omana luokkanaan.

Kaikkia Komennon periviä luokkia ei kaavioon kannata merkitä.

## Tehtävä 5

Kuvaa sekvenssikaaviona mitä tapahtuu kun edellisen tehtävän pääohjelma käynnistää sovelluksen eli kutsuu metodia <code>suorita</code> ja käyttäjä antaa komentoriville seuraavat syötteet:

<pre>
1
pekka
040-1234567
5
pekka
x
</pre>

Oliolla olevan HashMap:in voi ajatella myös sekvenssikaavion suhteen olevan samanlainen kuin ArrayList, eli sitä ei kannattane merkitä kaavioon omana oliona.

Aloita sekvenssikaavio siis hetkestä jolloin kaikki komento-oliot on jo luotu ja pääohjelma kutsuu luokan <code>Sovellus</code> metodia <code>suorita()</code>.

## Tehtävä 6

Hae koneellesi osoitteesta [https://github.com/Ooppa/OTM-kesa17/tree/master/koodi](https://github.com/Ooppa/OTM-kesa17/tree/master/koodi/) löytyvä tiedosto [Sateliittiseuranta.zip](/koodi/Sateliittiseuranta.zip?raw=true) ja pura se koneellesi. Zip-pakkauksesta löytyy NetBeans-projekti, joka sisältää [lintujen sateliittiseurannan](https://www.luomus.fi/fi/satelliittiseurannat) hallinnointiin tarkoitetun sovelluksen.

Tutustu sovelluksen toimintaan.

Huomaat että koodi ei ole ihan priimaa.

Listaa koodista kaikki oliosuunnittelun periaatteita rikkovat kohdat ja koodihajut.

Refaktoroi koodi mahdollisimman siistiksi. Koodin toiminnallisuuden ei tule muuttua, eli pidä huoli siitä että et riko testejä!

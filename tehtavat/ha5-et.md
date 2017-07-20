# Laskari 5 - Kotitehtävät

## Tehtävä 1

Seuraavassa on lueteltu käsitteitä, jotka ovat jossain suhteessa keskenään. Mitä suhdetyyppiä (normaali yhteys, kompositio, perintä, riippuvuus, ...) kussakin tapauksessa kannattaa käyttää (vai onko kyse jostain ihan muusta?). Piirrä esim. pieni luokkakaavio jokaisesta tilanteesta.

* [Ruokaileva filosofi](https://fi.wikipedia.org/wiki/Aterioivat_filosofit) käyttää haarukkaa
* Unixissa tiedosto on joko tavallinen tiedosto tai hakemisto
* Monikulmion kulmat ovat järjestetty jono pisteitä
* Opiskelija hallitsee Java-kielen
* Ihmisellä on syntymävuosi
* Hiiri, näppäimistö ja Näyttö ovat I/O-laitteita (eli tietokoneen syötelaite)
* Polku yhdistää kaksi kylää
* Monopolissa on 2-8 Pelaajaa. Pelaaja voi olla yksittäinen henkilö tai joukkue. Joukkue koostuu yksittäisistä henkilöistä

## Tehtävä 2

Laajennetaan toissaviikolla aloitettua monopolin luokkamallia. Ota pohjaksi oma toissaviikkoinen ratkaisusi tai esimerkkivastaus.

Voit olettaa, että kyseessä on tietokonepelinä toteutettava monopoli.

Seuraavassa asioita, jotka pitäisi tulla mallissa esille.

Ruutuja on useampaa eri tyyppiä:
* aloitusruutu
* vankila
* sattuma ja yhteismaa
* asemat ja laitokset
* normaalit kadut (joihin liittyy nimi)

Monopolipelin täytyy tuntea sekä aloitusruudun että vankilan sijainti.

Jokaiseen ruutuun liittyy jokin toiminto.

Sattuma- ja yhteismaaruutuihin liittyy kortteja, joihin kuhunkin liittyy joku toiminto.

Toimintoja on useanlaisia. Ei ole vielä tarvetta tarkentaa toiminnon laatua.

Normaaleille kaduille voi rakentaa korkeintaan 4 taloa tai yhden hotellin (kun hotelli rakennetaan se korvaa talot). Kadun voi omistaa joku pelaajista. Pelaajilla on rahaa.

Tässä vaiheessa ei vielä liitetä peliin metodeja.

## Tehtävä 3

Ohjelmointikursseilla on tutustuttu sangen hyödylliseen luokkaan <code>ArrayList</code>. ArrayList-luokan olioden voi ajatella olevan automaattisesti itseään kasvattavia vaihtelevanmittaisia taulukoita, joihin voidaan säilöä muita olioita. Lyhyesti ilmaistuna ArrayList-oliot ovat "oliosäiliöitä".

Etsi [Java-API:sta](http://docs.oracle.com/javase/8/docs/api/) luokka ArrayList.

Tutkitaan ArrayListin ominaisuuksia. Tällä kertaa ei kiinnitetä huomiota ArrayListin metodeihin vaan sen sijaintiin Java-API:n luokkahierarkiassa. Eli mikä on ArrayList:in yläluokka, mitä sisarluokkia (eli yliluokan muita aliluokkia) sillä on, mikä on ArrayListin yliluokan yliluokka, mitä serkkuluokkia ArrayListilla on, jne ...

**Erityisesti on syytä selvittää, mitä rajapintoja ArrayList mainitsee toteuttavansa? Mitä toteutettavissa rajapinnoissa luvataan?**

Piirrä tilanteesta luokkakaavio. **Jos kaavio kasvaa liian isoksi, älä tunge kaikkea kuvaan.** Metodinimiä ei kannata luokkakaavioon merkitä.

ArrayList ja muut oliosäiliöt liittyvät läheisesti kurssin _Tietorakenteet ja algoritmit_ aihepiiriin. Erilaisten "oliosäiliöiden" luokkaperhe on erittäin hyödyllinen. Kuten huomataan, on erilaisia säiliöluokkia todella suuri määrä. Joskus onkin haastavaa löytää parhaiten omiin tarpeisiin soveltuva luokka.

Ohjelmoinnin jatkokurssin [kolmannella viikolla](http://www.cs.helsinki.fi/group/java/s15-materiaali/viikko10/) tutustuttiin hieman ArrayListin tapaan toimivaan HashSet-luokkaan. **Miten HashSet sijoittuu Javan luokkahierarkiaan ArrayList:in suhteen? Mikä selittää sen, että HashSet:iä ja ArrayList:iä voi käyttää monin paikoin toistensa tilalla? Mitä eroavaisuuksia löytyy?**

## Tehtävä 4

Ohjelmoinnin jatkokurssilla myös <code>HashMap</code>:it ovat käyneet tutuksi. Tee sama HashMapien suhteen kuin mitä teit edellisessä tehtävässä ArrayListille, eli

* Mitä HashMap mainitsee toteuttavansa, ja rajapinnoissa luvataan?
* Mitä luokkia HashMap perii ja mitä ovat sen "sisarluokat"?
  * Erityisen kiinnostava näistä on TreeMap, miten se eroaa HashMap:ista

Tee myös HashMapin suhteista rajapintoihin ja muihin luokkiin luokkakaavio.

## Tehtävä 5

Hae koneellesi osoitteesta [/koodi/](/koodi/) löytyvä tiedosto [Palkanlaskenta.zip](koodi/Palkanlaskenta.zip) ja pura se koneellesi. Zip-pakkauksesta purkautuu NetBeans-projekti, joka sisältää yrityksen palkanlaskentaa ja -maksuja hoitavan sovelluksen.

Tutustu sovelluksen toimintaan. Sovelluksella ei ole dokumentaatiota, mutta testit demonstroivat kuinka ohjelmiston tulisi toimia.

Sovelluksen koodin laatu jättää hieman toivomisen varaa, varsinkin kun tiedetään, että sovellusta on laajennettava lähiaikoina. Tiedetään, että sovelluksessa tulee kuukauden kuluttua olla ainakin seuraavat lisäominaisuudet:
* Henkilölle pitää pystyä asettamaan milloin tahaansa uusi palkka
* Ohjelmiston tulee pystyä palauttamaan työntekijöiden ja maksutapahtumien lista myös muissa kuin CSV-formaatissa, esim. JSON:ina

Uuden palkan asettamiselle on jo tehty testi <code>palkanMuuttuminenHuomioidaanMaksussa</code>, testi on kuitenkin tässä vaiheessa kommentoitu pois. Huomaa, että testin poiskommentoimiseksi riittää testin edessä olevan annotaation _@test_ poiskommentointi.

**Mallinna sovelluksen rakenne luokkakaaviona ja piirrä sekvenssikaavio tilanteesta, missä _main_-metodissa oleva koodi suoritetaan.**

**Huom:** sovellus "suorittaa" tilisiirron lähettämällä tilisiirtopyynnön osoitteeseen [https://otmpaymentinterface.herokuapp.com/](https://otmpaymentinterface.herokuapp.com/). Kun suoritat ohjelman ensimmäistä kertaa, tilisiirtopalvelulta kuluu hetki (noin 10 sek) käynnistymiseen.

## Tehtävä 6

Luennoilla ja kalvoissa on mainittu 4 oliosuunnittelun periaatetta
* Single responsibility principle
* Program to interfaces, not to concrete implementations
* Favor composition over inheritance
* Riippuvuuksien minimointi

Luentomonisteessa mainituilla www-sivuilla  (esim. [https://sourcemaking.com/refactoring/smells](https://sourcemaking.com/refactoring/smells)) on lueteltu joukko _koodihajuja_ (engl. code smell), jotka ovat siis kooditasolla näkyviä merkkejä laiskasta ohjelmointityylistä tai huonosta oliosuunnittelusta. Koodihajut ja oliosuunnittelun periaatteet liittyvät läheisesti toisiinsa. Usein koodihajun taustalla on nimenomaan jonkun olionsuunnittelun periaatteen laiminlyönti.

**Listaa kaikki oliosuunnittelun periaatteita rikkovat kohdat ja koodihajut, joita löydät _palkanlaskentaohjelmistosta_.**

## Tehtävä 7

**Refaktoroi palkanlaskenta mahdollisimman siistiksi.** Koodin toiminnallisuuden ei tule muuttua, eli pidä huoli siitä että et riko testejä!

Yritä tehdä refaktorointi mahdollisimman pienin, koko ajan ohjelman ehjänä pitävin askelin.

Jos jokin testi hajoaa, ja et heti näe missä vika, lienee helpointa "reprodusoida" rikki mennyt asia mainissa. Esim. jos testi <code>tuntienLisaaminenTyontekijalle</code> hajoaa, katso testin koodia:

```java
    @Test
    public void tuntienLisaaminenTyontekijalle(){
        p.lisaaTyontekija(1,"Arto", "123 456", "mannerheimintie","ar@to.fi","040-12345",10);

        p.lisaaTunnit(1, 5);
        assertEquals(0, p.maksuhistoria("csv").size());

        p.maksaPalkat();
        assertEquals(1, p.maksuhistoria("csv").size());

        List<String> odotettu = new ArrayList<>();
        odotettu.add(toCSV("Arto", "123 456", "50"));
        assertContent(odotettu,p.maksuhistoria("csv"));        
    }   
```

ja "kopioi" oleellisilta osin koodi mainiin:

```java
    public static void main(String[] args) {
        Palkanlaskenta p = new Palkanlaskenta();
        p.lisaaTyontekija(1,"Arto", "123 456", "mannerheimintie","ar@to.fi","040-12345",10);

        p.lisaaTunnit(1, 5);

        // varmista että tulostuu 0
        System.out.println(p.maksuhistoria("csv").size());

        p.maksaPalkat();

        List<String> tulos = p.maksuhistoria("csv");

        // varmista että tulostuu 1        
        System.out.println(tulos);       

        // varmista, että lista tulos sisältää merkkijonon "Arto;123 456;50"
   }
```

Näin debuggaus muuttunee helpommaksi.

## Tehtävä 8

**Laajenna** ohjelmaa toteuttamalla metodi <code>uusiTuntipalkka</code> (jonka runko löytyy jo koodista).
Ota testi <code>palkanMuuttuminenHuomioidaanMaksussa</code> pois kommenteista. Kriteeri laajentumisen onnistumiselle on se, että _kaikki_ testit menevät edelleen läpi.

Koodille tekemäsi refaktoroinnin eräänä onnistumisen kriteerinä voi pitää sitä, kuinka helppo tässä tehtävässä tapahtuva laajennus on.

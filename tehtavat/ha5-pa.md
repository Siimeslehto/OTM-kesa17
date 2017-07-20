# Laskari 5 - Paikanpäällä tehtävät

## Osa 1: Ostoskorin ohjelmointi TDD-tekniikalla

Kumpula Biershopin ohjelmointi on käynnissä. Esimies antaa tehtäväksesi ohjelmoida luokan Ostoskori.

Kuten viikon kolme [esimerkkivastauksesta](http://www.cs.helsinki.fi/u/mluukkai/otm2015/biershop-luokka.pdf) selviää, Ostoskori ei talleta suoraan luokan Tuote-olioita, ostoskoriin vietyjä ostoksia vastaavat Ostos-luokan oliot:

![](http://www.cs.helsinki.fi/u/mluukkai/otm2015/kori1.png)

Luokka Tuote on hyvin suoraviivainen. Tuotteesta tiedetään nimi, hinta ja varastosaldo:

``` java
public class Tuote  {

    private String nimi;
    private int hinta;
    private int saldo;

    public Tuote(String nimi, int hinta) {
        this.nimi = nimi;
        this.hinta = hinta;
    }

    public void setHinta(int hinta) {
        this.hinta = hinta;
    }

    public int getHinta() {
        return hinta;
    }

    public String getNimi() {
        return nimi;
    }

    public int getSaldo() {
        return saldo;
    }

    public void setSaldo(int saldo) {
        this.saldo = saldo;
    }

    @Override
    public String toString() {
        return nimi + " " + hinta + " euroa";
    }
}
```
Tuote siis kuvaa yhden tuotteen esim. Lapin kulta tiedot (nimi, hinta ja varastosaldo). Ostoskoriin ei laiteta tuotteita vaan Ostoksia, ostos viittaa tuotteeseen ja kertoo kuinka monesta tuotteesta on kysymys. Eli jos ostetaan esim. 24 Lapin kultaa, tulee ostoskoriin Ostos-olio joka viittaa Lapin kulta -tuoteolioon sekä kertoo että tuotetta on valittu 24 kpl. Ostos-luokan koodi:

``` java
public class Ostos {

    private int lkm;
    private Tuote tuote;

    public Ostos(Tuote tuote) {
        this.lkm = 1;
        this.tuote = tuote;
    }

    public int hinta() {
        return lkm * tuote.getHinta();
    }

    public int lukumaara() {
        return lkm;
    }

    public String tuotteenNimi() {
        return tuote.getNimi();
    }

    public void muutaLukumaaraa(int muutos) {
        lkm += muutos;
        if ( lkm<0 ) {
            lkm = 0;
        }
    }

    @Override
    public int hashCode() {
        return this.tuote.hashCode();
    }

    @Override
    public boolean equals(Object obj) {
        if (obj == null || getClass() != obj.getClass()) {
            return false;
        }

        Ostos other = (Ostos) obj;

        return this.tuote.equals(other.tuote);
    }
}
```
Tehtävänäsi on siis ohjelmoida luokka ostoskori. Ostoskorin API:n tulee näyttää seuraavalta (metodeille on lisätty returnit jotta kääntäjä ei valittaisi koodista):

``` java
public class Ostoskori {

    public int tuotteitaKorissa() {
        // kertoo korissa olevien tuotteiden määrän
        // metodin nimi on hieman huono, kyseessä oikeastaan koriin lisättyjen "asioiden" määrä
        // eli jos koriin lisätty 2 kpl tuotetta "Koff", tulee metodin palauttaa 2     

        return -1;
    }

    public int hinta() {
        // kertoo korissa olevien tuotteiden yhteenlasketun hinnan

        return -1;
    }

    public void lisaaTuote(Tuote lisattava) {
        // lisää tuotteen
    }

    public void poista(Tuote poistettava) {
        // poistaa tuotteen
    }

    public ArrayList<Ostos> ostokset() {
        // palauttaa listan jossa on korissa olevat ostokset

        return null;
    }

    public void tyhjenna() {
        // tyhjentää korin
    }
}
```
*Ohjelmoimme nyt ostoskorin käyttäen Test Driven Development -tekniikkaa, eli kirjoitamme ensin testejä ja vasta sen jälkeen testit toteuttavan koodin.*

Huom: tämä tehtävä on samantapainen kun Ohjelmoinnin jatkokurssin viikon 3 ostoskori. Älä kuitenkaan copypastaa tehtävän koodia, sillä nyt tehtävässä ostoskorissa on pieniä mutta ratkaisevia eroja ohjan ostoskoriin.

Tee NetBeans-projekti ja kopioi sinne ylläolevat luokat Tuote ja Ostos ja pohja luokalle Ostoskori. Voit myös hakea luokat sisältävän valmiin Maven-muotoisen projektin [täältä](https://github.com/mluukkai/OTM2013/raw/master/koodi/TestDrivenDevelopment.zip)


**Tee seuraavat testit ja aina jokaisen testin jälkeen testin läpäisevä koodi**

*1. Luodun ostoskorin hinta ja tuotteiden määrä on 0.*

tee siis testi joka testaa ylläolevan. Kun testi on valmis, ohjelmoi ostoskoria sen verran että testi menee läpi

*2. Yhden tuotteen lisäämisen jälkeen ostoskorissa on 1 tuote.*

huom: joudut siis luomaan testissäsi tuotteen jonka lisäät koriin:

``` java
    @Test
    public void yhdenTuotteenLisaamisenJalkeenKorissaYksiTuote() {
        Tuote karjala = new Tuote("Karjala", 3);

        kori.lisaaTuote(karjala);

        // ...
    }
```

**Täsmennys:**

Vaikka metodin lisaaTuote parametrina on Tuote-olio, **ostoskori ei tallenna tuotetta** vaan luomansa Ostos-olion joka "tietää" mistä tuotteesta on kysymys.

![](http://www.cs.helsinki.fi/u/mluukkai/otm2015/kori2.png)

*3. Yhden tuotteen lisäämisen jälkeen ostoskorin hinta on sama kuin tuotteen hinta.*

*4. Kahden eri tuotteen lisäämisen jälkeen ostoskorissa on 2 tuotetta*

*5. Kahden eri tuotteen lisäämisen jälkeen ostoskorin hinta on sama kun tuotteiden hintojen summa*

*6. Kahden saman tuotteen lisäämisen jälkeen ostoskorissa on 2 tuotetta*

*7. Kahden saman tuotteen lisäämisen jälkeen ostoskorin hinta on sama kun 2 kertaa tuotteen hinta*

*8. Yhden tuotteen lisäämisen jälkeen ostoskori sisältää yhden ostoksen*

tässä testataan ostoskorin metodia ostokset():

``` java
    @Test
    public void yhdenTuotteenLisaamisenJalkeenKorissaYksiOstosOlio() {
        kori.lisaaTuote(tuote1);

        ArrayList<Ostos> ostokset = kori.ostokset();

        // testaa että metodin palauttamin listan pituus 1
    }
```

*9. Yhden tuotteen lisäämisen jälkeen ostoskori sisältää ostoksen, jolla sama nimi kuin tuotteella ja lukumäärä 1*

Testin on siis tutkittava jälleen korin metodin ostokset palauttamaa listaa:

``` java
    @Test
    public void yhdenTuotteenLisaamisenKorissaYksiOstosOlioJollaOikeaTuotteenNimiJaMaara() {
        kori.lisaaTuote(karhu);

        Ostos ostos = kori.ostokset().get(0);

        // testaa täällä, että palautetun listan ensimmäinen ostos on halutunkaltainen.
    }
```

*10. Kahden eri tuotteen lisäämisen jälkeen ostoskori sisältää kaksi ostosta*

*11. Kahden saman tuotteen lisäämisen jälkeen ostoskori sisältää yhden ostoksen*

eli jos korissa on jo ostos "karhu" ja koriin lisätään sama tuote uudelleen, tulee tämän jälkeen korissa olla edelleen vain yksi ostos "karhu", lukumäärän tulee kuitenkin kasvaa kahteen:

![](http://www.cs.helsinki.fi/u/mluukkai/otm2015/kori3.png)


*12. Kahden saman tuotteen lisäämisen jälkeen ostoskori sisältää ostoksen jolla sama nimi kuin tuotteella ja lukumäärä 2*

*13. Jos koriin on lisätty tuote ja sama tuote poistetaan, on kori tämän jälkeen tyhjä*

tyhjä kori tarkoittanee että tuotteita ei ole, korin hinta on nolla ja ostoksien listan pituus nolla

*14. jos korissa on kaksi samaa tuotetta ja toinen näistä poistetaan, jää koriin ostos jossa on tuotetta 1 kpl*

*15. Metodi tyhjenna tyhjentää korin*

Jos ostoskorissasi on mukana jotain ylimääräistä, refaktoroi koodiasi niin että kaikki turha poistuu. Erityisesti ylimääräisistä oliomuuttujista kannattaa hankkiutua eroon, tarvitset luokalle vain yhden oliomuuttujan, kaikki ylimääräiset tekevät koodista sekavamman ja vaikeammin ylläpidettävän. Jos luokassasi on ylimääräisiä oliomuuttujia, poista ne. Varmista koko ajan testien avulla ettet riko mitään.

## Osa 2: Lyyrakortin refaktorointi

Viime viikon paikanpäällä tehdyssä tehtävässä teimme kattavat yksikkötestit Lyyrakortille ja Kassapäätteelle. Voit ottaa tämän tehtävän pohjaksi joko viimeviikkoisen koodisi tai sen [mallivastaukset](../mallivastaukset/mallivastaukset_ha4-paikalla.md)

Tehtävän yhteydessä oleva Kassapäätteen koodi sisältää paljon copypastea, sekä edullisen että maukkaan lounaan ostosta käteisellä huolehtiva koodihan on oleellisesti sama. Vastaavasti korttioston suhteen.

Refaktoroi koodi siistiksi. Tavoitteena on saada kaikki mahdollinen copypaste pois ja saada koodista näin mahdollisimman helposti ylläpidettävä. Hyvä ylläpidettävyys tarkoittaa sitä, että jos jokun asia muuttuu (esim. ruuan hinta) tai koodista löydetään bugi, tai kassapäätettä halutaan laajentaa uusilla ateriatyypeillä, tulisi muutostarpeen olla mielellään ainioastaan yhdessä kohdassa koodia.

Refaktoroinnissa luokan ulospäinnäkyvän rajapinnan eli julkisten metodien tulisi toimia samaan tapaan kuin ennen refaktorointia, *eli toisinsanoen refaktorointi ei saa rikkoa testejä*.

Kun refaktoroit koodia, pyri etenemään mahdollisimman pienin ja hallituin askelein, pitäen testit läpimenevänä lähes koko ajan. Refaktoroidessasi voit muuttaa vapaasti luokan sisäistä toteutusta (esim. lisätä metodeja, muuttaa oliomuuttujien tyyppejä, ym.) haluamallasi tavalla.

**Vihje:** myytyjen edullisten- ja maukkaiden lounaiden määrää ei kannata pitää yksittäisissä muuttujissa, vaan esim. HashMapissa. Myös eri tyyppisten lounaiden hinnan tallentaminen kannattaa hoitaa HashMapin avulla.

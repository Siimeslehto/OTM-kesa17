## JUnit-ohje NetBeansin käyttäjille

Tutustutaan yksikkötestien tekemiseen JUnit-testauskehyksen avulla. Yksikkötesteissä testauksen kohteena ovat ohjelman pienimmät rakenneosaset eli yksittäiset oliot ja niiden metodit.

Esimerkkinä seuraavassa Ohjelmoinnin perusteista tuttu luokka LyyraKortti (luokan nykyinen nimi tosin on Maksukortti, kyseessä kuitenkin täsmälleen sama koodi). Kortin koodi:

``` java
public class LyyraKortti {
    private double arvo;
    private final double EDULLINEN = 2.5;
    private final double MAUKAS = 4.0;
 
    public LyyraKortti(double arvoaAlussa) {
        this.arvo = arvoaAlussa;
    }
 
    public void syoEdullisesti() {
        if (this.arvo >= EDULLINEN) {
            this.arvo -= EDULLINEN;
        }
    }
 
    public void syoMaukkaasti() {
        if (this.arvo > MAUKAS) {
            this.arvo -= MAUKAS;
        }
    }
 
    public void lataaRahaa(double rahamaara) {
        if (rahamaara < 0) {
            return;
        }
 
        this.arvo += rahamaara;
        if (this.arvo > 150) {
            this.arvo = 150;
        }
    }
 
    @Override
    public String toString() {
        return "Kortilla on rahaa " + this.arvo + " euroa";
    }
} 
```

Palauta mieleen kortin käyttötapa lukemalla [tehtäväkuvaus](http://www.cs.helsinki.fi/group/java/s15-materiaali/viikko4/#88maksukortti)

### alkutoimet

Tehdään NetBeans-projekti, luo luokka LyyraKortti ja copypastea siihen yllä oleva koodi. Lopputuloksen pitäisi näytää seuraavalta:

![](http://www.cs.helsinki.fi/u/mluukkai/otm2012/junit1.png)

Jos et osaa käyttää NetBeansia, lue [tämä](http://www.cs.helsinki.fi/group/java/k11/materiaali-k11/ohpe/netbeansohje.html) ja kysy tarvittaessa apua ohjaajalta. Tällä kurssilla tullaan käyttämään sen verran modernien ohjelmointiympäristöjen edistyneitä piirteitä että tekstieditorin käyttö ohjelmointitehtävissä on hankalaa. Netbeansin lisäksi sopivia ohjelmointiympäristöjä ovat Eclipse ja IntelliJ, niille ei kuitenkaan tule ohjeita.

Seuraavaksi aloitetaan testien luominen. Klikataan projektia hiiren oikealla näppäimellä ja valitaan _new > other > Unit Tests > JUnit test > next_. Annetaan testiluokalle nimi, esim. LyyraKorttiTest (testiluokan nimen on päätyttävä sanaan Test). Valitaan versio _JUnit 4.x_

Jos toimit oikein testi ilmestyy projektin sisälle kohdan Test Packages alle:

![](http://www.cs.helsinki.fi/u/mluukkai/otm2012/junit2.png)

Eli varsinainen koodi kirjotetaan Source Packages:in alle ja testit Test Packages:in alle.

### aloitetaan testien kirjoittaminen

Testiluokka näyttää alussa seuraavalta:

``` java
import org.junit.After;
import org.junit.AfterClass;
import org.junit.Before;
import org.junit.BeforeClass;
import org.junit.Test;
import static org.junit.Assert.*;

public class LyyraKorttiTest {
    
    public LyyraKorttiTest() {
    }
    
    @BeforeClass
    public static void setUpClass() {
    }
    
    @AfterClass
    public static void tearDownClass() {
    }
    
    @Before
    public void setUp() {
    }
    
    @After
    public void tearDown() {
    }
    // TODO add test methods here.
    // The methods must be annotated with annotation @Test. For example:
    //
    // @Test
    // public void hello() {}
}
```

Ei välitetä vielä sisällöstä. Poistetaan kommentti alimmasta metodista (vihje voit poistaa kommentit maalaamalla kommentoidun osan ja painamalla ctrl, shift ja c, samalla tavalla voit myös kommentoida koodia):

``` java
public class LyyraKorttiTest {
    // ...

    @Test
    public void hello() {}
} 
```

Kyseessä on testi joka ei testaa mitään. Ajetaan kuitenkin testi valitsemalla _Run > Test project_ (tai alt F6). Jotain tapahtuu ja NetBeansin alareunaan tulee joko vihreä palkki joka ilmaisee että kaikki testit menevät läpi. Jos palkki ei ole näkyvissä , saat sen esiin valitsemalla NB:stä _Window > IDE Tools > Test results_.

Kirjoitetaan ensimmäinen oikea testi. Testit ovat testiluokan sisällä olevia metodeja, joiden yläpuolella/edessä on annotaatio eli merkintä @Test. Metodien paluuarvon tyyppi on yleensä void. Tehdään testi, eli kirjoitetaan seuraava koodinpätkä hello():n alapuolelle:

``` java
    @Test
    public void konstruktoriAsettaaSaldonOikein() {
        LyyraKortti kortti = new LyyraKortti(10);
        
        String vastaus = kortti.toString(); 

        assertEquals("Kortilla on rahaa 10.0 euroa", vastaus);
    }
```

Ensimmäinen rivi luo kortin jonka saldoksi tulee 10 euroa. Testin on tarkoitus varmistaa, että konstruktorin parametrina oleva luku menee kortin alkusaldoksi. Tämä varmistetaan selvittämällä kortin saldo. Kortin saldo selviää kortin toString-metodin muodostamasta merkkijonoesitysestä. Testin toinen rivi kutsuu toStringia ja ottaa merkkijonoesityksen talteen muuttujaan `vastaus`. Viimeinen rivi tarkastaa onko vastaus sama kuin odotettu tulos, eli "Kortilla on rahaa 10.0 euroa".

Tarkastus tapahtuu JUnitissa paljon käytettyä assert- eli väittämäkomentoa käyttäen. Komento testaa onko sille ensimmäisenä parametrina annettu odotettu tulos sama kuin toisena parametrina oleva testissä saatu tulos.


Seuraavaksi ajetaan testi (Run > Test project tai alt F6) ja toivotaan että testi menee läpi.

Vaihtoehtoinen tapa määritellä sama testi olisi seuraava:

``` java
    @Test
    public void konstruktoriAsettaaSaldonOikein() {
        LyyraKortti kortti = new LyyraKortti(10);       
        assertEquals("Kortilla on rahaa 10.0 euroa",  kortti.toString());
    }
```

eli metodikutsun palauttamaa arvoa ei oteta erikseen talteen muuttujaan vaan sitä kutsutaan suoraan assertEquals-vertailun sisällä. Käy niin, että ennen kuin varsinainen vertailu suoritetaan, tehdään metodikutsu ja vertailtavaksi tulee metodin palauttama arvo.

Kannattaa varmistaa, että JUnit todellakin löytää virheet, eli muutetaan edellistä testiä siten että se ei mene läpi (assertEqualsissa väitetään että saldo olisi 9):

``` java
    @Test
    public void konstruktoriAsettaaSaldonOikein() {
        LyyraKortti kortti = new LyyraKortti(10);       
        assertEquals("Kortilla on rahaa 9.0 euroa",  kortti.toString());
    }
```

Varmistetaan että saamme alalaitaan punaisen palkin.

**Korjataan testi taas ennalleen.**

Painamalla työkalupalkin kohdalla hiiren oikeaa nappia ja valitsemalla Customize voit vetää listasta palkkiin oman napin testien ajamista varten. Näin kannattaa tehdä välittömästi. Jos olet käyttänyt koneella TMC:tä, se on luonut napin (musta silmä) jo valmiiksi.

Tehdään seuraavaksi testi, joka varmistaa, että kortin saldo pienee kutsuttaessa metodia <code>syoEdullisesti</code>.

``` java
    @Test
    public void syoEdullisestiVahentaaSaldoaOikein() {
        LyyraKortti kortti = new LyyraKortti(10);
        
        kortti.syoEdullisesti();
        
        assertEquals("Kortilla on rahaa 7.5 euroa", kortti.toString());
    }
```

Jälleen testi alkaa kortin luomisella. Seuraavaksi kutsutaan kortin testattavaa metodia ja viimeisenä on rivi joka varmistaa, että tulos on haluttu, eli että kortin saldo on pienentynyt edullisen lounaan hinnan verran.

### muutama huomio

* Molemmat testit ovat yksinkertaisia ja testaavat vain yhtä asiaa, tämä on suositeltava käytäntö vaikka on mahdollista laittaa yhteen testiin useitakin assert:eja
* Testit on nimetty siten, että nimi kertoo selvästi sen mitä testi testaa
* Kaikki testit ovat toisistaan riippumattomia, esim. kortilla maksaminen ei vaikuta kortin saldoon kuin siinä testissä missä korttimaksu tapahtuu. Molemmat testit toimivat siis kuin kaksi erillistä pientä 'main':ia. Testien järjestyksellä testikoodissa ei ole merkitystä. 
* Testit kannattaa ajaa mahdollisimman usein, eli aina kun teet testin (tai muutat normaalia koodia) aja testit!

Tehdään muutama testi lisää

``` java
    @Test
    public void syoMaukkaastiVahentaaSaldoaOikein() {
        LyyraKortti kortti = new LyyraKortti(10);

        kortti.syoMaukkaasti();

        assertEquals("Kortilla on rahaa 6.0 euroa", kortti.toString());
    }
    
    @Test
    public void syoEdullisestiEiVieSaldoaNegatiiviseksi() {
        LyyraKortti kortti = new LyyraKortti(10);

        kortti.syoMaukkaasti();
        kortti.syoMaukkaasti();
        // nyt kortin saldo on 2
        kortti.syoEdullisesti();

        assertEquals("Kortilla on rahaa 2.0 euroa", kortti.toString());
    }  
```

Ensimmäinen testeistä tarkastaa, että maukkaasti syöminen vähentää saldoa oikein. Toinen testi varmistaa, että edullista lounasta ei voi ostaa jos kortin saldo on liian pieni.

### Testin alustustoimet

Huomaamme, että testikoodissamme toistoa: kolme ensimmäistä testiä luovat kaikki samanlaisen 10 euron saldon omaavan kortin.

Siirrämmekin metodin luonnin testiluokassa määriteltyyn alustusmetodiin:

``` java
public class LyyraKorttiTest {

    LyyraKortti kortti;

    @Before
    public void setUp() {
        kortti = new LyyraKortti(10);
    }

    @Test
    public void konstruktoriAsettaaSaldonOikein() {
        assertEquals("Kortilla on rahaa 10.0 euroa", kortti.toString());
    }

    @Test
    public void syoEdullisestiVahentaaSaldoaOikein() {
        kortti.syoEdullisesti();
        assertEquals("Kortilla on rahaa 7.5 euroa", kortti.toString());
    }

    @Test
    public void syoMaukkaastiVahentaaSaldoaOikein() {
        kortti.syoMaukkaasti();
        assertEquals("Kortilla on rahaa 6.0 euroa", kortti.toString());
    }

    @Test
    public void syoEdullisestiEiVieSaldoaNegatiiviseksi() {
        kortti.syoMaukkaasti();
        kortti.syoMaukkaasti();
        kortti.syoEdullisesti();
        assertEquals("Kortilla on rahaa 2.0 euroa", kortti.toString());
    }
}
```
Merkinnällä @before varustettu **setUp() suoritetaan ennen jokaista testitapausta** (eli testimetodia). Jokainen testitapaus siis aloittaa tilanteesta, jossa on luotu kortti jonka saldo on 10. 

### Lisää testejä

Tehdään vielä testi metodille lataaRahaa. Ensimmäinen testi varmistaa, että lataus onnistuu ja toinen testaa, ettei kortin saldo kasva suuremmaksi kuin 150 euroa. 

``` java
    @Test
    public void kortilleVoiLadataRahaa() {
        kortti.lataaRahaa(25);
        assertEquals("Kortilla on rahaa 35.0 euroa", kortti.toString());
    }

    @Test
    public void kortinSaldoEiYlitaMaksimiarvoa() {
        kortti.lataaRahaa(200);
        assertEquals("Kortilla on rahaa 150.0 euroa", kortti.toString());
    }
```

### Testit ovat toisistaan riippumattomia

Yllä jo mainittiin että testit ovat toisistaan riippumattomia eli molemmat testit toimivat siis kuin kaksi erillistä pientä "main":ia. Mitä tämä oikein tarkoittaa?

LyyraKorttia testataan usealla pienellä testimetodilla joista jokaisen alussa on annotaatio @Test. Jokainen erillinen testi testaa yhtä pientä asiaa, esim. että kortin saldo vähenee lounaan hinnan verran. On tarkoituksena, että jokainen testi aloittaa "puhtaalta löydältä", eli ennen jokaista testiä luodaan alustuksen tekevässä setUp-metodissa uusi varasto.

Jokainen testi siis alkaa tilanteesta jossa kortti on juuri luotu. Tämän jälkeen testi joko kutsuu suoraan testattavaa metodia tai ensin saa aikaan sopivan alkutilanteen  ja tämän jälkeen kutsuu testattavaa metodia (näin tapahtui testimetodissa syoEdullisestiEiVieSaldoaNegatiiviseksi, maukkaastisyömisellä saldo väheni 2 euroon jonka jälkeen testattiin ettei edullisestisyöminen vie saldoa negatiiviseksi).

### Onko jo testattu tarpeeksi?

Olemme tyytyväisiä, uskomme että testitapauksia on nyt tarpeeksi. Onko tosiaan näin? Onneksi NetBeansissa on työkalu, jolla voidaan tarkastaa testien lausekattavuus, eli se mitä koodirivejä testien suorittaminen on tutkinut. Tutstumme testien lausekattavuuden mittaamiseen myöhemmin kurssin aikana.

Testikattavuuden mittaus paljastaa että koodi on lähes kattavasti testattu. Ainoa testien tutkimatta jättänyt komento on tilanteessa, jossa kortille yritetään ladata negatiivinen saldo.

### Huomioita

Kortin alkusaldo on kaikissa testeissä 10. Varmempaa olisi testata myös muutamaa muuta alkusaldoa.

Testien kirjoittaminen ohjelmoinnin jälkeen raskasta ja turhauttavaa. Parempi tapa onkin kirjoittaa testejä jo ennen kun itse ohjelma on tehty. Tutustumme tähän **testit ensin** -ohjelmointiin (englanniksi Test Driven Development eli TDD) myöhemmin kurssilla.

**Kerrataan vielä testien toiminnan kannalta erittäin tärkeä asia: testiluokan nimen on päätyttävä sanaan Test**

### Testiluokka vielä kokonaisuudessaan

Testiluokan konstruktori sekä metodit setUpClass (suoritetaan ennen kuin testaus aloitetaan), tearDownClass (suoritetaan testauksen päätyttyä) ja tearDown (suoritetaan jokaisen testin jälkeen) on poistettu sillä testimme ei niitä tarvitse. 

``` java
public class LyyraKorttiTest {

    LyyraKortti kortti;

    @Before
    public void setUp() {
        kortti = new LyyraKortti(10);
    }

    @Test
    public void konstruktoriAsettaaSaldonOikein() {
        assertEquals("Kortilla on rahaa 10.0 euroa", kortti.toString());
    }

    @Test
    public void syoEdullisestiVahentaaSaldoaOikein() {
        kortti.syoEdullisesti();
        assertEquals("Kortilla on rahaa 7.5 euroa", kortti.toString());
    }

    @Test
    public void syoMaukkaastiVahentaaSaldoaOikein() {
        kortti.syoMaukkaasti();
        assertEquals("Kortilla on rahaa 6.0 euroa", kortti.toString());
    }

    @Test
    public void syoEdullisestiEiVieSaldoaNegatiiviseksi() {
        kortti.syoMaukkaasti();
        kortti.syoMaukkaasti();
        kortti.syoEdullisesti();
        assertEquals("Kortilla on rahaa 2.0 euroa", kortti.toString());
    }

    @Test
    public void kortilleVoiLadataRahaa() {
        kortti.lataaRahaa(25);
        assertEquals("Kortilla on rahaa 35.0 euroa", kortti.toString());
    }

    @Test
    public void kortinSaldoEiYlitaMaksimiarvoa() {
        kortti.lataaRahaa(200);
        assertEquals("Kortilla on rahaa 150.0 euroa", kortti.toString());
    }
}

```
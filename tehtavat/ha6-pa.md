# Laskari 6 - Paikanpäällä tehtävät

## Osa 1: AssertJ testejä graafiseen laskimeen

Ohjelmoiniin jatkokurssin viidennellä viikon tehtävässä 165 ohjelmoitiin Swing-käyttöliittymän omaava [laskin](http://www.cs.helsinki.fi/group/java/s15-materiaali/viikko12/#165laskin).

![](https://www.cs.helsinki.fi/group/java/k13/ohja/img-ohja/laskin.png)

Hae [täältä](/koodi/GraafinenLaskin.zip?raw=true) zip-pakattu graafisen laskimen sisältämä NetBeans-projekti. Pura paketti ja avaa projekti NetBeansilla.

Lisätään nyt projektiin testit, jotka testaavat laskimen toimintaa käyttäjän tapaan graafisen käyttöliittymän kautta.

Testaus tapahtuu [AssertJ](http://joel-costigliola.github.io/assertj/assertj-swing.html)-kirjaston avulla. Projektissa on valmiina pari testiä sisältävä testiluokka:

```java
// importteja

public class LaskinTest extends AssertJSwingJUnitTestCase {
    FrameFixture window;

    @Test
    public void tulosAlussaNolla() {
        window.textBox("tulos").requireText("0");    
    }    

    @Test
    public void luvunLisaysNolaanToimii() {
        window.textBox("syote").enterText("10");
        window.button("+").click();
        window.textBox("tulos").requireText("10");    
    }     

    // lisää testejä tänne

    @Override
    protected void onSetUp() {
        // sovellus on Main.java-luokassa
        application(Main.class).start();

        window = findFrame(new GenericTypeMatcher<Frame>(Frame.class) {
            protected boolean isMatching(Frame frame) {
                return frame.isShowing();
            }
        }).using(robot());        
    }   
}

```

Testiluokka poikkeaa hieman normaaleista JUnit-testeistä, sillä testiluokka perii luokan <code>AssertJSwingJUnitTestCase</code>. Avainasemassa testeissä on muuttuja <code>FrameFixture window</code> jolle metodi
<code>protected void onSetUp()</code> asettaa arvon. Muuttujan window avulla testit pääsevät käsiksi graafisen käyttöliittymän komponentteihin.

Jotta testit löytäisivät helposti käyttöliittymän komponentit, on niille annettu ohjelman koodissa _setName_-metodin avulla nimet:

```java
    private void luoKomponentit(Container container) {
        // ...         
        tuloskentta.setName("tulos");
        syotekentta.setName("syote");
        plus.setName("+");
        miinus.setName("-");
        nollaa.setName("Z");
        // ...
}
```

Testeistä ensimmäinen etsii käyttöliittymästä tekstikentän, jolle on annettu nimi _tulos_ ja varmistaa, että siinä on arvo 0:

```java
    @Test
    public void tulosAlussaNolla() {
        window.textBox("tulos").requireText("0");    
    }   
```

Toisin kuin normaaleissa JUnit-testeissä, testin oikeaa tulosta ei varmisteta _assert_-muotoisella komennolla, vaan liittämällä window-muuttujasta 'etsittyyn' tekstikenttän sisällölle vaatimus _requireText_-metodilla.

Toinen esimerkkitesti hakee ensin käyttöliittymästä tekstikentän nimeltään _syote_, kirjoittaa tekstikenttään merkkijonon _10_, etsii painikkeen, jonka nimenä _+_ ja painaa sitä ja lopulta varmistaa, että tekstikenttä _tulos_ saa arvon 10.

```java
    @Test
    public void luvunLisaysNolaanToimii() {
        window.textBox("syote").enterText("10");
        window.button("+").click();
        window.textBox("tulos").requireText("10");    
    }
```

Tee seuraavat testit graafiselle laskimelle:
* luvun vähennys toimii
* usean peräkkäisen laskutoimituksen (lisäyksiä ja vähennyksiä) kombinaatio toimii
* jos syötekentässä on epävalidi arvo (joku muu kuin kokonaisluku) ja painetaan summausnappia
  * syötekenttä tyhjenee
  * tuloskentän arvo säilyy muuttumattomana (oletetaan että tuloskentässä oli joku muu arvo kuin 0)
* nollausnappi ei ole alussa klikattavissa, eli on tilassa disabled
* kun tulos saa jonkun nollasta poikkeavan arvon, nollausnappi klikkaaminen on mahdollista
* nollausnappi klikkaaminen nollaa tuloskentän
* nollausnappi ei ole klikattavissa jos kahden tai useamman laskuoperaation jälkeen tuloskentässä on 0

## Osa 2: Testejä Numerotiedustelulle

Tarkastellaan jälleen ohjelmoinnin jatkokurssilta tuttua  [numerotiedustelua](http://www.cs.helsinki.fi/group/java/s15-materiaali/viikko10/#151numerotiedustelu).

Hae [täältä](/koodi/NumerotiedusteluForTests.zip?raw=true) zip-pakattu, tähän tehtävään sopiva versio numerotiedustelusta. Pura paketti ja avaa projekti NetBeansilla.

Lisätään nyt projektiin testit, jotka testaavat numerotiedustelua simuloiden käyttäjää, eli tekstuaalisen käyttöliittymän kautta.

Projektista löytyy valmiina muutama testi:

```java
public class NumerotiedusteluTest {
    ByteArrayOutputStream tulosvirta;

    @Before
    public void setUp() {
        tulosvirta = new ByteArrayOutputStream();
        System.setOut(new PrintStream(tulosvirta));
    }

    @Test
    public void menuTulostuu() {
	String syote = muodosta("x");

	Numerotiedustelu tiedustelu = new Numerotiedustelu(new Scanner(syote));
        tiedustelu.kaynnista();

        String tulos = tulosvirta.toString();

        assertTrue(tulos.contains("1 lisää numero"));
        assertTrue(tulos.contains("2 hae numerot"));
        assertTrue(tulos.contains("3 hae puhelinnumeroa vastaava henkilö"));
    }

    @Test
    public void tuntemattomanNumeroaEiLoydy() {
	String syote = muodosta("2","arto","x");

	Numerotiedustelu tiedustelu = new Numerotiedustelu(new Scanner(syote));
        tiedustelu.kaynnista();

        String tulos = tulosvirta.toString();

        assertTrue(tulos.contains("ei löytynyt"));
    }        

    private String muodosta(String... lines) {
        String linesWithEnter = "";
        for (String line : lines) {
            linesWithEnter += line + "\n";
        }
        return linesWithEnter;
    }
}

```

Testeissä ohjelman toimivuus varmistetaan tutkimalla että ohjelman tulostusvirtaan, eli konsoliin tekemät tulostukset ovat odotetun kaltaisia. Testien aluksi alustetaan tulostusvirtaolio ja _ohjataan_ metodilla <code>System.setOut</code> ohjelman tulostus tapahtumaan konsolin sijaan testin määrittelemään tulostusvirtaolioon.

```java
    ByteArrayOutputStream tulosvirta;

    @Before
    public void setUp() {
        tulosvirta = new ByteArrayOutputStream();
        System.setOut(new PrintStream(tulosvirta));
    }
```

Testien aluksi määritellään _merkkijonona_ simuloitu käyttäjän antama syöte. Jokainen erillinen käyttäjän antama syöterivi päättyy rivinvaihtoon eli merkkiin _\n_. Tämän jälkeen käynnistetään sovellus. Sovellukselle annetaan konstruktorin parametrina <code>Scanner</code>-olio, joka saa parametrikseen simuloitua syötettä vastaavan merkkijonon. Merkkijono muodostetaan apumetodin <code>muodosta</code> avulla.

```java
    String syote =  muodosta("2","arto","x");

    Numerotiedustelu tiedustelu = new Numerotiedustelu(new Scanner(syote));
    tiedustelu.kaynnista();
```

Apumetodi lisää rivinvaihdon jokaisen parametrinsa väliin, eli merkkijonon <code>syote</code> arvoksi edellisessä tulee <code>2\narto\nx\n</code>.

Nyt Numerotiedustelu-sovelluksen näkökulmasta scanner toimii siten, kuin käyttäjä antaisi ensin syötteeksi _2_, sen jälkeen _arto_ ja lopulta _x_.

Testin lopussa muutetaan _tulosvirta_ merkkijonoksi ja varmistetaan, että ohjelman tulostus on odotetun kaltainen:

```java
    String tulos = tulosvirta.toString();

    assertTrue(tulos.contains("ei löytynyt"));     
```

Käytännössä ohjelman tulostus on yksi iso merkkijono, ja odotettuja tulosrivejä voidaan etsiä ohjelman tuloksen seasta esim. _contains_-operaatiolla.

<code>assertTrue()</code>-muotoisten komentojen antama virheilmoitus ei ole kovin informatiivinen. Voimme onneksi antaa komennolle myös kaksi parametria, joista ensimmäiseksi tulee customoitu virheilmoitus. Jos muuttaisimme edellisen testin seuraavaan muotoon:

```java
    @Test
    public void tuntemattomanNumeroaEiLoydy() {
	String syote = muodosta("2","arto","x");

	Numerotiedustelu tiedustelu = new Numerotiedustelu(new Scanner(syote));
        tiedustelu.kaynnista();

        String tulos = tulosvirta.toString();

        assertTrue("ohjelman tulostus oli: "+tulos, tulos.contains("ei ole"));
    }   
```

saisimme virheilmoituksen, joka sisältää tiedon siitä mitä ohjelma tulosti testin suorituksen aikana, ja näin debuggaus olisi hieman helpompaa:

![](https://www.evernote.com/shard/s417/sh/c28eed29-5ac5-4f11-9592-f83cc15d8def/ff03adc8cbc3b0e5b47ded8d52c14c85/deep/0/NumerotiedusteluForTests---NetBeans-IDE-8.0.png)

Eli virhe oli siinä, että testi oletti ohjelman tulostavan _ei ole_, mutta ohjelma tulosti _ei löytynyt_

Tee nyt Numerotiedustelulle seuraavat tekstuaalisen käyttöliittymän kautta suoritettavat testit:
* jos henkilölle lisätään numero, löydetään numero hakuoperaatiolla
* jos henkilölle on lisätty useita numeroita, löydetään numerot hakuoperaatiolla
* numeroa vastaava henkilö löydetään
* jos numeroa vastaavaa henkilöä ei ole, ohjelma tulostaa _ei löytynyt_
* jos puhelinnumeron omaavalle henkilölle lisätään osoite, tulostuvat molemmat henkilön tiedoissa
* jos henkilöllä on numero mutta ei ole osoitetta, tulostuu henkilön tietoihin _osoite ei tiedossa_
* jos henkilöllä on osoite, mutta ei numeroa, tulostuu henkilön tietoihin _ei puhelinta_
* jos henkilö poistetaan, ei mitään henkilöön liittyvää löydetä _missään_ haussa

**HUOM1:** kaikkien syötteiden tulee päättyä ohjelman lopetuskomentoon _'x'_, eli olla muotoa:

```java
    String syote = muodosta(..., "x");
```

**HUOM2:** kaikkien testien muoto on sama. Ensin alustetaan testaussyöte, sen jälkeen luodaan ja käynnistetään numerotiedostelu, lopuksi tutkitaan, että ohjelman tulostus on oikeanlainen.

```java
    @Test
    public void kaikkiTestitNäin() {
	String syote = muodosta(..., "x");

	Numerotiedustelu tiedustelu = new Numerotiedustelu(new Scanner(syote));
        tiedustelu.kaynnista();

        String tulos = tulosvirta.toString();

        // assertJotain  
    }
```

**HUOM3:**
Varmista tarvittaessa ohjelman eri toimintojen haluttu toiminnallisuus ja tulostusasu
[tehtävämäärittelystä](http://www.cs.helsinki.fi/group/java/s15-materiaali/viikko10/#151numerotiedustelu).


## Osa 3: Lisätoiminnallisuus puhelinmuistiolle

Tee testi myös puhelinmuistion toiminnallisuudelle numero 7 eli _hakusanalla filtteröity listaus (nimen mukaan aakkostettuna), hakusana voi esiintyä henkilön nimessä tai osoitteessa_
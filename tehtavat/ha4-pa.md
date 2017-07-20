# Laskari 4 - Paikanpäällä tehtävät

## 1 Maksukortti ja kassapääte: testit kortille

Ohjelmoinnin perusteiden viikolla [5 tehtävissä 97](http://www.cs.helsinki.fi/group/java/s15-materiaali/viikko5/#97maksukortti_ja_kassapaate)
 toteutettiin "tyhmä" Maksukortti ja Kassapääte.

Rahan käsittely double-tyyppisenä on hiukan ongelmallista. Seuraavassa rahaa käsitellän kokonaislukuna ja kaikki rahamäärät talletetaan eurojen sijasta sentteinä.

Ohjelmointiuransa aloittelevan tuttavasi vastaus seuraavassa:

``` java
public class Maksukortti {

    private int saldo;

    public Maksukortti(int saldo) {
        this.saldo = saldo;
    }

    public int saldo() {
        return saldo;
    }

    public void lataaRahaa(int lisays) {
        this.saldo += lisays;
    }

    public boolean otaRahaa(int maara) {
        if (this.saldo < maara)
            return false;

        this.saldo = this.saldo - maara;
        return true;
    }

    @Override
    public String toString() {
        int euroa = saldo/100;
        int senttia = saldo%100;
        return "saldo: "+euroa+"."+senttia;
    }    
}
```

Kassapäätteelle on lisätty metodit, jotka mahdollistavat myytyjen lounaiden määrän ja kassassa olevan rahamäärän kysymisen, toString()-metodi on poistettu:


``` java
public class Kassapaate {

    private int kassassaRahaa;
    private int edulliset;
    private int maukkaat;

    public Kassapaate() {
        this.kassassaRahaa = 100000;
    }

    public int syoEdullisesti(int maksu) {
        if (maksu >= 240) {
            this.kassassaRahaa = kassassaRahaa + 240;
            ++this.edulliset;
            return maksu - 240;
        } else {
            return maksu;
        }
    }

    public int syoMaukkaasti(int maksu) {
        if (maksu >= 400) {
            this.kassassaRahaa = kassassaRahaa + 400;
            this.maukkaat++;
            return maksu - 400;
        } else {
            return maksu;
        }
    }

    public boolean syoEdullisesti(Maksukortti kortti) {
        if (kortti.saldo() >= 240) {
            kortti.otaRahaa(240);
            this.edulliset++;
            return true;
        } else {
            return false;
        }
    }

    public boolean syoMaukkaasti(Maksukortti kortti) {
        if (kortti.saldo() >= 400) {
            kortti.otaRahaa(400);
            this.maukkaat++;
            return true;
        } else {
            return false;
        }
    }

    public void lataaRahaaKortille(Maksukortti kortti, int summa) {
        if (summa >= 0) {
            kortti.lataaRahaa(summa);
            this.kassassaRahaa += summa;
        } else {
            return;
        }
    }

    public int kassassaRahaa() {
        return kassassaRahaa;
    }

    public int maukkaitaLounaitaMyyty() {
        return maukkaat;
    }

    public int edullisiaLounaitaMyyty() {
        return edulliset;
    }
}

```

Hae [täältä](https://github.com/Ooppa/OTM-kesa17/tree/master/koodi/Unicafe.zip) zip-pakattu projekti, jossa Maksukortti ja Kassapääte ovat valmiina. Pura paketti ja avaa projekti NetBeansilla.

Olemme tähän asti suorittaneet testit NetBeansilla. Kokeillaan nyt miten testit voidaan suorittaa _komentoriviltä_.
* avaa terminaali (joka löytyy avaamalla vasemman yläkulman etsintäikkuna ja kirjoittamalla _terminal_)
* selvitä missä _hakemistossa_ NetBeans-projektisi sijaitsee
  * tämä onnistuu esim. klikkaamalla NB:stä projektin nimeä oikealla hiiren napilla, ja valitsemalla _properties_ ja _sources_:

![](https://github.com/mluukkai/OTM2015/blob/master/kuvat/unicafe1.png)

* anna terminaalissa komento _cd /kopioi/tahan/projektin/hakemisto_
* suorita testit antamalla komento _mvn test_
  * huomaa, että komento _mvn_ tulee antaa aina projektin juurihakemistossa, eli samassa hakemistossa, jossa tiedosto _pom.xml_ sijaitsee


*Tee valmiiseen testiluokkaan MaksukorttiTest* testit jotka testaavat ainakin seuraavia asioita:
* rahan lataaminen kasvattaa saldoa oikein
* rahan ottaminen toimii:
  * saldo vähenee oikein jos rahaa on tarpeeksi
  * saldo ei muutu jos rahaa ei ole tarpeeksi
  * metodi palauttaa true jos rahat riittivät ja muuten false



## 2 Testauskattavuus

Olemme tyytyväisiä, uskomme että testitapauksia on nyt tarpeeksi. Onko tosiaan näin? Onneksi  on olemassa työkaluja, joilla voidaan tarkastaa testien lause- ja haarautumakattavuun. __Lausekattavuus__ mittaa mitä koodirivejä testien suorittaminen on tutkinut. Täydellinen lausekattavuuskaan ei tietenkään takaa että ohjelma toimii oikein, mutta on parempi kuin ei mitään. __Haarautumakattavuus__ taas mittaa mitä eri suoritushaarjoa koodista on käyty läpi. Suoritushaaroilla tarkoitetaan esim. if-komentijen valintatilanteita.

Projektitiedostoon on valmiiksi konfiguroitu käytettäväksi [Cobertura](http://cobertura.github.io/cobertura/) joka mittaa sekä lause- että haarautumakattavuuden.

Kattavuuden mittaus suoritetaan klikkaamalla NB:stä projektin kohdalla oikeaa nappia ja valitsemalla __custom/cobertura__

Kokeile myös miten cobertura suoritetaan antamalla komentoriviltä (projektihakemistossa ollessasi) komento <code>mvn cobertura:cobertura</code>

Tulokset tulevat projektihakemistosi alihakemistoon __target/site/cobertura/index.html__. Avaa tulokset web-selaimella. Firefoxilla tämä tapahtuu komennolla __open file__. Voit myös avata selaimen terminaalissa menemällä ensin projektihakemistoon ja antamalla komento __chromium-browser target/site/cobertura/index.html__

**Jos maksukortin koodissa on vielä rivejä tai haarautumia (merkitty punaisella) joille ei ole testiä, kirjoita sopivat testit.**

## 3 Testien testaaminen

Pelkkä koodirivien kattaminen testeillä ei riitä.

On mahdollista kirjoittaa testejä jotka "koskevat" kaikkia koodirivejä mutta eivät testaa oikein mitän järkevää. Pelkän suuren kattavuuden tavoittelun lisäksi on siis tärkeä muistaa testata:

* normaali toimina
* poikkeuksellinen toiminta
* virhetilanteet

Erityinen huomio kannattaa kiinnittää ns. raja-arvoihin:

* toimiiko kortilla ostaminen jos rahaa on jäljellä täsmälleen lounaan hinnan verran?

Yksi tapa testien hyvyyden testaamiseen on __mutaatiotestaus__. Testaamme seuraavaksi testiemme hyvyyttä [PIT-työkalun](http://pitest.org/) avulla.

Seuraavassa PIT:in sivulta otettu selitys joka kertoo mistä mutaatiotestauksessa on kyse:

> Mutation testing is conceptually quite simple. Faults (or mutations) are automatically seeded into your code, then your tests are run. If your tests fail then the mutation is killed, if your tests pass then the mutation lived.

> The quality of your tests can be gauged from the percentage of mutations killed.

> To put it another way - PIT runs your unit tests against automatically modified versions of your application code. When the application code changes, it should produce different results and cause the unit tests to fail. If a unit test does not fail in this situation, it may indicate an issue with the test suite.

Mutaatiotestaa ohjelmasi *suorittamalla ensin testit* ja sen jälkeen klikkaamalla NB:stä projektin kohdalla oikeaa nappia ja valitsemalla __custom/pit__

tai vaihtoehtoisesti ollessasi projektihakemistossa antamalla komentoriviltä komennot <code> mvn test</code> ja <code>mvn org.pitest:pitest-maven:mutationCoverage</code>

Edellinen onnistuu myös yhdellä komennolla <code>mvn test org.pitest:pitest-maven:mutationCoverage</code>

Tulokset tulevat projektihakemistosi alihakemistoon __target/pit-reports/aikaleima/index.html__. Aikaleima on numerosarja joka kertoo suoritushetken, tyyliin 201511181742 (testi ajettu 18.11.2015 klo 1742). Avaa tulokset web-selaimella. Firefoxilla tämä tapahtuu komennolla __open file__. Voit myös avata selaimen terminaalissa menemällä ensin projektihakemistoon ja antamalla komento __chromium-browser target/pit-reports/aikaleima/index.html__. Kun ajat mutaatiotestit uudelleen, muista avat oikea raportti!

Voit poistaa vanhat raportit painamalla vasara+harja-symbolia (clean and build) tai komentoriviltä komennolla <code>mvn clean</code>

Raportista näet mitkä mutantit jäävät henkiin.

**Yritä parantaa testejäsi siten, että lausekattavuus säilyy ja kaikki mutantit kuolevat.**

## 4 Kassapäätteen testit

Laajennetaan Unicafen testaus kattamaan myös kassapääte.

*Tee testiluokka KassapaateTest (samaan pakkaukseen, missä MaksukortiTest jo on!) ja tee testit jotka testaavat ainakin seuraavia asioita:*
* luodun kassapäätteen rahamäärä ja myytyjen lounaiden määrä on oikea (rahaa 1000, lounaita myyty 0)
* käteisosto toimii sekä edullisten että maukkaiden lounaiden osalta
  * jos maksu riittävä: kassassa oleva rahamäärä kasvaa lounaan hinnalla ja vaihtorahan suuruus on oikea
  * jos maksu on riittävä: myytyjen lounaiden määrä kasvaa
  * jos maksu ei ole riittävä: kassassa oleva rahamäärä ei muutu, kaikki rahat palautetaan vaihtorahana ja myytyjen lounaiden määrässä ei muutosta
* _seuraavissa testeissä tarvitaan myös Lyyrakorttia jonka oletetaan toimivan oikein_
* korttiosto toimii sekä edullisten että maukkaiden lounaiden osalta
  * jos kortilla on tarpeeksi rahaa, velotetan summa kortilta ja palautetaan true
  * jos kortilla on tarpeeksi rahaa, myytyjen lounaiden määrä kasvaa
  * jos kortilla ei ole tarpeeksi rahaa, kortin rahamäärä ei muutu, myytyjen lounaiden määrä muuttumaton ja palautetaan false
  * kassassa oleva rahamäärä ei muutu kortilla ostettaessa
* kortille rahaa ladattaessa kortin saldo muuttuu ja kassassa oleva rahamäärä kasvaa ladatulla summalla

Huomaat että kassapääte sisältää melkoisen määrän "copypastea". Nyt kun kassapäätteellä on automaattiset testit on sen rakennetta helppo muokata eli refaktoroida siistimmäksi koko ajan kuitenkin varmistaen että testit menevät läpi. Tulemme tekemään refaktoroinnin seuraavalla viikolla.

## 5 100% lause- ja haarautumakattavuus

Varmista että kassapäätteen teksteillä on 100% lause- ja haarautumakattavuus ja että mutantteja ei jää henkiin. Negatiivisen saldon lataamiseen liittyvää mutanttia on mahdoton tappaa. Tämä johtuu siitä, että nollan lataaminen ei aiheuta saldoon muutosta, joten vaikka ehdon muuttaisi negaatioksi, nollan lataaminen toimii edelleen.

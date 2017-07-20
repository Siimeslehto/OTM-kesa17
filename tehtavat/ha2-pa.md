# Laskari 2 - Paikanpäällä tehtävät

## Tehtävä 1

* tutustu [JUnit-ohjeeseen](/tehtavat/ohjeet/JUnit-ohje.md), lukiessasi tee testit myös itse
* lisää lopuksi lyyrakortille seuraavat testit:
  * maukkaan lounaan syöminen ei vie saldoa negatiiviseksi, ota tähän mallia testistä syoEdullisestiEiVieSaldoaNegatiiviseksi
  * negatiivisen summan lataaminen ei muuta kortin saldoa
  * kortilla pystyy ostamaan edullisen lounaan kun kortilla rahaa vain edullisen lounaan verran (eli 2.5e)
  * kortilla pystyy ostamaan maukkaan lounaan kun kortilla rahaa vain maukkaan lounaan verran (eli 4e)

**HUOM1** on suositeltavaa, että yksi testi testaa vaan "yhtä asiaa" kerrallaan. Tee siis jokaisesta ylläolevasta oma testinsä.

**HUOM2** Kirjoita assertEquals-komennot aina siten, että ensimmäisenä parametrina on odotettu tulos ja toisena parametrina testatun metodin antama tulos.

## Tehtävä 2

* tutustu [debuggeriohjeeseen](/tehtavat/ohjeet/Debuggeri.md)
* lukiessasi ohjetta, kokeile kaikkia esimerkkejä koneellasi

## Tehtävä 3

Ohjelmoinnin perusteiden eräässä tehtävässä (ks. [tehtävänanto](http://www.cs.helsinki.fi/group/java/s15-materiaali/viikko4/#89kello_laskurin_avulla) ) ohjelmoitiin luokka YlhaaltaRajoitettuLaskuri:

``` java
public class YlhaaltaRajoitettuLaskuri {

    private int arvo;
    private int ylaraja;

    public YlhaaltaRajoitettuLaskuri(int ylarajanAlkuarvo) {
        this.ylaraja = ylarajanAlkuarvo;
        this.arvo = 0;
    }

    public void seuraava() {
        if (this.arvo == this.ylaraja) {
            this.arvo = 0;
        } else {
            this.arvo++;
        }
    }

    public int arvo() {
        return this.arvo;
    }

    public void asetaArvo(int uusiArvo) {
        if (uusiArvo < 0 || uusiArvo > this.ylaraja) {
            return;
        }

        this.arvo = uusiArvo;
    }

    @Override
    public String toString() {
        String etunolla = "0";
        if (this.arvo > 9) {
            etunolla = "";
        }
        return etunolla + this.arvo;
    }
}
```

Tee laskurille seuraavat testit:
* luodun laskurin alkuarvo on 0
* kun laskuri etenee kerran, sen arvo on 1
* kun laskuri etenee kaksi kertaa, sen arvo on 2
* jos laskurin yläraja on n ja laskuri etenee n kertaa, on laskurin arvo n
  * korvaa testeissä n jollain konkreettisella arvolla, esim. 4
* jos laskurin yläraja on n ja laskuri etenee n+1 kertaa, on laskurin arvo 0
* jos laskurin yläraja on n ja laskuri etenee n+2 kertaa, on laskurin arvo 1
* metodi asetaArvo asettaa laskurin arvon oikein jos parametrin arvo on välillä 0 - laskurin yläraja
* metodi asetaArvo ei tee mitään jos parametrin arvo ei ole välillä 0 - laskurin yläraja
* toString tuottaa etunollan jos laskurin arvo on alle 10
* toString ei tuota etunollaa jos laskurin arvo on vähintään 10

**HUOM** on suositeltavaa, että yksi testi testaa vaan "yhtä asiaa" kerrallaan. Tee siis jokaisesta ylläolevasta oma testinsä.

## Tehtävä 4

Seuraavassa on ylhäältä rajoitetettujen laskureiden avulla muodostettu kello, joka käynnistyy ajassa 23:50:00:

``` java

public class Paaohjelma {

    public static void main(String[] args) {

        YlhaaltaRajoitettuLaskuri sekunnit = new YlhaaltaRajoitettuLaskuri(59);
        YlhaaltaRajoitettuLaskuri minuutit = new YlhaaltaRajoitettuLaskuri(59);
        YlhaaltaRajoitettuLaskuri tunnit = new YlhaaltaRajoitettuLaskuri(23);

        tunnit.asetaArvo(23);
        minuutit.asetaArvo(50);       

        int i = 0;
        while (i < 602) {
            System.out.println(tunnit + ":" + minuutit + ":" +sekunnit);
            sekunnit.seuraava();
            if (sekunnit.arvo() == 0) {
                minuutit.seuraava();
                if (minuutit.arvo()==0 ) {
                    tunnit.seuraava();
                }
            }

            i++;
        }
    }
}
```
Seuraa debuggerin avulla kellon etenemistä, ensin aikaan 23:51:00 ja sitten vuorokauden vaihtumista aikaan 00:00:00. Askelia puoleen yöhön on niin monta, että etenemistä kannattaa nopeuttaa debuggeriohjeen kohdan "useita breakpointeja ja suorituksen jatkaminen" tyyliin.

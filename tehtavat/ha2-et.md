# Laskari 2 - Kotitehtävät

## Tehtävä 1

Mallinna seuraavan Kellon Java-koodi luokkakaaviona.

``` java
public class Kello {
    private YlhaaltaRajoitettuLaskuri tunnit;
    private YlhaaltaRajoitettuLaskuri minuutit;
    private YlhaaltaRajoitettuLaskuri sekunnit;

    public Kello(int tunnitAlussa, int minuutitAlussa, int sekunnitAlussa) {
        // luodaan kello joka asetetaan parametrina annettuun aikaan
        tunnit = new YlhaaltaRajoitettuLaskuri(23);
        minuutit = new YlhaaltaRajoitettuLaskuri(59);
        sekunnit = new YlhaaltaRajoitettuLaskuri(59);

        tunnit.asetaArvo(tunnitAlussa);
        minuutit.asetaArvo(minuutitAlussa);
        sekunnit.asetaArvo(sekunnitAlussa);
    }

    public void etene() {
        // kello etenee sekunnilla
        sekunnit.seuraava();
        if (sekunnit.arvo() == 0) {
            minuutit.seuraava();
            if (minuutit.arvo() == 0) {
                tunnit.seuraava();
            }
        }
    }

    public String toString() {
        return tunnit.toString() + ":" + minuutit.toString() + ":" + sekunnit.toString();
    }
}

public class YlhaaltaRajoitettuLaskuri {
    private int arvo;
    private int ylaraja;

    public YlhaaltaRajoitettuLaskuri(int ylaraja) {
        this.ylaraja = ylaraja;
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

Tee luokkakaavio tarkasti (merkitse kaikki metodit ja attribuutit). Jatkossa riittää että luokkakaavioon merkataan kulloisenkin käyttötilanteen suhteen tärkeimmät metodit/attribuutit. Yleensä ei merkata kuin korkeintaan julkiset metodit ja attribuutit, ei aina niitäkään.

## Tehtävä 2

Mallinna Ohjelmoinnin jatkokurssin tehtävän 129 [Lastiruuma](http://www.cs.helsinki.fi/group/java/s15-materiaali/viikko8/#129tavara_matkalaukku_ja_lastiruuma) koodi luokkakaaviona.

``` java

public class Tavara {
    private String nimi;
    private int paino;

    public Tavara(String nimi, int paino) {
        this.nimi = nimi;
        this.paino = paino;
    }

    public String getNimi() {
        return this.nimi;
    }

    public int getPaino() {
        return this.paino;
    }

    @Override
    public String toString() {
        return this.nimi + ": (" + this.paino + " kg)";
    }
}


public class Matkalaukku {
    private int maksimipaino;
    private ArrayList<Tavara> tavarat;

    public Matkalaukku(int maksimipaino) {
        this.tavarat = new ArrayList<Tavara>();
        this.maksimipaino = maksimipaino;
    }

    public void lisaaTavara(Tavara tavara) {
        if (this.yhteispaino() + tavara.getPaino() > this.maksimipaino) {
            return;
        }

        this.tavarat.add(tavara);
    }

    public int yhteispaino() {
        int paino = 0;
        for (Tavara tavara : this.tavarat) {
            paino += tavara.getPaino();
        }

        return paino;
    }

    public void tulostaTavarat() {
        for (Tavara tavara : this.tavarat) {
            System.out.println(tavara);
        }
    }

    public Tavara raskainTavara() {
        Tavara raskain = null;

        for (Tavara tavara : this.tavarat) {
            if (raskain == null || raskain.getPaino() < tavara.getPaino()) {
                raskain = tavara;
            }
        }

        return raskain;
    }

    @Override
    public String toString() {
        if (this.tavarat.isEmpty()) {
            return "ei tavaroita (0 kg)";
        }

        if (this.tavarat.size() == 1) {
            return "1 tavara (" + this.yhteispaino() + " kg)";
        }

        return this.tavarat.size() + " tavaraa (" + this.yhteispaino() + " kg)";
    }
}


public class Lastiruuma {
    private ArrayList<Matkalaukku> matkalaukut;
    private int maksimipaino;

    public Lastiruuma(int maksimipaino) {
        this.maksimipaino = maksimipaino;
        this.matkalaukut = new ArrayList<Matkalaukku>();
    }

    public void lisaaMatkalaukku(Matkalaukku matkalaukku) {
        if (this.yhteispaino() + matkalaukku.yhteispaino() > maksimipaino) {
            return;
        }

        this.matkalaukut.add(matkalaukku);
    }

    public int yhteispaino() {
        int paino = 0;
        for (Matkalaukku laukku : this.matkalaukut) {
            paino += laukku.yhteispaino();
        }

        return paino;
    }

    public void tulostaTavarat() {
        for (Matkalaukku laukku : this.matkalaukut) {
            laukku.tulostaTavarat();
        }
    }

    @Override
    public String toString() {
        if (this.matkalaukut.isEmpty()) {
            return "ei matkalaukkuja (0 kg)";
        }

        if (this.matkalaukut.size() == 1) {
            return "1 matkalaukku (" + this.yhteispaino() + " kg)";
        }

        return this.matkalaukut.size() + " matkalaukkua (" + this.yhteispaino() + " kg)";
    }
}

```

## Tehtävä 3

Piirrä allaolevaa koodia vastaava luokkakaavio. Metodeja ja attribuutteja ei tarvitse merkitä kaavioon.

``` java
public class Kioski {
    public Matkakortti ostaMatkakortti(String nimi) {
        Matkakortti uusiKortti = new Matkakortti(nimi);                        
        return uusiKortti;
    }

    public Matkakortti ostaMatkakortti(String nimi, int arvo) {
        Matkakortti uusiKortti = new Matkakortti(nimi);                      
        uusiKortti.kasvataArvoa(arvo);
        return uusiKortti;
    }       
}


public class Matkakortti {
    private String omistaja;
    private double arvo;
    private int pvm;
    private int kk;

    public Matkakortti(String n){
        omistaja = n;  pvm = 0;  kk = 0;  arvo = 0;
    }         

    public void kasvataArvoa(double a){ arvo += a; }

    public void vahennaArvoa(double a){ arvo -= a; }

    public double getArvo(){ return arvo; }   

    public void uusiAika(int p, int k){
        kk = k;
        pvm = p;
    }    
}


public class Lataajalaite {
    public void lataaArvoa(Matkakortti k, double a) {
        k.kasvataArvoa(a);
    }

    public void lataaAikaa(Matkakortti k, int pvm, int kk) {
        k.uusiAika(pvm, kk);
    }
}



public class Lukijalaite {
    private double RATIKKA = 1.5;
    private double HKL = 2.1;
    private double SEUTU = 3.5;

    public boolean ostaLippu(Matkakortti k, int tyyppi){
        double hinta = 0;
        if ( tyyppi == 0 ) hinta = RATIKKA;
        else if ( tyyppi ==1 ) hinta = HKL;
        else hinta = SEUTU;

        if ( k.getArvo()<hinta ) return false;
        k.vahennaArvoa(hinta);

        return true;
    }     
}

public class HKLLaitehallinto {

    private ArrayList<Lataajalaite> lataajat;
    private ArrayList<Lukijalaite> lukijat;

    public HKLLaitehallinto() {
        lataajat = new ArrayList<>();
        lukijat = new ArrayList<>();
    }

    public void lisaaLataaja(Lataajalaite lataaja) {
        lataajat.add(lataaja);
    }

    public void lisaaLukija(Lukijalaite lukija) {
        lukijat.add(lukija);
    }
}

public class Main {
    public static void main(String[] args) {
        HKLLaitehallinto laitehallinto = new HKLLaitehallinto();

        Lataajalaite rautatietori = new Lataajalaite();
        Lukijalaite ratikka6 = new Lukijalaite();
        Lukijalaite bussi244 = new Lukijalaite();   

        laitehallinto.lisaaLataaja(rautatietori);
        laitehallinto.lisaaLukija(ratikka6);
        laitehallinto.lisaaLukija(bussi244);

        Kioski lippuLuukku = new Kioski();
        Matkakortti artonKortti = lippuLuukku.ostaMatkakortti("Arto");

        rautatietori.lataaArvoa(artonKortti, 3);

        ratikka6.ostaLippu(artonKortti, 0);
        bussi244.ostaLippu(artonKortti, 2);   
    }
}
```

## Tehtävä 4

Piirrä edellisen tehtävän luokkakaavion mukainen oliokaavio, joka kuvaa main():in viimeisellä rivillä olevan tilanteen.

## Tehtävä 5

Tee mahdollisimman kattavat testit Ohjelmoinnin perusteiden tehtävän 90 luokalle [Lukutilasto](http://www.cs.helsinki.fi/group/java/s15-materiaali/viikko4/#90lukutilasto)

Yksikkötestien kirjoittamiseen löytyy apua luentokalvoista ja [JUnit-ohjeesta](/tehtavat/ohjeet/JUnit-ohje.md).

Lukutilaston koodi:

``` java
public class Lukutilasto {
    // alla muutaman metodin runko valmiina   

    private int lukujenMaara;
    private int summa;

    public Lukutilasto() {
        this.lukujenMaara = 0;
        this.summa = 0;
    }

    public void lisaaLuku(int luku) {
        this.lukujenMaara++;
        this.summa += luku;
    }

    public int haeLukujenMaara() {
        return this.lukujenMaara;
    }

    public int summa() {
        return this.summa;
    }

    public double keskiarvo() {
        if (this.lukujenMaara == 0) {
            return 0;
        }

        return 1.0 * this.summa / this.lukujenMaara;
    }
}
```
**Huom:** double-tyyppisiä lukuja vertaillessa tulee assertEquals-komennosta käyttää kolmeparametrista versiota:

``` java
public void testi() {
    double tulos = testattavaMetodi();
    double odotettu = 1,25;

    assertEquals(odotettu, tulos, 0.001);
}
```
Kolmantena parametrina assertEquals-komentoon annetaan mittaustarkkuus. Syy tälle on doublella laskemisessa syntyvät pyöristysvirheet.

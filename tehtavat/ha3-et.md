# Laskari 3 - Kotitehtävät

## Tehtävä 1

Monopoli ks. esim. [http://fi.wikipedia.org/wiki/Monopoli_(peli)](http://fi.wikipedia.org/wiki/Monopoli_(peli)) on varmasti kaikkien tuntema lautapeli. Tehdään alustava luokkakaavio, joka kuvaa peliä. Kaaviota tarkennetaan ehkä myöhemmin. Tässä vaiheessa kyseessä voisi olla oikeastaan minkä tahansa nopalla pelattavan lautapelin karkean tason luokkakaavio. Oikeassa monopolissahan on rahaa, taloja, hotelleja ym, mutta unohdetaan ne nyt.

Pelin kuvaus siinä tarkkuudessa, mikä meitä nyt kiinnostaa, on seuraavassa:

Monopolia pelataan käyttäen kahta noppaa. Pelaajia on vähintään 2 ja enintään 8. Peliä pelataan pelilaudalla joita on yksi. Pelilauta sisältää 40 ruutua. kukin ruutu tietää, mikä on sitä seuraava ruutu pelilaudalla. Kullakin pelaajalla on yksi pelinappula. Pelinappula sijaitsee aina yhdessä ruudussa. (Pelinappula siis etenee ruudusta toiseen pelin kuluessa.)

Voit olettaa, että luokat ovat pelilauta, ruutu, pelinappula, pelaaja, noppa ja itse kokonaisuutta mallintava monopoliPeli.

Huomaa että isompia luokkamalleja kannattaa tehdä pala kerrallaan. Piirrä ensin vaikkapa kaikki luokat paperille. Mieti yhteyksiä esim. kahden luokan välillä kerrallaan. Jos aikaansaannos ei ole hyvä, heitä paperi roskiin ja aloita alusta. Älä vaivu epätoivoon.

## Tehtävä 2

Tarkastellaan seuraavaa yliopiston kursseihin, niiden esitietovaatimuksiin, kurssitoteutuksiin sekä opettajiin liittyvä tilannetta.

Kurssilla voi olla esitietovaatimuksina useampi muu kurssi. Välttämättä esitietovaatimuksia ei ole. Kurssilla on opintopistemäärä ja nimi. Kurssin (esim. Ohjelmoinnin jatkokurssi) tiettyä luennointikertaa (esim. ohja-syksy15, ohja-syksy16, . . . ) sanotaan kurssitoteutukseksi. Kurssitoteutukseen kuuluu 1 tai 2 koetta, kokeilla on aika ja paikka. Kurssitoteutuksella on alkamis- ja päättymisaika, luentoajat ja luentopaikka. Kurssitoteutus sisältää harjoitusryhmiä. Harjoitusryhmällä on kokoontumisaika ja paikka. Jokaisella ryhmällä on ohjaajana yksi tai useampi henkilökunnan jäsen. Myös kurssitoteutuksen luennoijana on yksi henkilökunnan jäsen. Henkilökunnan jäsen voi olla usean ryhmän ohjaajana tai usean kurssitoteutuksen luennoijana.

Mallinna tilanne luokkakaaviona. Voit olettaa, että luokat ovat kurssi, kurssitoteutus, koe, harjoitusryhmä ja henkilökunnan jäsen. Muut tekstissä mainitut käsitteet eivät siis ole luokkia vaan yhteyksiä, yhteyden rooleja tai attribuutteja.

## Tehtävä 3

Piirrä edellisen tehtävän luokkakaavion mukainen oliokaavio, joka kuvaa tilanteen kurssien Ohjelmoinnin perusteet, Ohjelmistotekniikan menetelmät ja Johdatus tietojenkäsittelytieteeseen sekä niiden tämän syksyn toteutusten osalta. Kaikkia laskariryhmiä ja opettajia ei tarvitse oliokaaviossa huomioida.

## Tehtävä 4

Tarkastellaan [edelliseltä harjoituskerralta](ha2-et.md) tuttua matkakorttiohjelmaa. Tällä kertaa tehtävänä on kuvata *sekvenssikaaviona* main-metodin suorituksen aikaansaama toiminnallisuus.

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
    String omistaja;
    double arvo;
    int pvm;
    int kk;

    public Matkakortti(String n){
        omistaja = n;  
        pvm = 0;  
        kk = 0;  
        arvo = 0;
    }         

    public void kasvataArvoa(double a){
        arvo += a;
    }

    public void vahennaArvoa(double a){
        arvo -= a;
    }

    public double getArvo(){
        return arvo;
    }   

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
        if ( tyyppi == 0 ) {
            hinta = RATIKKA;
        } else if ( tyyppi ==1 ) {
            hinta = HKL;
        } else {
            hinta = SEUTU;
        }                        

        if ( k.getArvo()<hinta ) {
            return false;
        }
        k.vahennaArvoa(hinta);

        return true;
    }     
}

public class HKLLaitehallinto {
        ArrayList<Lataajalaite> lataajat = new ArrayList();
        ArrayList<Lukijalaite> lukijat = new ArrayList();

        lisaaLataaja(Lataajalaite lataaja){ lataajat.add(lataaja); }

        lisaaLukija(Lukijalaite lukija){ lukijat.add(lukija); }
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

**Muista:** sekvenssikaaviossa tulee tulla ilmi kaikki mainin suorituksen aikaansaamat olioiden luomiset ja metodien kutsut!

## Tehtävä 5

[Viime harjoituskerralla](ha2-et.md) teimme luokkakaavion ohjelmoinnin jatkokurssin ensimmäisen viikon tehtävästä “Tavara, Matkalaukku ja Lastiruuma”. Jatketaan saman ohjelman mallintamista. Kuvaa *sekvenssikaaviona* mitä ohjelmassa tapahtuu kun alla olevan main-metodi suoritetaan:

``` java

public class Main {
    public static void main(String[] args) {
        Tavara kirja = new Tavara("Aapiskukko", 2);
        Tavara puhelin = new Tavara("Nokia 3210", 1);
        Tavara tiiliskivi = new Tavara("Tiiliskivi", 4);

        Matkalaukku matinLaukku = new Matkalaukku(10);
        matinLaukku.lisaaTavara(kirja);
        matinLaukku.lisaaTavara(puhelin);

        Matkalaukku pekanLaukku = new Matkalaukku(10);
        pekanLaukku.lisaaTavara(tiiliskivi);

        Lastiruuma lastiruuma = new Lastiruuma(1000);
        lastiruuma.lisaaMatkalaukku(matinLaukku);
        lastiruuma.lisaaMatkalaukku(pekanLaukku);

        System.out.println("Ruuman matkalaukuissa on seuraavat tavarat:");
        lastiruuma.tulostaTavarat();
    }
}
```

Muista, että sekvenssikaaviossa tulee tulla ilmi kaikki mainin suorituksen aikaansaamat olioiden luomiset ja metodien kutsut!

## Tehtävä 6

Seuraavassa on hieman modifioitu versio Ohjelmoinnin perusteiden tehtävästä 102 [Joukkueet ja pelaajat](http://www.cs.helsinki.fi/group/java/s15-materiaali/viikko5/#102joukkueet_ja_pelaajat). Tee kattavat testit luokalle Joukkue. Testit voivat olettaa, että luokka Pelaaja toimii oikein.

Jos et olle vielä tehnyt yhtään testaamiseen liittyvää tehtävää, katso ohjeita testien kirjoittamiseen [täältä](/tehtavat/ohjeet/JUnit-ohje.md).

``` java
import java.util.ArrayList;

public class Joukkue {
    private String nimi;
    private ArrayList<Pelaaja> pelaajat;
    private int maksimikoko;

    public Joukkue(String nimi) {
        this.nimi = nimi;
        this.pelaajat = new ArrayList<Pelaaja>();
        this.maksimikoko = 5;
    }

    public String haeNimi() {
        return this.nimi;
    }

    public void lisaaPelaaja(Pelaaja pelaaja) {
        if (this.koko() >= this.maksimikoko) {
            return;
        }

        this.pelaajat.add(pelaaja);
    }

    public ArrayList<String> pelaajatMerkkijonolistana() {
        ArrayList<String> strings = new ArrayList<>();       

        for (Pelaaja pelaaja : this.pelaajat) {
            strings.add(pelaaja.toString());
        }

        return strings;
    }

    public void asetaMaksimikoko(int maksimikoko) {
        this.maksimikoko = maksimikoko;
    }

    public int koko() {
        return this.pelaajat.size();
    }

    public int maalit() {
        int maaleja = 0;
        for (Pelaaja pelaaja : this.pelaajat) {
            maaleja += pelaaja.maalit();
        }

        return maaleja;
    }
}
```

``` java
public class Pelaaja {
    private String nimi;
    private int maalit;

    public Pelaaja(String nimi) {
        this(nimi, 0);
    }

    public Pelaaja(String nimi, int maalit) {
        this.nimi = nimi;
        this.maalit = maalit;
    }

    public int maalit() {
        return this.maalit;
    }

    public String haeNimi() {
        return this.nimi;
    }

    @Override
    public String toString() {
        return this.nimi + ", maaleja " + this.maalit;
    }
}
```

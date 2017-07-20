# Laskari 4 - Kotitehtävät

## Tehtävä 1

Takaisinmallinna seuraava koodi luokkakaaviona:

``` java

public class Paaohjelma {
    public static void main(String[] args) {
        Kurssi otm = new Kurssi();
        Opiskelija o1 = new Opiskelija("Arto", 45);
        Opiskelija o2 = new Opiskelija("Matti", 27);
        Opiskelija o3 = new Opiskelija("Maija", 55);

        otm.lisaaOpiskelija(o1);
        otm.lisaaOpiskelija(o2);
        otm.lisaaOpiskelija(o3);

        otm.arvosteleOpiskelijat();
        otm.tulostaKurssinTulokset();
    }
}

public class ArvosanaLaskin {
    public int selvitaArvosana(int pisteet) {
        if ( pisteet<30 )      return 0;
        else if ( pisteet<35 ) return 1;
        else if ( pisteet<40 ) return 2;
        else if ( pisteet<45 ) return 3;
        else if ( pisteet<50 ) return 4;

        return 5;
    }
}

public class Kurssi {
    private ArrayList<Opiskelija> opiskelijat;
    private ArvosanaLaskin arvosanaLaskin;

    public Kurssi(){
        opiskelijat = new ArrayList<Opiskelija>();
        arvosanaLaskin = new ArvosanaLaskin();
    }

    public void lisaaOpiskelija(Opiskelija opisk){
        opiskelijat.add(opisk);
    }

    public void arvosteleOpiskelijat(){
        for ( Opiskelija opisk : opiskelijat ) {
            int pisteet = opisk.getPisteet();
            int arvosana = arvosanaLaskin.selvitaArvosana( pisteet );
            opisk.setArvosana(arvosana);
        }
    }

    public void tulostaKurssinTulokset(){
        for ( Opiskelija opisk : opiskelijat )
            opisk.tulostaTiedot();
    }
}

public class Opiskelija {
    private String nimi;
    private int pisteet;
    private int arvosana;

    public Opiskelija(String nimi, int pisteet) {
        this.nimi = nimi;
        this.pisteet = pisteet;
    }

    public int getPisteet(){
        return pisteet;
    }

    public void setArvosana(int arvosana){
        this.arvosana = arvosana;
    }

    public void tulostaTiedot(){
        System.out.println( nimi+" "+arvosana );
    }
}
```

## Tehtävä 2

Piirrä edellisen tehtävän koodia vastaava __oliokaavio__ rivin _otm.lisaaOpiskelija(o2);_ jälkeisestä tilanteesta.

## Tehtävä 3

Takaisinmallinna tehtävän 1 koodi sekvenssikaaviona. Sekvenssikaavio piirretään tilanteesta, jossa kutsutaan luokan Paaohjelma main-metodia.

## Tehtävä 4

[Ensimmäisissä laskareissa](ha1-et.md) oli tehtävänä laatia käyttötapausmalli Ohjelmistotuotantoprojektin hallinnointia tukevaan järjestelmään.

Tee järjestelmää vastaava määrittelyvaiheen luokkakaavio.

Etene samaan tapaan kuin luennon elokuvateatteri- ja kampaamoesimerkeissä, eli etsi ensin luokkakandidaatit keräämällä viikon 2 laskareissa olevassa tehtäväkuvauksessa esiintyvät substantiivit. Karsi sen jälkeen ylimääräiset luokkakandidaatit, eli attribuutit, synonyymit, tekemistä tarkoittavat sanat ja asiaan kuulumattomat. Tämän jälkeen mieti minkälaisia yhteyksiä luokkien olioilla on. Lopuksi tarkenna yhteydet osallistumisrajoitteilla sekä nimeä yhteydet ja roolit tarvittaessa.

Huomioi seuraava:

> yleensä ei jakseta kerätä kaikkia substantiiveja vaan tehdään esim. synonyymien ja tekemistä tarkoittavien sanojen karsinta samalla kun "kunnollisten" substantiivien etsintä etenee

Muista keskittyä olioiden välisiin pysyviin yhteyksiin. **Älä yritä sisällyttää toiminnallisuutta luokkakaavioon** (vrt luennon 3 kalvo 26 ja luennon 4 kalvo 22).

## Tehtävä 5

[AppsForFinland-kilpailuun](http://www.apps4finland.fi) pari vuotta sitten osallistuneen [STOPPI-sovelluksen](http://www.apps4finland.fi/kilpailutyo/stoppi/) toista päätoiminnallisuutta kuvaa seuraava vapaamuotoisena tekstinä kirjoitettu käyttötapaus:

> Matkustaja menee pysäkille, jolta haluaa päästä kyytiin tiettyyn bussiin. Matkustaja käynnistää sovelluksen matkapuhelimessaan. Puhelin tunnistaa pysäkin GPS-paikannusjärjestelmän avulla. Puhelin ilmoittaa sijaintinsa HSL:n verkkopalveluun, joka palauttaa puhelimelle listan pysäkille lähiaikoina saapuvista busseista. HSL:n verkkopalvelu siis tietää kaikkien bussien sijainnin, bussien säännöllisesti lähettämien GPS-paikannustietojen perusteella. Matkustaja valitsee puhelimen näytöllä näytetyistä busseista haluamansa ja painaa pysäytyspyyntönappia. HSL:n verkkopalvelu välittää pysäytyspyynnön bussille ja kertoo puhelimen sovellukselle bussin odotetun saapumisajan. Bussin karttapaikannuslaite osaa varottaa bussin kuljettajaa lähestyttäessä pysäkkiä, jolta on tehty pysähtymispyyntö. Näin bussin kuljettaja osaa vähentää nopeutta pysäkkiä lähestyttäessä. Puhelimen sovellus kysyy säännöllisin väliajoin HSL:n verkkopalvelulta bussin sijaintitiedot ja bussin ollessa lähestymässä pysäkkiä, sovellus ilmaisee asian matkustajalle.

Mallinna sekvenssikaaviona sovelluksen eri osapuolten (matkustaja, puhelin, GPS-palvelu, HSL-verkkopalvelu, bussin sovellus, bussin kuljettaja) käymä interaktio.

## Tehtävä 6

Tee mahdollisimman kattavat testit [harjoituskerran 2 tehtävän 5](ha2-et.md) luokille _Matkakortti_ ja _Kioski_. Kioskin joudut testaamaan siten, että tutkit että se luo oikeanlaisia matkakortteja.

## Tehtävä 7

Testaa myös edelliseen tehtävään liittyvät luokat _Lataajalaite_ ja _Lukijalaite_.

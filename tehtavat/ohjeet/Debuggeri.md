Ohjelmissa on kiusallisen usein bugeja, ja ohjelmoija joutuu debuggaamaan koodia.

Melko yleinen debuggaustapa on lisätä ohjelmaan aputulosteita joiden avulla pyritään varmistamaan että ohjelma käsittelee muuttujiaan juuri niin kuin ohjelmoija ajatteli.

Aputulostuksia monipuolisempi debuggaustapa on debuggeriohjelman käyttö. Debuggerin avulla ohjelman suoritus voidaan pysäyttää haluttuun kohtaan ja sen jälkeen jatkaa ohjelman suoritusta komento kerrallaan. Samalla on mahdollista tarkastella ohjelman kaikkien muuttujien arvoa. Moderneihin ohjelmistonkehitysympäristöihin (Netbeans, Eclipse, IntelliJ) on integroitu helppokäyttöiset debuggerit.

### Ensimmäinen esimerkki

Tee projekti joka sisältää seuraavan pääohjelman sisältävän luokan:

``` java
public class Main {
 
    public static void main(String[] args) {
        int luku1 = 1;
        int luku2 = 4;
 
        int summa = luku1 + luku2;
        System.out.println( "lukujen " + luku1 + " ja "+ luku2 + " summa on "+ summa);
    }
}
```

### Breakpointin asetus

Paina ensimmäisen koodirivin rivinumeroa. Rivin pitäisi värjääntyä punaiseksi.

Riveille on nyt asetettu breakpoint eli pysähtymispiste. Kun ohjelmaa aletaan suorittaa debuggerilla, pysähtyy ohjelman suoritus rivin kohdalle.
debuggerin käynnistys

Klikkaa projektinäkymästä (vasemmalta) ohjelmatiedoston nimeä hiiren oikealla napilla ja valitse debug file. Debuggeri käynnistyy myös vihreän napin oikeanpuoleisesta napista jos projekti on valittuna main projektiksi.

Debuggeri käynnistyy ja ruudulla pitäisi näyttää seuraavalta:

![](http://www.cs.helsinki.fi/u/mluukkai/otm2012/debug1.png)

Breakpointattu rivi on värjääntynyt vihreäksi. Ohjelman suoritus on pysähtynyt vihreän rivin kohdalle ja seuraavana suoritusvuorossa olevana komentona on rivillä oleva komento.

Alas on ilmestynyt variables välilehti. Välilehdellä näytetään ohjelman muuttujat ja niiden arvot. Koska ohjelma ei ole vielä suorittanut muuttujien alustuksen sisältäviä rivejä, eivät muuttujat vielä ole olemassa (muita kuin koemntoriviparametrit ohjelmalle välittävä args).

### askelletaan komentoja

Pysähtynyttä ohjelmaa voi suorittaa yksi komento kerrallaan yläpalkista valitsemalla Debug > Step Over. Komennon suorittama kuvake löytyy myös yläpalkista (vihreän nuolen oikealla puolella) tai sen voi suorittaa (PC:llä) näppäimellä F8.

Suorita ohjelmaa kolme askelta. Katso jokaisen askeleen jälkeen variables-välilehteä. Askelten jälkeen tilanne on seuraava:

![](http://www.cs.helsinki.fi/u/mluukkai/otm2012/debug2.png)

Eli ohjelmalla on kolme muuttujaa, kaikkien tyyppinä int ja on suoritusvuorossa tulostuskomento.

Suorita vielä viimeinen komento Step over:illa. Tulostus näkyy Output-välilehdellä (kohdassa debug).

### mahdollisia ongelmia

**entä jos variables-välilehti en näy?**

Joskus käy niin, että välilehti "häviää". Sen saa takaisin valitsemalla yläpalkista Window > Debugging > Variables

**entä jos output-välilehteä ei näy?**

Output saattaa myös "hävitä". Saat sen takaisin valitsemalla yläpalkista Window > Output > Output

Hävitä variables ja output ja palauta ne näkyviin.

### taulukon ja Scanner:in sisältävä ohjelma

Muuta ohjelmasi seuraavaksi:

``` java
public class Main {

    public static void main(String[] args) {
        Scanner lukija = new Scanner(System.in);
        int[] luvut = {1, 5, 4, 3};

        System.out.print("etsittävä luku: ");
        int etsittava = lukija.nextInt();

        boolean loytyi = false;
        for (int i = 0; i < luvut.length; i++) {
            if (luvut[i] == etsittava) {
                loytyi = true;
                break;
            }
        }

        if (loytyi) {
            System.out.println("löytyi");
        } else {
            System.out.println("ei löytynyt");
        }
    }
}
```

### scannerin syötteen lukeminen debugatessa

Laita breakpoint for:in kohdalle. Käynnistä debuggaus.

Huomaat että debuggeri "jumiutuu", variables-välilehdelle ei tule mitään ja vihreällä merkattua riviä ei näy.

Ohjelma on pysähtynyt odottamaan käyttäjän syötettä.

Mene output-välilehdelle ja anna syöte. Huomaat että debuggeri normalisoituu ja suoritus etenee for:in alkuun.

### taulukon läpikäynti

Mene Step Over:illa yksi askel eteenpäin. Huomaat muuttujan i ilmestyneen variables-välilehdelle.

Laajenna taulukko klikkaamalla taulukkomuuttujan luvut vasemmalla puolella olevaa rastia. Näkymä on seuraava:

![](http://www.cs.helsinki.fi/u/mluukkai/otm2012/debug3.png)

Kuvassa taulukon alkiot eivät näy "lokerojärjestyksessä". Jos itselläsi on sama ongelma, klikkaa välilehden yläreunasta name niin saat taulukon järjestymään lokerojärjestykseen.

Askella ohjelmaa eteenpäin ja katso miten muuttujien arvot etenevät.

Kokeile suorittaa ohjelma debuggerilla taupauksessa jossa etsittävä löytyy ja jossa etsittävä ei löydy.

## Debugging protip 1

Ohjelman vianselvittäminen on hidasta jos syötteen joutuu antamaan aina käsin esim. Scanner:in kautta.

Vianselvitysvaiheessa kannattaa syöte ehdottomasti "kovakoodata" esim. suoraan muuttujiin. Edellisen ohjelman tapauksessa voisimme toimia seuraavasti:

``` java
public class Main {

    public static void main(String[] args) {
        Scanner lukija = new Scanner(System.in);
        int[] luvut = {1, 5, 4, 3};

        System.out.print("etsittävä luku: ");
        //int etsittava = lukija.nextInt();
        int etsittava = 7;        

        // ...
    }
}
```
Eli kommentoidaan varsinainen syöte pois ja korvataan se kovakoodatulla.

Asian voi tehdä tyylikkäämminkin, esim. korvaamalla Scannerin siten että se lukee näppäimistön sijaan muuttujaa, tällöin varsinaista ohjelmakoodia ei tarvitse muutella yhtään. Ehkä palaamme myöhemmin tähän edistyneempään tekniikkaan.

### Metodin sisältävän ohjelman debuggaus

Korvataan tarkasteltava koodi seuraavalla:

``` java
public class Main {

    public static void main(String[] args) {
        int[] luvut = {1, 5, 4, 3};
        Scanner lukija = new Scanner(System.in);

        System.out.print("etsittävä luku: ");
        int etsittava = lukija.nextInt();

        boolean loytyi = etsiTaulukosta(luvut, etsittava);
        if (loytyi) {
            System.out.println("löytyi");
        } else {
            System.out.println("ei löytynyt");
        }
    }

    public static boolean etsiTaulukosta(int[] taulukko, int etsittava) {
        for (int i = 0; i < taulukko.length; i++) {
            if (taulukko[i] == etsittava) {
                return true;
            }
        }

        return false;
    }
}
```

### Step Over vs Step Into

Laita breakpoint metodikutsun sisältävän rivin alkuun.

Askella ohjelma tuttuun tapaan käyttämällä Step Over:ia. (Muista että debuggaus keskeytyy syötteen kysymiseen.)

Huomaat että ohjelma suorittaa metodin yhtenä askeleena.

Jos halutaan tarkastella myös mitä metodin sisällä tapahtuu, on vaihtoehtoja kaksi.

* Jos metodikutsun kohdalla painetaan Step Into (yläpalkista Debug > Step Into tai näppäin F7 tai työkalurivin symbooli jossa suora nuoli alaspäin) ottaa debuggeri askeleen joka vie metodin sisällä
* Jos metodin sisälle on laitettu breakpoint, pysähtyy ohjelma sinne vaikka käytettäisiin Step over:ia

Huomioi miten variables-välilehden näkymä muuttuu kun siirrytään metodin sisälle. Metodin sisällähän ei nähdä pääohjelman muuttujia vaan ainoastaan metodin sisäiset muuttujat ja parametrina välitetyt muuttujat:

![](http://www.cs.helsinki.fi/u/mluukkai/otm2012/debug4.png)

## Olioita sisältävän ohjelman debuggaaminen

Muutetaan jälleen ohjelmaa. Tee projekti jossa on tehtävästä 1 tuttu LyyraKortti sekä seuraava pääohjelma:


``` java
public class Main {
    public static void main(String[] args) {
        LyyraKortti teronKortti = new LyyraKortti(10);
        LyyraKortti tonynKortti = new LyyraKortti(2.5);
        
        teronKortti.syoMaukkaasti();
        teronKortti.syoMaukkaasti();
        
        teronKortti.lataaRahaa(125);
        
        tonynKortti.syoEdullisesti();
    }
}
```

## Olio variables-välilehdellä

Laita main:in ensimmäiselle riville breakpoint ja lähde askeltamaan ohjelmaa Step over:illa (F8).

Huomaat että oliomuuttujat ilmestyvät variables-välilehdelle. Klikkaa muuttujien vasemmalla puolella olevia rasteja niin näet olion sisälle. Huomaa, että molempien korttien sisälle on talletettu myös lounaiden hinnat.

Huomaat että olioiden sisällä oleva muuttujan arvo vaihtaa arvoaan.

Tee _Step Into (F7) jonkun metodikutsun kohdalla. Näin pääset "olion sisälle". Seuraavassa näkymä kun on F7 painettu metodikutsun teronKortti.lataaRahaa(125) yhteydessä:

![](http://www.cs.helsinki.fi/u/mluukkai/otm2012/debug5.png)

Kuvassa on painettu muuttujan this (eli olion viite itseensä) vasemmalta puolelta plussaa ja näin on saatu näkyviin olion sisäisten muuttujien arvot. Olio siis näkee ainoastaan sisäiset muuttujansa ja mahdolliset metodin parametrit. Etene askel kerrallaan ja seuraa tilanteen kehittymistä.

### useita breakpointeja ja suorituksen jatkaminen

Aina ei ole mielekästä edetä debuggerilla askel kerrallaan naputtamalla F7 tai F8 näppäintä. 
Tällöin voidaan asettaa breakpoint useaan eri kohtaan koodia:

![](http://www.cs.helsinki.fi/u/mluukkai/otm2012/debug6.png)

Aloitettaessa ohjelma pysähtyy normaaliin tapaan ensimmäiseen breakpointiin. Siinä vaiheessa kun askellusta ei tästä breakpointista haluta jatkaa voidaan antaa ohjelman jatkaa seuraavaan breakpointiin asti. Tämä tapahtuu yläpalkista komennolla Debug > Continue tai painamalla (PC:ssä) F5 tai painamalla vihreää ympyrää jonka sisällä on pieni valkea nuoli.

Askellusta voidaan jälleen jatkaa toisesta breakpointista.

### debuggauksen lopetus

Painamalla punaista neliöä saadaan käynnissä oleva debuggaussessio lopetettua.

## Testin suorituksen debuggaus

Yksittäiset testit ovat Javaa siinä missä esim. main, joten niiden debuggaus onnistuu myös.

Debuggaus tapahtuu laittamalla haluttuun kohtaan testiä breakpoint ja käynnistämällä debuggaus klikkaamalla projektinäkymästä (vasemmalta) testin nimeä hiiren oikealla napilla ja valitse debug test file.

Testin debuggaus etenee täsmälleen samalla tavalla kuin normaalin ohjelman eteneminen.

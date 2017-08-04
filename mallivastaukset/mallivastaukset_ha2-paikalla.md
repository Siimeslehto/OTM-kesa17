## Lyyrakortin testit

``` java
public class LyyraKorttiTest {

    LyyraKortti kortti;

    @Before
    public void setUp() {
        kortti = new LyyraKortti(10);
    }

    @Test
    public void konstruktoriAsettaaSaldonOikein() {
        String vastaus = kortti.toString();

        assertEquals("Kortilla on rahaa 10.0 euroa", vastaus);
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
        // nyt kortin saldo on 2
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

    @Test
    public void maukkaastiSyominenEiVieSaldoaNegatiiviseksi() {
        for (int i = 0; i < 3; i++) {
            kortti.syoMaukkaasti();
        }
        assertEquals("Kortilla on rahaa 2.0 euroa", kortti.toString());
    }

    @Test
    public void negatiivisenSummanLataamienEiMuutaSaldoa() {
        kortti.lataaRahaa(-5);
        assertEquals("Kortilla on rahaa 10.0 euroa", kortti.toString());
    }

    @Test
    public void voiOstaaEdullisenLounaanKunJuuriTarpeeksiRahaa() {
        kortti = new LyyraKortti(2.5);
        kortti.syoEdullisesti();
        assertEquals("Kortilla on rahaa 0.0 euroa", kortti.toString());
    }

    @Test
    public void voiOstaaMaukkaastiKunJuuriTarpeeksiRahaa() {
        kortti = new LyyraKortti(4);
        kortti.syoMaukkaasti();
        assertEquals("Kortilla on rahaa 0.0 euroa", kortti.toString());
    }
}
```

## Yläältärajoitetun laskurin testit

``` java
public class YlhaaltaRajoitettuLaskuriTest {
     // apumetodi
    public void eteneNKertaa(YlhaaltaRajoitettuLaskuri laskuri, int n) {
        for (int i = 0; i < n; i++) {
            laskuri.seuraava();
        }
    }

    @Before
    public void setUp() {
        this.laskuri = new YlhaaltaRajoitettuLaskuri(15);
    }
    
    @Test
    public void laskurinArvoOnAluksiNolla() {
        assertEquals(0, laskuri.arvo());
    }
    
    @Test
    public void laskurinArvoKasvaaYhdellaEdetessaKerran() {
        laskuri.seuraava();
        assertEquals(1, laskuri.arvo());
    }
    
    @Test
    public void laskurinArvoOnKaksiKunSeEteneeKahdesti() {
        laskuri.seuraava();
        laskuri.seuraava();
        
        assertEquals(2, laskuri.arvo());
    }
    
    @Test
    public void laskurinArvoOn14KunSitaKasvatetaan14Kertaa() {
        eteneNKertaa(laskuri, 14);
        
        assertEquals(14, laskuri.arvo());
    }
    
    @Test
    public void laskurinArvoOnYlarajaJosLaskuriaKasvatetaanYlarajanMontaKertaa() {
        eteneNKertaa(laskuri, 15);
        
        assertEquals(15, laskuri.arvo());
    }
    
    @Test
    public void laskurinArvoOnNollaJosLaskuriaKasvatetaanYlarajanJalkeenKerran() {
        eteneNKertaa(laskuri, 16);
        
        assertEquals(0, laskuri.arvo());
    }
    
    @Test
    public void laskurinArvoOnYksiJosLaskuriaKasvatetaanYlarajanJalkeenKaksiKertaa() {
        eteneNKertaa(laskuri, 17);
        
        assertEquals(1, laskuri.arvo());
    }
  
    @Test
    public void asetaArvoAsettaaArvonKunNolla() {
        // asetetaan jokin muu arvo kuin nolla jotta nahdaan että arvo oikeasti vaihtuu
        laskuri.asetaArvo(5);
        laskuri.asetaArvo(0);
        assertEquals(0, laskuri.arvo());
    }
    
    @Test
    public void asetaArvoAsettaaArvonKunArvoOnVahemmanKuinYlaraja() {
        laskuri.asetaArvo(10);
        assertEquals(10, laskuri.arvo());
    }
    
    @Test
    public void asetaArvoAsettaaArvonKunYlaraja() {
        laskuri.asetaArvo(15);
        assertEquals(15, laskuri.arvo());
    }
    
    @Test
    public void laskurinArvoEiMuutuKunAsetetaanNegatiivinenArvo() {
        // kasvatetaan yhdellä että nähdään ettei arvoa aseteta nollaksi
        laskuri.seuraava();
        laskuri.asetaArvo(-5);
        assertEquals(1, laskuri.arvo());
    }
    
    @Test
    public void laskurinArvoEiMuutuKunAsetetaanYlarajaPlusYksi() {
        laskuri.asetaArvo(16);
        assertEquals(0, laskuri.arvo());
    }
    
    @Test
    public void laskurinArvoEiMuutuKunAsetetaanSelvastiYlarajaaSuurempiArvo() {
        laskuri.asetaArvo(50);
        assertEquals(0, laskuri.arvo());
    }
    
    @Test
    public void toStringTuottaaEtunollanKunArvoNolla() {
        assertEquals("00", laskuri.toString());
 
    }
    
    @Test
    public void toStringTuottaaEtunollanKunLaskurinArvoOnYksi() {
        laskuri.seuraava();
        assertEquals("01", laskuri.toString());
    }
    
    @Test
    public void toStringTuottaaEtunollanKunLaskurinArvo9() {
        laskuri.asetaArvo(9);
        assertEquals("09", laskuri.toString());
    }
    
    @Test
    public void toStringEiTuotaEtunollaaKunArvo10() {
        laskuri.asetaArvo(10);
        assertEquals("10", laskuri.toString());
    }
    
    @Test
    public void toStringEiTuotaEtunollaaKunArvo11() {
        laskuri.asetaArvo(11);
        assertEquals("11", laskuri.toString());
    }       
}
```

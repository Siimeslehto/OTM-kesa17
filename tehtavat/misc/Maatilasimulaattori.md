### Maatilasimulaattorin ohjelmakoodi

``` java
public interface Eleleva {
    void eleleTunti();
}

public interface Lypsava {
    double lypsa();
}

public class Lehma implements Lypsava, Eleleva {
    private static final String[] NIMIA = new String[] {
        "Anu", "Arpa", "Essi", "Heluna", "Hely",
        "Hento", "Hilke", "Hilsu", "Hymy", "Ihq", "Ilme", "Ilo",
        "Jaana", "Jami", "Jatta", "Laku", "Liekki",
        "Mainikki", "Mella", "Mimmi", "Naatti",
        "Nina", "Nyytti", "Papu", "Pullukka", "Pulu",
        "Rima", "Soma", "Sylkki", "Valpu", "Virpi"
    };
 
    private Random random = new Random();
    private String nimi;
    private double tilavuus;
    private double maara;
 
    public Lehma() {
        this(NIMIA[new Random().nextInt(NIMIA.length)]);
    }
 
    public Lehma(String nimi) {
        this.nimi = nimi;
        this.tilavuus = 15 + random.nextInt(26);
        this.maara = 0;
    }
 
    public String getNimi() {
        return nimi;
    }
 
    public double getTilavuus() {
        return tilavuus;
    }
 
    public double getMaara() {
        return maara;
    }
 
    @Override
    public double lypsa() {
        double lypsettyMaara = maara;
        maara = 0;
        return lypsettyMaara;
    }
 
    @Override
    public void eleleTunti() {
        double kehitettyMaara = 0.7 + random.nextDouble() * 1.3;
        if (maara + kehitettyMaara > tilavuus) {
            maara = tilavuus;
        } else {
            maara += kehitettyMaara;
        }
    }
 
    @Override
    public String toString() {
        return nimi + " " + Math.ceil(maara) + "/" + Math.ceil(tilavuus);
    }
}

public class Lypsyrobotti {
    private Maitosailio maitosailio = null;
 
    public Maitosailio getMaitosailio() {
        return maitosailio;
    }
 
    public void setMaitosailio(Maitosailio maitosailio) {
        this.maitosailio = maitosailio;
    }
 
    public void lypsa(Lypsava lypsava) {
        if (maitosailio == null) {
            throw new IllegalStateException("Maitosäiliötä ei ole asennettu");
        }
 
        maitosailio.lisaaSailioon(lypsava.lypsa());
    }
}

public class Maatila implements Eleleva {
    private String omistaja;
    private Navetta navetta;
    private Set<Lehma> lehmat = new HashSet<Lehma>();
 
    public Maatila(String omistaja, Navetta navetta) {
        this.omistaja = omistaja;
        this.navetta = navetta;
    }
 
    public String getOmistaja() {
        return omistaja;
    }
 
    public void lisaaLehma(Lehma lehma) {
        lehmat.add(lehma);
    }
 
    @Override
    public void eleleTunti() {
        for (Lehma lehma : lehmat) {
            lehma.eleleTunti();
        }
    }
 
    public void hoidaLehmat() {
        navetta.hoida(lehmat);
    }
 
    public void asennaNavettaanLypsyrobotti(Lypsyrobotti lypsyrobotti) {
        navetta.asennaLypsyrobotti(lypsyrobotti);
    }
 
    @Override
    public String toString() {
        String mj =  "Maatilan omistaja: " + omistaja + "\\n"; 
        mj += "Navetan maitosäiliö: " + navetta.getMaitosailio() + "\\n";
 
        if (lehmat.isEmpty()) {
            mj += "Ei lehmiä.";
        } else {
            mj += "Lehmät:\\n";
 
            for (Lehma lehma : lehmat) {
                mj += "        " + lehma + "\\n";
            }
        }
 
        return mj;
    }
}

public class Maitosailio {
    private double tilavuus;
    private double saldo = 0;
 
    public Maitosailio() {
        this(2000);
    }
 
    public Maitosailio(double tilavuus) {
        this.tilavuus = tilavuus;
    }
 
    public double getTilavuus() {
        return tilavuus;
    }
 
    public double getSaldo() {
        return saldo;
    }
 
    public double paljonkoTilaaJaljella() {
        return tilavuus - saldo;
    }
 
    public void lisaaSailioon(double maara) {
        double uusiSaldo = saldo + maara;
        if (uusiSaldo > tilavuus) {
            uusiSaldo = tilavuus;
        }
        saldo = uusiSaldo;
    }
 
    public double otaSailiosta(double maara) {
        double otettuMaara = maara;
        if (otettuMaara > saldo) {
            otettuMaara = saldo;
        }
        saldo -= otettuMaara;
        return otettuMaara;
    }
 
    @Override
    public String toString() {
        return Math.ceil(saldo) + "/" + Math.ceil(tilavuus);
    }
}

public class Navetta {
    private Maitosailio maitosailio;
    private Lypsyrobotti lypsyrobotti;
 
    public Navetta(Maitosailio maitosailio) {
        this.maitosailio = maitosailio;
    }
 
    public Maitosailio getMaitosailio() {
        return maitosailio;
    }
 
    public void asennaLypsyrobotti(Lypsyrobotti lypsyrobotti) {
        lypsyrobotti.setMaitosailio(maitosailio);
        this.lypsyrobotti = lypsyrobotti;
    }
 
    public void hoida(Lehma lehma) {
        if (lypsyrobotti == null) {
            throw new IllegalStateException("Lypsyrobottia ei ole asennettu");
        }
 
        lypsyrobotti.lypsa(lehma);
    }
 
    public void hoida(Collection<Lehma> lehmat) {
        for (Lehma lehma : lehmat) {
            hoida(lehma);
        }
    }
 
    @Override
    public String toString() {
        return maitosailio.toString();
    }
}
```

# Laskari 1 - Paikanpäällä tehtävät

**Jakaantukaa 3-5 hengen ryhmiin**

Oluen verkkomyynti sallittiin Suomessa vuoden 2016 alussa. Kumpula Biershopin toimitusjohtaja Mehtonen haluaa avata verkkokaupan heti vuoden alussa ja on päättänyt pyytää meitä toimittamaan sovelluksen. Visio sovelluksesta on seuraava:

Haluamme markkinoille niin pian kuin mahdollista. Sen takia onkin tärkeää että ensin laitetaan kaupan perustoiminnallisuus kuntoon, lisätoimintoja ehdimme viritellä sivuille myöhemminkin.

Pääsivulle sijoitetaan firman logo sekä linkit yhteystietoihin ja itse kauppaan. Kaupassa näytetään lista ostettavissa olevista tuotteista. Jokaisesta tuotteesta näytetään nimi ja hinta. Klikkaamalla tuotetta voisi päästä tuotteen tarkempaan kuvaukseen, kuvauksen tiedot kannattaa hakea Ratebeer.com-sivun tarjoamasta verkkorajapinnasta. Toiminnallisuuden on tarkoitus olla normaalin webbi-kaupan kaltainen, eli klikkaamalla tuotteen yhteydessä olevaa nappia, menevät tuotteet asiakkaan ostoskoriin. Olisi hyvä jos ostoskorin saldo ja siellä olevat ostokset olisivat koko ajan näkyvillä. Jos jo korissa olevaa tuotetta laitetaan koriin lisää, kannattaa ne näyttää yhtenä "ostoksena", tyyliin "Lapin kulta, 24 pulloa".

Kun ostokset on kerätty koriin, pääsee asiakas kassalle nappia “maksa ostokset”~klikkaamalla. Kassalla ollessa ostoskorin sisältö näytetään. Kassalla ostoksia on mahdollista poistaa ostoskorista, koko kori on mahdollista tyhjentää tai voidaan myös palata takaisin tekemään lisää ostoksia. Kassalla ollessa asiakas täyttää sivulle osoitteensa ja luottokorttinumeron. Kun asiakas klikkaa “suorita maksu”, veloitetaan asiakkaan korttia ja ilmoitetaan jos veloitus onnistui. Asiakas saa sitten tuotteensa postitse kotiin. Jos luottokorttinumero on virheellinen tai maksu ei muusta syystä onnistu, ohjaa järjestelmä asiakkaan takaisin kassalle. Jos ostotapahtuma onnistuu, asiakas ohjataan pääsivulle. Ostoskori tietenkin tyhjenee kun ostokset on tehty onnistuneesti.

Sovellukselle tarvitaan myös erillinen hallintasivu, jonka kautta ylläpitäjän on mahdollista lisätä tuotteita ja niiden tietoja järjestelmään. Jokaisen tuotteen varastosaldo on tietenkin oleellista olla järjestelmällä tiedossa. Onnistuneen ostoksen tulee päivittää saldoa. Kun uusia tuotteita saapuu varastoon, ylläpitäjä kasvattaa saldoa.

Kaikki ostokset on siis pakko tehdä luottokortilla. Luottokorttimaksut tapahtuvat Luottokunnan verkkorajapintaa hyödyntäen. Asiakas ei tietenkään näe mitään, ohjelma hoitaa luottokunnan kanssa kommunikoimisen sisäisesti. Postituksia varten meillä on oma tietojärjestelmä. Verkkokauppa on siihen yhteydessä verkon välityksellä, ja jokaisesta onnistuneesta myyntitapahtumasta pitää lähteä tiedot (toimitusosoite ja ostetut tuotteet) postituksen tietojärjestelmään. Ylläpitäjän on hyvä päästä tarvittaessa näkemään lista kaikista luottokunnan kautta tehdyistä maksuista ja postitustietojärjestelmän kautta tehdyistä toimituksista.

## Tehtävät

  1. Tunnistakaa verkkokaupan käyttäjät
  2. Listatkaa kaikki verkkokaupan käyttötapaukset. Käyttötapauksesta riittää mainita nimi, käyttäjät, esiehto ja jälkiehto sekä käyttötapauksen kulku lyhyesti
    * HUOM: suositaan pienen osatoiminnallisuuden sisältäviä käyttötapauksia, esim. "asiakas valitsee tuotteita ja täyttää laskutustietonsa ja suorittaa maksun" on liian monta toiminnallisuutta sisältävä käyttötapaus ja kannattaa jakaa useaan erilliseen käyttötapaukseen
        Isompi toiminnallisuus koostetaan pienten käyttötapausten avulla!
        Pienelle käyttötapaukselle tulee merkitä esiehdoksi ne aiemmat käyttötapaukset jotka oletetaan suoritetuksi ennen kuin käyttötapaus voidaan suorittaa (esim. ostoksen voi poistaa ostoskorista vain jos siellä on jo ostos)
  3. Piirtäkää käyttötapauskaavio

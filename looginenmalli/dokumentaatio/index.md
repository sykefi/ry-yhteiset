---
layout: "default"
description: ""
id: "dokumentaatio"
status: "Ehdotus"
---
# Loogisen tason kaavatietomalli
{:.no_toc}

**Kopio Kaavatietomallista, MUOKKAA**

1. 
{:toc}

## Yleistä
Loogisen tason Kaavatietomalli määrittelee kaikille kaavalajeille yhteiset tietorakenteet, joita sovelletaan kaavatiedon ilmaisemiseen kullekin kaavalajille laadittujen soveltamisohjeiden ([asemakaava](../../soveltamisohjeet/asemakaava/), [yleiskaava](../../soveltamisohjeet/yleiskaava/)) ja niissä kiinnitettyjen koodistojen sekä [elinkaari](../elinkaarisaannot.html)- ja [laatusääntöjen](../laatusaannot.html) mukaisesti. Looginen tietomalli pyrkii olemaan mahdollisimman riippumaton tietystä toteutusteknologiasta tai tiedon fyysisestä esitystavasta (esim. relaatiotietokanta, tietyn ohjelmointikielen tietorakenteet, XML, JSON).

## Normatiiviset viittaukset
Seuraavat dokumentit ovat välttämättömiä tämän dokumentin täysipainoisessa soveltamisessa:

* [ISO 639-2:1998 Codes for the representation of names of languages — Part 2: Alpha-3 code][ISO-639-2]
* [ISO 8601-1:2019 Date and time — Representations for information interchange — Part 1: Basic rules][ISO-8601-1]
* [ISO 19103:2015 Geographic information — Conceptual schema language][ISO-19103]
* [ISO 19107:2019 Geographic information — Spatial schema][ISO-19107]
* [ISO 19108:2002 Geographic information — Temporal schema][ISO-19108]
* [ISO 19109:2015 Geographic information — Rules for application schema][ISO-19109]
* [ISO 19505-2:ISO/IEC 19505-2:2012, Information technology — Object Management Group Unified Modeling Language (OMG UML) — Part 2: Superstructure][ISO-19505-2]

## Standardienmukaisuus
Looginen kaavatietomalli perustuu [ISO 19109][ISO-19109]-standardin yleinen kohdetietomalliin (General Feature Model, GFM), joka määrittelee rakennuspalikat paikkatiedon ISO-standardiperheen mukaisten sovellusskeemojen määrittelyyn. GFM kuvaa muun muassa metaluokat ```FeatureType```, ```AttributeType``` ja ```FeatureAssociationType```. Kaavatietomallissa kaikki tietokohteet, joilla on tunnus ja jota voivat esiintyä erillään toisista kohteista on määritelty kohdetyypeinä (stereotyyppi ```FeatureType```. Sellaiset tietokohteet, joilla ei ole omaa tunnusta ja jotka voivat esiintyä vain kohdetyyppien attribuuttien arvoina on määritelty [ISO 19103][ISO-19103]-standardin ```DataType```-stereotyypin avulla. Lisäksi [HallinnollinenAlue](#hallinnollinenalue) ja [Organisaatio](#organisaatio) on mallinnettu vain rajapintojen (```Interface```) avulla, koska on niitä ei ole tarpeen kuvata kaavatietomallissa yksityiskohtaisesti, ja on todennäköistä, että kaavatietovarastoja ylläpitävät tietojärjestelmät tarjovat niille konkreettiset toteuttavat luokat.

[ISO 19109][ISO-19109] -standardin lisäksi Kaavatietomalli perustuu muihin paikkatiedon ISO-standardeihin, joista keskeisimpiä ovat [ISO 19103][ISO-19103] (UML-kielen käyttö paikkatietojen mallinnuksessa), [ISO 19107][ISO-19107] (sijaintitiedon mallintaminen) ja [ISO 19108][ISO-19108] (aikaan sidotun tiedon mallintaminen).

### Muulla määritellyt luokat ja tietotyypit

#### CharacterString

Kuvaa yleisen merkkijonon, joka koostuu 0..* merkistä, merkkijonon pituudesta, merkistökoodista ja maksimipituudesta. Määritelty rajapinta-tyyppisenä [ISO 19103][ISO-19103]-standardissa.

#### LanguageString

Kuvaa kielikohtaisen merkkijonon. Laajentaa [CharacterString](#characterstring)-rajapintaa lisäämällä siihen ```language```-attribuutin, jonka arvo on ```LanguageCode```-koodiston arvo. Kielikoodi voi [ISO 19103][ISO-19103]-standardin määritelmän mukaan olla mikä tahansa ISO 639 -standardin osa.

#### Number

Kuvaa yleisen numeroarvon, joka voi olla kokonaisluku, desimaaliluku tai liukuluku. Määritelty rajapintana [ISO 19103][ISO-19103]-standardissa.

#### Integer

Laajentaa [Number](#number)-rajapintaa kuvaamaan numeron, joka on kokonaisluku ilman murto- tai desimaaliosaa. Määritelty rajapintana [ISO 19103][ISO-19103]-standardissa.

#### Decimal

Laajentaa [Number](#number)-rajapintaa kuvaamaan numeron, joka on desimaaliluku. Decimal-rajapinnan toteuttava numero voidaan ilmaista virheettä yhden kymmenysosan tarkkuudella. Määritelty rajapintana [ISO 19103][ISO-19103]-standardissa. Decimal-numeroita käytetään, kun desimaalien käsittelyn tulee olla tarkkaa, esim. rahaan liityvissä tehtävissä.

#### Real

Laajentaa [Number](#number)-rajapintaa kuvaamaan numeron, joka on tarkkudeltaan rajoitettu liukuluku. Real-rajapinnan numero voi ilmaista tarkasti vain luvut, jotka ovat 1/2:n (puolen) potensseja. Määritelty rajapintana [ISO 19103][ISO-19103]-standardissa. Käytännössä esitystarkkuus riippuu numeron tallentamiseen varattujen bittien määrästä, esim. ```float (32-bittinen)``` (tarkkuus 7 desimaalia) ja ```double (64-bittinen)``` (tarkkuus 15 desimaalia).

#### TM_Object

Aikamääreiden yhteinen yläluokka, käytetään, mikäli arvo voi olla joko yksittäinen ajanhetki tai aikaväli. Määritelty luokkana [ISO 19108][ISO-19108]-standardissa. 

#### TM_Instant

Kuvaa yksittäisen ajanhetken 0-ulotteisena ajan geometriana, joka vastaa pistettä avaruudessa. Määritelty luokkana [ISO 19108][ISO-19108]-standardissa. Aikapisteen arvo on määritelty ```TM_Position```-luokalla yhdistelmänä [ISO 8601][ISO-8601-1]-standin mukaisia päivämäärä- tai kellonaika-kenttiä tai näiden yhdistelmää, tai muuta ```TM_TemporalPosition```-luokan avulla kuvattua aikapistettä. Viimeksi mainitun luokan attribuutti ```indeterminatePosition``` mahdollistaa ei-täsmällisen ajanhetken ilmaisemisen liittämällä mahdolliseen arvoon luokittelun tuntematon, nyt, ennen, jälkeen tai nimi.

#### TM_Period

Kuvaa aikavälin [TM_Instant](#tm_instant)-tyyppisten ```begin```- ja ```end```-attribuuttien avulla. Molemmat attribuutit ovat pakollisia, mutta voivat sisältää tuntemattoman arvon  ```indeterminatePosition = unknown``` -attribuutin arvon avulla annettuna. Määritelty luokkana [ISO 19108][ISO-19108]-standardissa.

#### URI

Määrittää merkkijonomuotoisen Uniform Resource Identifier (URI) -tunnuksen [ISO 19103][ISO-19103]-standardissa. URIa voi käyttää joko pelkkänä tunnuksena tai resurssin paikantimena (Uniform Resource Locator, URL).

#### Geometry

Kaikkien geometria-tyyppien yhteinen rajapinta [ISO 19107][ISO-19107]-standardissa. Tyypillisimpiä [ISO 19107][ISO-19107]-standardin Geometry-rajapintaa laajentavia rajapintoja ovat ```Point```, ```Curve```, ```Surface``` ja ```Solid``` sekä ```Collection```, jota käyttämällä voidaan kuvata geometriakokoelmia (multipoint, multicurve, multisurface, multisolid).

#### Point
Täsmälleen yhdestä pisteestä koostuva geometriatyyppi. Määritelty rajapintana [ISO 19107][ISO-19107]-standardissa.

## Kaavatietomallin yleispiirteet

Tietomalli on jaettu kahteen UML-pakettiin: [MKP-ydin](#mkp-ydin) kuvaa maankäyttöpäätösten tietomallintamisessa yleiskäyttöisiksi suunnitellut luokat ja niihin liittyvät koodistot, ja [Kaavatiedot](#kaavatiedot) kuvaa kaavojen mallinukseen tarkoitetut, ydinpakettia hyödyntävät luokat ja niiden koodistot. Myös muita maankäyttöpäätöksiä kuin kaavoja voidaan tulevaisuudessa mallintaa laajentamalla MKP-ydin -paketin luokkia.

UML-mallin suunnittelussa on kiinnitetty erityistä huomiota siihen, että malli tarjoaa hyvän yhteentoimivuuskehikon tulevaisuuden kaavatietojen tietomallipohjaiseen kuvaamiseen erilaisissa tietojärjestelmissä. Malliin on tarkoituksellisesti jätetty useita laajennusmahdollisuuksia käyttäen abstrakteja luokkia ja koodistoja. Käyttämällä yhteistä luokkarakennetta ja erikoistamala koodistoja kaavalajikohtaisesti on saatu aikaan malli, joka mahdollistaa eri kaavalajien käsittelyn, tiedonsiirron ja tallentamisen samojen tietojärjestelmien ja -rakenteiden avulla, mutta tarjoaa kuitenkin riittävät mahdollisuudet eri kaavalajien erityispiirteiden huomioimiseen niiden tietosisällön ja merkityksen osalta.

Kaavatietomallin UML-luokkakaaviot ovat saatavilla erillisellä [UML-kaaviot](../uml/)-sivulla.

## MKP-ydin

### AbstraktiVersioituObjekti
Englanninkielinen nimi: **AbstractVersionedObject**

Stereotyyppi: FeatureType (kohdetyyppi)

Yhteinen yläluokka kaikille kaavatietomallin versiohallituille luokille. Kuvaa kaikkien kohdetyyppien yhteiset ominaisuudet ja assosiaatiot.

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
paikallinenTunnus| localId            | [CharacterString](#characterstring)     | 0..1            | kohteen pääavain (id)
nimiavaruus      | namespace          | [URI](#uri)                 | 0..1            | tunnusten nimiavaruus
viittausTunnus   | referenceId        | [URI](#uri)                 | 0..1            | johdettu nimiavaruudesta, luokan englanninkielisestä nimestä ja paikallisesta tunnuksesta
identiteettiTunnus | identityId       | [CharacterString](#characterstring)     | 0..1            | kohteen versioriippumaton tunnus
tuottajakohtainenTunnus | producerSpecificId | [CharacterString](#characterstring) | 0..1         | kohteen tunnus tuottajatietojärjestelmässä
viimeisinMuutos  | latestChange       | [TM_Instant](#tm_instant)          | 0..1            | ajanhetki, jolloin kohteen tietoja on viimeksi muutettu tuottajatietojärjestelmässä
tallennusAika    | storageTime        | [TM_Instant](#tm_instant)          | 0..1            | ajanhetki, jolloin kohde on tietojärjestelmään

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
korvaaObjektin   | replacesObject     | [AbstraktiVersioituObjekti](#abstraktiversioituobjekti) | 0..* | kohteen versio, jonka tämä versio korvaa. Voi olla saman kohteen edellinen versio tai poistuva kohde, jonka tämä kohde korvaa. Oltava saman luokan instanssi.
korvattuObjektilla | replacedByObject | [AbstraktiVersioituObjekti](#abstraktiversioituobjekti) | 0..* | kohteen versio, jolla tämä versio on korvattu. Voi olla saman kohteen seuraava versio tai uusi kohde, jolla tämä kohde on korvattu. Oltava saman luokan instanssi.

### AbstraktiMaankayttoasia
Englanninkielinen nimi: **AbstractLandUseMatter**

Erikoistaa luokkaa [AbstraktiVersioituObjekti](#abstraktiversioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
nimi             | name               | [LanguageString](#languagestring) | 0..* | asian nimi
kuvaus           | description        | [LanguageString](#languagestring) | 0..* | asian kuvausteksti
aluerajaus       | boundary           | [Geometry](#geometry) | 0..1            | maantieteellinen alue, jota maankäyttöasia koskee.
oikeusvaikutteisuus | legalEffectiveness | [OikeusvaikutteisuudenLaji](#oikeusvaikutteisuudenlaji) | 0..1 | maankäyttöasiassa tehdyn päätöksen oikeusvaikutteisuus
metatatietokuvaus | metadata          | [URI](#uri)         | 0..1            | viittaus ulkoiseen metatietokuvaukseen
voimassaoloaika  | validityTime       | [TM_Period](#tm_period) | 0..1        | maankäyttöasiasssa tehdyn päätöksen voimassaoloaika

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
koskeeHallinnollistaAluetta | appliesToAdministrativeArea | [HallinnollinenAlue](#hallinnollinenalue) | 0..* | hallinnollinen alue, jota asia koskee
vastuullinenOrganisaatio | responsibleOrganisation | [Organisaatio](#organisaatio) | 0..1 | organisaatio, joka on vastuussa asian käsittelystä
hyodynnettyAineisto | usedInputDataset | [Lahtotietoaineisto](#lahtotietoaineisto) | 0..* | lähtötietoaineisto, jota asian valmistelussa ja käsittelyssä on hyödynnetty
liittyvaAsia     | relatedMatter      | [AbstraktiMaankayttoasia](#abstraktimaankayttoasia) | 0..* | toinen, tähän asiaan liittyä asia. Kukin assosiaatio voi sisältää ```rooli```-määreen tyyppiä [LanguageString](#languagestring),joka kuvaa miten asia liittyy tähän asiaan
asianLiite       | attachment         | [Asiakirja](#asiakirja) | 0..*      | asian kuvaamiseen tai käsittelyyn olennaisesti kuuluva liitetty asiakirja. Kukin assosiaatio voi sisältää ```rooli```-määreen tyyppiä [LanguageString](#languagestring),joka kuvaa nimeää liitteen. 

### Asiakirja
Englanninkielinen nimi: **Document**

Kuvaa käsitteen [Kaavan liite](../../kasitemalli/#kaavan-liite), erikoistaa luokkaa [AbstraktiVersioituObjekti](#abstraktiversioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
asiakirjanTunnus | documentIdentifier | [URI](#uri)         | 0..*            | asiakirjan pysyvä tunnus, esim. diaarinumero tai muu dokumentinhallinnan tunnus
nimi             | name               | [LanguageString](#languagestring) | 0..* | asiakirjan nimi
laji             | type               | [AsiakirjanLaji](#asiakirjanlaji) | 1 | asiakirjan tyyppi
lisatietolinkki  | additionalInformationLink | [URI](#uri)  | 0..1            | viittaus ulkoiseen lisätietokuvaukseen asiakirjasta
metatietokuvaus  | metadata           | [URI](#uri)         | 0..1            | viittaus ulkoiseen metatietokuvaukseen


**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
liittyvaAsiakirja | relatedDocument      | [Asiakirja](#asiakirja) | 0..* | toinen, tähän asiaan liittyä asiakirja. Kukin assosiaatio voi sisältää ```rooli```-määreen tyyppiä [LanguageString](#languagestring),joka kuvaa miten asiakirja liittyy tähän asiakirjaan

{% include common/note.html content="Asiakirja-luokka ei kuvaa dokumentin sisältöä, eikä ota kantaa tapaan, jolla sisältö noudetaan kaavatietovarastosta tai muusta dokumentinhallintajärjestelmästä. Nämä tiedot voidaan kuvata asiakirjan metatietokuvauksessa." %}

### Lahtotietoaineisto
Englanninkielinen nimi: **InputDataset**

Kuvaa käsitteen [Lahtotietoaineisto](../../kasitemalli/#lahtotietoaineisto), erikoistaa luokkaa [AbstraktiVersioituObjekti](#abstraktiversioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
aineistoTunnus   | datasetIdentifier  | [URI](#uri)         | 0..*            | lähtötietoaineiston tunnus
nimi             | name               | [LanguageString](#languagestring) | 0..* | aineiston nimi
laji             | type               | [LahtotietoaineistonLaji](#lahtotietoaineistonlaji) | 1 | aineiston tyyppi
aluerajaus       | boundary           | [Geometry](#geometry) | 0..*            | maantieteellinen alue, jota ainesto koskee
lisatietolinkki  | additionalInformationLink | [URI](#uri)  | 0..1            | viittaus ulkoiseen lisätietokuvaukseen asiakirjasta
metatietokuvaus  | metadata           | [URI](#uri)         | 0..1            | viittaus ulkoiseen metatietokuvaukseen

{% include common/note.html content="Lahtotietoaineisto-luokka ei kuvaa aineiston sisältöä, eikä ota kantaa tapaan, jolla sisältö noudetaan kaavatietovarastosta tai muusta tietojärjestelmästä. Nämä tiedot voidaan kuvata lähtötietoaineiston metatietokuvauksessa." %}


### AbstraktiTapahtuma
Englanninkielinen nimi: **AbstractEvent**

Erikoistaa luokkaa [AbstraktiVersioituObjekti](#abstraktiversioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
nimi             | name               | [LanguageString](#languagestring) | 0..* | tapahtuman nimi
tapahtumaAika    | eventTime          | [TM_Object](#tm_object) | 0..1        | tapahtuman aika (hetki tai aikaväli)
kuvaus           | description        | [LanguageString](#languagestring) | 0..* | tapahtuman tekstimuotoinen kuvaus
sijainti         | location           | [Geometry](#geometry) | 0..1          | tapahtumapaikka
lisatietolinkki  | additionalInformationLink | [URI](#uri)  | 0..1            | viittaus ulkoiseen lisätietokuvaukseen tapahtumasta
peruttu          | cancelled          | boolean = false     | 1               | onko tapahtuma peruttu (oletus: false)


**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
liittyvaAsia     | relatedMatter      | [AbstraktiMaankayttoasia](#abstraktimaankayttoasia) | 0..* | asia(n versio)t, joihin tapahtuma liittyy.
liittyvaAsiakirja | relatedDocument      | [Asiakirja](#asiakirja) | 0..* | tapahtumaa liittyä asiakirja. Kukin assosiaatio voi sisältää ```rooli```-määreen tyyppiä [LanguageString](#languagestring),joka kuvaa miten asiakirja liittyy tähän tapahtumaan


### Kasittelytapahtuma
Englanninkielinen nimi: **HandlingEvent**

Kuvaa käsitteen [Käsittelytapahtuma](../../kasitemalli/#käsittelytapahtuma), erikoistaa luokkaa [AbstraktiTapahtuma](#abstraktitapahtuma), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
laji             | type               | [AbstraktiKasittelytapahtumanLaji](#abstraktikasittelytapahtumanlaji) | 1 | käsittelytapahtuman tyyppi

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
kasittelija      | handler            | [Organisaatio](#organisaatio) | 0..1  | tapahtuman vastuullinen toimija


### Vuorovaikutustapahtuma
Englanninkielinen nimi: **InteractionEvent**

Kuvaa käsitteen [Vuorovaikutustapahtuma](../../kasitemalli/#vuorovaikutustapahtuma), erikoistaa luokkaa [AbstraktiTapahtuma](#abstraktitapahtuma), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi              | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
laji             | type               | [AbstraktiVuorovaikutustapahtumanLaji](#abstraktivuorovaikutustapahtumanlaji) | 1 | vuorovaikutustapahtuman tyyppi


### HallinnollinenAlue
Englanninkielinen nimi: **AdministrativeArea**

Stereotyyppi: Interface (rajapinta)

Hallinnollinen alue on kuvattu kaavatietomallissa ainoastaan rajapintana, koska sen mallintaminen on kuulu kaavatietomallin sovellusalaan. Toteuttavien tietojärjestelmien tulee tarjota rajapinnan määrittelemät vähimmäistoiminnallisuudet.

**Operaatiot**

Nimi             | Name               | Palautusarvon tyyppi              | Kuvaus
-----------------|--------------------|-----------------------------------|------------------------------------
hallintoaluetunnus | administrativeAreaIdentifier | [CharacterString](#characterstring) | palauttaa hallinnollisen alueen tunnuksen
alue             | area               | [Geometry](#geometry)             | palauttaa hallinnollisen alueen aluerajauksen
nimi ([CharacterString](#characterstring)) | name  | [CharacterString](#characterstring) | palauttaa hallinnollisen alueen nimen valitulla kielellä


### Organisaatio
Englanninkielinen nimi: **Organization**

Stereotyyppi: Interface (rajapinta)

Organisaatio on kuvattu kaavatietomallissa ainoastaan rajapintana, koska sen mallintaminen on kuulu kaavatietomallin sovellusalaan. Toteuttavien tietojärjestelmien tulee tarjota rajapinnan määrittelemät vähimmäistoiminnallisuudet.

**Operaatiot**

Nimi             | Name               | Palautusarvon tyyppi              | Kuvaus
-----------------|--------------------|-----------------------------------|------------------------------------
nimi ([CharacterString](#characterstring)) | name  | [CharacterString](#characterstring) | palauttaa organisaation alueen nimen valitulla kielellä

### Koodistot

#### LahtotietoaineistonLaji

Englanninkielinen nimi: **InputDatasetKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none) 

{% include common/codelistref.html registry="rytj" id="RY_LahtotietoaineistonLaji" name="Lähtötietoaineiston lajit (asema- ja yleiskaava)" %}


#### AsiakirjanLaji
Englanninkielinen nimi: **DocumentKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none) 

{% include common/codelistref.html registry="rytj" id="RY_AsiakirjanLaji_YKAK" name="Asiakirjan laji (yleis- ja asemakaava)" %}


#### OikeusvaikutteisuudenLaji
Englanninkielinen nimi: **LegalEffectivenessKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_OikeusvaikutteisuudenLaji" name="Oikeusvaikutteisuuden laji (maankäyttöasia)" %}


#### AbstraktiKasittelytapahtumanLaji
Englanninkielinen nimi: **AbstractHandlingEventKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

Käsittelytapahtumien lajit kuvataan MKP-ydin -paketissa abstraktina koodistona, jota laajennetaan kunkin maankäyttöpäätöksen prosessin konkreettisten arvojen mukaisesti niiden tietomalleissa.

#### AbstraktiVuorovaikutustapahtumanLaji
Englanninkielinen nimi: **AbstractInteractionEventKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

Vuorovaikutustapahtumien lajit kuvataan MKP-ydin -paketissa abstraktina koodistona, jota laajennetaan kunkin maankäyttöpäätöksen prosessin konkreettisten arvojen mukaisesti niiden tietomalleissa.

## Kaavatiedot

### Kaava
Englanninkielinen nimi: **SpatialPlan**

Kuvaa käsitteen [Kaava](../../kasitemalli/#kaava), erikoistaa luokkaa [AbstraktiMaankayttoasia](#abstraktimaankayttoasia), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
laji             | type               | [Kaavalaji](#kaavalaji)      | 1               | kaavan tyyppi
kaavaTunnus      | planId             | [URI](#uri)                  | 1               | kaavan yksilöivä ja pysyvä tunnus
elinkaaritila    | lifecycleStatus    | [KaavanElinkaaritila](#kaavanelinkaaritila) | 1 | kaavan elinkaaren tila
kumoamistieto    | cancellationInfo   | [KaavanKumoamistieto](#kaavanakumoamistieto) | 0..* | kaava tai sen osa, jonka tämä kaava kumoaa
maanalaisuus     | groundRelativePosition | [MaanalaisuudenLaji](#maanalaisuudenlaji) | 0..1 | luokittelu maanalaista ja maanpäällistä maankäyttöä koskeviin kaavoihin
virelletuloAika  | initiationTime     | [TM_Instant](#tm_instant)    | 0..1            | aika, jolloin kaava on tullut virelle
hyvaksymisAika   | approvalTime       | [TM_Instant](#tm_instant)    | 0..1            | aika, jolloin kaava on tullut virallisesti hyväksytty
digitaalinenAlkupera | digitalOrigin  | [DigitaalinenAlkupera](#digitaalinenalkupera) | 0..1 | luokittelu alunperin tietomallin mukaan luotuihin ja jälkeenpäin digituihin kaavoihin

[AbstraktiMaankayttoasia](#abstraktimaankayttoasia)-luokasta peritytyvä attribuutti ```aluerajauus``` kuvaa kaavan suunnittelualueen.

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
osallistumisJaArvointisuunnitelma | participationAndEvaluationPlan | [OsallistumisJaArviontiSuunnitelma](#osallistumisjaarviointisuunnitelma) | 0..1 | kaavan osallistumis- ja arviointisuunnitelma
selostus         | spatialPlanCommentary | [Kaavaselostus](#kaavaselostus) | 0..1          | kaavaselostus
laatija          | planner            | [KaavanLaatija](#kaavanlaatija) | 0..* | kaavan laatimiseen tietyssä roolissa osallistunut suunnittelija
kaavakohde       | planObject         | [Kaavakohde](#kaavakohde) | 0..*       | paikkatietokohde, johon kohdistuu kaavamääräyksiä tai -suosituksia
yleismaarays     | generalRegulation  | [Kaavamaarays](#kaavamaarays) | 0..*   | kaavamääräys, joka koskee koko kaavan aluetta
yleissuositus    | generalGuidance    | [Kaavasuositus](#kaavasuositus) | 0..* | kaavasuositus, joka koskee koko kaavaan aluetta

### Kaavaselostus

Englanninkielinen nimi: **SpatialPlanCommentary**

Kuvaa käsitteen [Kaavaselostus](../../kasitemalli/#kaavaselostus), erikoistaa luokkaa [AbstraktiVersioituObjekti](#abstraktiversioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

Kaavaselostus on on Kaavatietomallin tässä versiossa kuvattu vain viittauksena kaavaselostuksen muodostaviin asiakirjoihin. Tulevissa tietomallin kehitysversiossa kaavaselostusta voidaan rakenteistaa pidemmälle tämän luokan avulla.

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
asiakirja        | document           | [Asiakirja](#asiakirja) | 0..*        | asiakirja, joka on osa kaavaselostusta. Kukin assosiaatio voi sisältää ```rooli```-määreen tyyppiä [LanguageString](#languagestring), joka kuvaa mikä osa kaavaselostusta asiakirja on


### OsallistumisJaArviointisuunnitelma

Englanninkielinen nimi: **ParticipationAndEvalutionPlan**

Kuvaa käsitteen [Osallistumis- ja arviointisuunnitelma](../../kasitemalli/#osallistumis--ja-arviointisuunnitelma), erikoistaa luokkaa [AbstraktiVersioituObjekti](#abstraktiversioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

Osallistumis- ja arviointisuunnitelma on on Kaavatietomallin tässä versiossa kuvattu vain viittauksena suunnitelman muodostaviin asiakirjoihin. Tulevissa tietomallin kehitysversiossa osallistumis- ja arviointisuunnitelmaa voidaan rakenteistaa pidemmälle tämän luokan avulla.

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
asiakirja        | document           | [Asiakirja](#asiakirja) | 0..*        | asiakirja, joka on osa kaavaselostusta. Kukin assosiaatio voi sisältää ```rooli```-määreen tyyppiä [LanguageString](#languagestring), joka kuvaa mikä osa osallistumis- ja arviointisuunnitelmaa asiakirja on


### KaavanLaatija

Englanninkielinen nimi: **Planner**

Kuvaa käsitteen [Kaavan laatija](../../kasitemalli/#kaavan-laatija), erikoistaa luokkaa [AbstraktiVersioituObjekti](#abstraktiversioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
nimi             | personName         | [CharacterString](#characterstring) | 1        | Laatijan nimi
nimike           | professionalTitle  | [LanguageString](#languagestring) | 0..*       | ammatti- tai virkanimike
rooli            | role               | [LanguageString](#languagestring) | 0..*       | suunnittelijan rooli kaavan laadinnassa 


### AbstraktiKaavakohde

Englanninkielinen nimi: **AbstractPlanObject**

Erikoistaa luokkaa [AbstraktiVersioituObjekti](#abstraktiversioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

Kaikkien kaavaan liittyvien paikkatietokohteiden yhteinen abstrakti yläluokka. Kohteen geometria voi olla 2-ulotteinen piste, viiva tai alue, tai 3-ulotteinen kappale. Moniosaiset geometriat (multigeometry) ovat sallittuja. Haluttaessa korkeusulottuvuus voidaan ilmaista 2-ulotteisen ```geometria```-attribuutin arvo ja ```pystysuuntainenRajaus```-attribuutin kuvaamien korkeusvälien avulla, myös useampana erillisenä kerroksena. Tällöin kohteen ulottuvuus vastaa 3-ulotteista avaruusgeometriaa, joka muodostuu työntämällä 2-ulotteista pintaa ylös- ja/tai alaspäin annatun korkeusvälin rajoihin saakka. Huomaa, että [Korkeusvali](#korkeusvali)-luokan ylä- tai alaraja (korkeuden maksimi- tai minimiarvo) voi myös puuttua, jolloin kohde kattaa alueen ylöspäin tai alaspäin annetusta korkeudesta.  

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
nimi             | name               | [LanguageString](#languagestring) | 0..*       | kohteen tunnistamiseen käytettävä nimi. Huom: kaavan oikeusvaikutteiset nimeämiset (mm. katujen, teiden ja yleisten alueiden nimet ja korttelinumerot) kuvataan kaavamääräysten arvojen avulla.
geometria        | geometry           | [Geometry](#geometry)        | 1               | kohteen sijainti kaava-alueella
pystysuuntainenRajaus | verticalLimit | [Korkeusvali](#korkeusvali)  | 0..*            | kohteen alueen pystysuuntainen rajaus

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
kaava            | spatialPlan        | [Kaava](#kaava)     | 1               | kaava, johon kohde kuuluu
liittyvaKohde    | relatedPlanObject  | [AbstraktiKaavakohde](#abstraktikaavakohde) | 0..* | kohde, joka liittyy tähän kohteeseen. Kukin assosiaatio voi sisältää ```rooli```-määreen tyyppiä [LanguageString](#languagestring), joka kuvaa miten kohde liittyy tähän kohteeseen.

### AbstraktiTietoyksikko

Englanninkielinen nimi: **AbstractInformationUnit**

Erikoistaa luokkaa [AbstraktiVersioituObjekti](#abstraktiversioituobjekti), stereotyyppi: FeatureType (kohdetyyppi)

Kaikkien kaavaan liittyvien tietoelementtien yhteinen abstrakti yläluokka.

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
nimi             | name               | [LanguageString](#languagestring) | 0..*       | tietoyksikön tunnistamiseen käytettävä nimi. Huom: kaavan oikeusvaikutteiset nimeämiset (mm. katujen, teiden ja yleisten alueiden nimet ja korttelinumerot) kuvataan kaavamääräysten arvojen avulla.
arvo             | value              | [AbstraktiArvo](#abstraktiarvo) | 0..*         | tietoyksikön arvo


### Kaavakohde
Englanninkielinen nimi: **PlanObject**

Kuvaa käsitteen [Kaavakohde](../../kasitemalli/#kaavakohde), erikoistaa luokkaa [AbstraktiKaavakohde](#abstraktikaavakohde), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
laji             | type               | [AbstraktiKaavakohdelaji](#abstraktikaavakohdelaji) | 0..1 | varattu tulevaisuuden käyttöön
sijainninSitovuus | bindingnessOflocation | [Sitovuuslaji](#sitovuuslaji) | 0..1       | kaavakohteeseen liitettyjen kaavamääräysten ja -suositusten sijainnin tulkinta
liittyvanLahtotietokohteenTunnus | relatedInputDatasetObjectId | [URI](#uri) | 0..*    | viittaus kaavan lähtötietoaineistoon sisältyvään tietokohteeseen, joka liittyy kaavakohteeseen. Esim. pohjavesialue
ymparistomuutoksenLaji | environmentalChangeNature | [AbstraktiYmparistomuutoksenLaji](#abstraktiymparistomuutoksenlaji) | 0..1 | kuvaa kaavakohteen alueelle kaavassa suunnitellun muutoksen merkittävyyttä
maanalaisuus     | groundRelativePosition | [MaanalaisuudenLaji](#maanalaisuudenlaji) | 0..1 | luokittelu maanalaista ja maanpäällistä maankäyttöä koskeviin kaavakohteisiin

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
maarays          | regulation         | [Kaavamaarays](#kaavamaarays) | 0..*  | kaavamääräys, joka kohdistuu tämän kaavakohteen alueelle
suositus         | guidance           |[ Kaavasuositus](#kaavasuositus) | 0..*  | kaavasuositus, joka kohdistuu tämän kaavakohteen alueelle

### Kaavamaarays

Englanninkielinen nimi: **PlanRegulation**

Kuvaa käsitteen [Kaavamääräys](../../kasitemalli/#kaavamääräys), erikoistaa luokkaa [AbstraktiTietoyksikko](#abstraktitietoyksikko), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
laji             | type               | [AbstraktiKaavamaaraysLaji](#abstraktikaavamaarayslaji) | 1 | kaavamääräyksen luokittelu
elinkaaritila    | lifecycleStatus    | [KaavanElinkaaritila](#kaavanelinkaaritila) | 1 | kaavamääräyksen elinkaaren tila
teema            | theme              | [AbstraktiKaavoitusteema](#abstraktikaaavoitusteema) | 0..* | kaavamääräyksen teemoittelu
lisatieto        | additionalInformation | [Lisatieto](#lisatieto)   | 0..*            | tarkentaa tai rajaa kaavamääräystä
voimassaoloAika  | validityTime       | [TM_Period](#tm_period)      | 0..1            | kaavamääräyksen lainvoimaisuusaika

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
liittyvaAsiakirja | relatedDocument   | [Asiakirja](#asiakirja) | 0..*        | asiakirja, joka on perustelee kaavamääräystä tai liittyy siihen muulla tavalla. Kukin assosiaatio voi sisältää ```rooli```-määreen tyyppiä [LanguageString](#languagestring), joka kuvaa liittymistavan.

### Kaavasuositus

Englanninkielinen nimi: **PlanGuidance**

Kuvaa käsitteen [Kaavasuositus](../../kasitemalli/#kaavasuositus), erikoistaa luokkaa [AbstraktiTietoyksikko](#abstraktitietoyksikko), stereotyyppi: FeatureType (kohdetyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
elinkaaritila    | lifecycleStatus    | [KaavanElinkaaritila](#kaavanelinkaaritila) | 1 | kaavasuosituksen elinkaaren tila
teema            | theme              | [AbstraktiKaavoitusteema](#abstraktikaaavoitusteema) | 0..* | kaavasuosituksen teemoittelu
voimassaoloAika  | validityTime       | [TM_Period](#tm_period)      | 0..1            | kaavasuosituksen lainvoimaisuusaika

**Assosiaatiot**

Roolinimi        | Role name          | Kohde               | Kardinaliteetti | Kuvaus
-----------------|--------------------|---------------------|-----------------|------------------------------------
liittyvaAsiakirja | relatedDocument   | [Asiakirja](#asiakirja) | 0..*        | asiakirja, joka on perustelee kaavasuositusta tai liittyy siihen muulla tavalla. Kukin assosiaatio voi sisältää ```rooli```-määreen tyyppiä [LanguageString](#languagestring), joka kuvaa liittymistavan.

### Lisatieto

Englanninkielinen nimi: **SupplementaryInformation**

Kuvaa käsitteen [Lisätieto](../../kasitemalli/#lisätieto), stereotyyppi: DataType (tietotyyppi)

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
laji             | type               | [AbstraktiLisatiedonLaji](#abstraktilisatiedonlaji) | 1 | lisätiedon luokittelu
nimi             | name               | [LanguageString](#languagestring) | 0..*       | lisätiedon tunnistamiseen käytettävä nimi. Huom: kaavan oikeusvaikutteiset nimeämiset (mm. katujen, teiden ja yleisten alueiden nimet ja korttelinumerot) kuvataan kaavamääräysten arvojen avulla.
arvo             | value              | [AbstraktiArvo](#abstraktiarvo) | 0..*         | lisätiedon lajia tarkentava arvo


### KaavanKumoamistieto
Englanninkielinen nimi: **CancellationInfo**

Stereotyyppi: DataType (tietotyyppi)

Kumoamistieto yksilöi mitä kaavoja tai niiden osia kaava kumoaa lainvoimaiseksi tullessaan.

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
kumottavanKaavanTunnus | cancelledPlanId | [URI](#uri)               | 1               | kaava, johon kumoaminen kohdistuu
kumoaaKaavanKokonaan | cancelsEntirePlan | boolean                   | 1               | jos arvo on ```true```, kumoaa kaavan kokonaisuudessaan, muuten muiden ominaisuuksien yksilöimällä tavalla
kumottavaKaavanAlue | areaToCancel    | [Geometry](#geometry)        | 0..1            | alue, jonka sisällä kokonaan olevia kaavamääräyksiä ja -suosituksia kumoaminen koskee
kumottavanMaarayksenTunnus | cancelledRegulationId | [URI](#uri)     | 0..*            | kaavamääräyksen ```viittausTunnus```, jota kumoaminen koskee
kumottavanSuosituksenTunnus | cancelledGuidanceId | [URI](#uri)      | 0..*            | kaavasuosituksen ```viittausTunnus```, jota kumoaminen koskee

### AbstraktiArvo
Englanninkielinen nimi: **AbstractValue**

Kuvaa käsitteen [Arvo](../../kasitemalli/#arvo), stereotyyppi: DataType (tietotyyppi)

[Kaavamaarays](#kaavamaarays)- ja [Lisatieto](#lisatieto)-luokkien instansseja tarkentavien arvojen yhteinen abstrakti yläluokka.

### Ajanhetkiarvo
Englanninkielinen nimi: **TimeInstantValue**

Erikoistaa luokkaa [AbstraktiArvo](#abstraktiarvo), stereotyyppi: DataType (tietotyyppi)

Arvo, joka kuvaa yksittäistä ajanhetkeä.

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
arvo             | value              | [TM_Instant](#tm_instant)    | 1               | ajanhetkiarvo

### Aikavaliarvo
Englanninkielinen nimi: **TimePeriodValue**

Erikoistaa luokkaa [AbstraktiArvo](#abstraktiarvo), stereotyyppi: DataType (tietotyyppi)

Arvo, joka kuvaa aikaväliä.

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
arvo             | value              | [TM_Period](#tm_period)      | 1               | aikaväliarvo


### GeometriaArvo
Englanninkielinen nimi: **GeometryValue**

Erikoistaa luokkaa [AbstraktiArvo](#abstraktiarvo), stereotyyppi: DataType (tietotyyppi)

Arvo, joka kuvaa aikaväliä.

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
arvo             | value              | [Geometry](#geometry)        | 1               | geometria-arvo


### Koodiarvo
Englanninkielinen nimi: **CodeValue**

Erikoistaa luokkaa [AbstraktiArvo](#abstraktiarvo), stereotyyppi: DataType (tietotyyppi)

Arvo, joka kuvaa tiettyä annetun koodiston koodia.

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
arvo             | value              | [URI](#uri)                  | 1               | koodiarvo
koodistonTunnus  | codeList           | [URI](#uri)                  | 0..1            | koodiston tunnus. Voidaan jättää pois jos arvo on selvää koodiarvon perusteella selvä
otsikko          | title              | [LanguageString](#languagestring) | 0..*       | koodin otsikko halutulla kielellä. Ei huomioida tallennettaessa tietoa

### NumeerinenArvo
Englanninkielinen nimi: **NumericValue**

Erikoistaa luokkaa [AbstraktiArvo](#abstraktiarvo), stereotyyppi: DataType (tietotyyppi)

Arvo, joka kuvaa liukulukuna annettua numeroa.

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
arvo             | value              | [Number](#number)            | 1               | numeroarvo, voidaan tarkentaa soveltamisprofiileissa tyyppeihin Integer, Decimal tai Real
mittayksikko     | unitOfMeasure      | [CharacterString](#characterstring) | 0..1     | mittayksikön tunnus, esim. [UCUM](https://ucum.org/ucum.html)-notaation mukaisesti

### NumeerinenArvovali
Englanninkielinen nimi: **NumericRange**

Erikoistaa luokkaa [AbstraktiArvo](#abstraktiarvo), stereotyyppi: DataType (tietotyyppi)

Arvo, joka kuvaa liukulukuna annettua numeerista arvoväliä.

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
nimimiarvo       | minimumValue       | [Number](#number)            | 0..1            | välin alaraja, voidaan tarkentaa soveltamisprofiileissa tyyppeihin Integer, Decimal tai Real. Jos ei anneta, kuvaa väliä, joka ulottuu äärettömän pitkälle alaspäin
maksimiarvo      | maximumValue       | [Number](#number)            | 0..1            | välin yläraja. voidaan tarkentaa soveltamisprofiileissa tyyppeihin Integer, Decimal tai Real. Jos ei anneta, kuvaa väliä, joka ulottuu äärettömän pitkälle ylöspäin
mittayksikko     | unitOfMeasure      | [CharacterString](#characterstring) | 0..1     | mittayksikön tunnus, esim. [UCUM](https://ucum.org/ucum.html)-notaation mukaisesti

### Tunnusarvo
Englanninkielinen nimi: **IdentifierValue**

Erikoistaa luokkaa [AbstraktiArvo](#abstraktiarvo), stereotyyppi: DataType (tietotyyppi)

Kuvaa rekisterissä hallittavan tunnuksen arvona. Kaavamääräyksestä ja lisätiedosta riippuen tunnus annetaan tai siihen viitataan kaavassa. 

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
arvo             | value              | [URI](#uri)                  | 1               | tunnus
rekisterinTunnus | registerId         | [URI](#uri)                  | 0..1            | rekisterin tunnus
rekisterinNimi   | registerName       | [LanguageString](#languagestring) | 0..*       | rekisterin nimi halutulla kielellä. Ei huomioida tallennettaessa tietoa

### Tekstiarvo
Englanninkielinen nimi: **TextValue**

Erikoistaa luokkaa [AbstraktiArvo](#abstraktiarvo), stereotyyppi: DataType (tietotyyppi)

Kuvaa mahdollisesti monella kielellä annettavan tekstimuotoisen arvon.

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
arvo             | value              | [LanguageString](#languagestring) | 1..*       | kielikohtainen teksti
syntaksi         | syntax             | [CharacterString](#characterstring) | 0..1     | syntaksi, jolla tekstiarvo on kuvattu. Jos ei anneta, tulkitaan luonnollisen kielen arvona.

### Korkeuspiste
Englanninkielinen nimi: **ElevationPosition**

Erikoistaa luokkaa [NumeerinenArvo](#numeerinenarvo), stereotyyppi: DataType (tietotyyppi)

Arvo, joka kuvaa pistettä pystysuuntaisella koordinaatistolla.

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
referenssipiste  | referencePoint     | [Point](#point)              | 0..1            | pystysuuntaisen koordinaatiston piste, jossa ```arvo```-attribuutin arvo on nolla. Käytetty pystysuuntainen koordinaatisto on annettava pisteen yhteydessä.

### Korkeusvali
Englanninkielinen nimi: **ElevationRange**

Erikoistaa luokkaa [NumeerinenArvovali](#numeerinenarvovali), stereotyyppi: DataType (tietotyyppi)

Arvo, joka kuvaa kahden pystysuuntaisella koordinaatiston koordinaatin välistä janaa.

**Ominaisuudet**

Nimi             | Name               | Tyyppi                       | Kardinaliteetti | Kuvaus
-----------------|--------------------|------------------------------|-----------------|------------------------------------
referenssipiste  | referencePoint     | [Point](#point)              | 0..1            | pystysuuntaisen koordinaatiston piste, jossa ```arvo```-attribuutin arvo on nolla. Käytetty pystysuuntainen koordinaatisto on annettava pisteen yhteydessä.


### Koodistot

#### Kaavalaji
Englanninkielinen nimi: **SpatialPlanKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_Kaavalaji" name="Kaavalajit (maakunta-, yleis- ja asemakaava)" %}


#### KaavanElinkaaritila
Englanninkielinen nimi: **SpatialPlanLifecycleStatus**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_KaavanElinkaaritila" name="Elinkaaren tila (yleis- ja asemakaava)" %}

#### Sitovuuslaji
Englanninkielinen nimi: **BindingnessKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_Sitovuuslaji" name="Sijainnin sitovuuden laji" %}

#### MaanalaisuudenLaji
Englanninkielinen nimi: **GroundRelativenessKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_MaanalaisuudenLaji" name="Maanalaisuuden laji" %}

#### DigitaalinenAlkupera
Englanninkielinen nimi: **DigitalOriginKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_DigitaalinenAlkupera" name="Kaavan digitaalinen alkuperä" %}

#### AbstraktiKaavakohdelaji
Englanninkielinen nimi: **AbstractPlanObjectKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Tyhjä koodiluettelo](http://inspire.ec.europa.eu/registry/extensibility/any)

Ei määriteltyjä arvoja. Varattu tulevaisuuden kaavakohteiden tyypitykseen.

#### AbstraktiYmparistomuutoksenLaji
Englanninkielinen nimi: **AbstractEnvironmentalChangeKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

#### YmparistomuutoksenLajiYleiskaava
Englanninkielinen nimi: **MasterPlanEnvironmentalChangeKind**

Erikoistaa luokkaa [AbstraktiYmparistomuutoksenLaji](#abstraktiymparistomuutoksenlaji), stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_YmparistomuutoksenLaji_YK" name="Ympäristömuutoksen laji (yleiskaava)" %}

#### AbstraktiKaavoitusteema
Englanninkielinen nimi: **AbstractSpatialPlanTheme**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Laajennettavissa kaikilla tasoilla](http://inspire.ec.europa.eu/registry/extensibility/open)


#### KaavoitusteemaAsemakaava
Englanninkielinen nimi: **DetailPlanTheme**

Erikoistaa luokkaa [AbstraktiKaavoitusteema](#abstraktikaavoitusteema), stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Laajennettavissa kaikilla tasoilla](http://inspire.ec.europa.eu/registry/extensibility/open)

{% include common/codelistref.html registry="rytj" id="RY_Kaavoitusteema_AK" name="Kaavoitusteema (asemakaava)" %}

#### KaavoitusteemaYleiskaava
Englanninkielinen nimi: **MasterPlanTheme**

Erikoistaa luokkaa [AbstraktiKaavoitusteema](#abstraktikaavoitusteema), stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Laajennettavissa kaikilla tasoilla](http://inspire.ec.europa.eu/registry/extensibility/open)

{% include common/codelistref.html registry="rytj" id="RY_Kaavoitusteema_YK" name="Kaavoitusteema (yleiskaava)" %}

#### AbstraktiKaavamaarayslaji
Englanninkielinen nimi: **AbstractPlanRegulationKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

#### KaavamaarayslajiAsemakaava
Englanninkielinen nimi: **DetailPlanRegulationKind**

Erikoistaa luokkaa [AbstraktiKaavamaarayslaji](#abstraktikaavamaarayslaji), stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_KaavamaaraysLaji_AK" name="Kaavamääräyslaji (asemakaava)" %}

#### KaavamaarayslajiYleiskaava
Englanninkielinen nimi: **MasterPlanRegulationKind**

Erikoistaa luokkaa [AbstraktiKaavamaarayslaji](#abstraktikaavamaarayslaji), stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_KaavamaaraysLaji_YK" name="Kaavamääräyslaji (yleiskaava)" %}


#### AbstraktiLisatiedonLaji
Englanninkielinen nimi: **AbstractAdditionInformationKind**

Stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Laajennettavissa kaikilla tasoilla](http://inspire.ec.europa.eu/registry/extensibility/open)

#### LisatiedonLajiAsemakaava
Englanninkielinen nimi: **DetailPlanAdditionInformationKind**

Erikoistaa luokkaa [AbstraktiLisatiedonLaji](#abstraktilisatiedonlaji), stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_LisatiedonLaji_AK" name="Kaavamääräyksen lisätiedon laji (asemakaava)" %}

#### LisatiedonLajiYleiskaava
Englanninkielinen nimi: **MasterPlanAdditionInformationKind**

Erikoistaa luokkaa [AbstraktiLisatiedonLaji](#abstraktilisatiedonlaji), stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_LisatiedonLaji_YK" name="Kaavamääräyksen lisätiedon laji (yleiskaava)" %}

#### KaavanKasittelytapahtumanLaji
Englanninkielinen nimi: **SpatialPlanHandlingEventKind**

Erikoistaa luokkaa [AbstraktiKasittelytapahtumanLaji](#abstraktikasittelytapahtumanlaji), stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_KaavanKasittelytapahtumanLaji" name="Käsittelytapahtuman laji (asema- ja yleiskaava)" %}

#### KaavanVuorovaikutustapahtumanlaji
Englanninkielinen nimi: **SpatialPlanInteractionEventKind**

Erikoistaa luokkaa [AbstraktiVuorovaikutustapahtumanLaji](#abstraktivuorovaikutustapahtumanlaji), stereotyyppi: CodeList (koodisto)

Laajennettavuus: [Ei laajennettavissa](http://inspire.ec.europa.eu/registry/extensibility/none)

{% include common/codelistref.html registry="rytj" id="RY_KaavanVuorovaikutustapahtumanLaji" name="Vuorovaikutustapahtuman laji (asema- ja yleiskaava)" %}

[ISO-8601-1]: https://www.iso.org/standard/70907.html "ISO 8601-1:2019 Date and time — Representations for information interchange — Part 1: Basic rules"
[ISO-639-2]: https://www.iso.org/standard/4767.html "ISO 639-2:1998 Codes for the representation of names of languages — Part 2: Alpha-3 code"
[ISO-19103]: https://www.iso.org/standard/56734.html "ISO 19103:2015 Geographic information — Conceptual schema language"
[ISO-19107]: https://www.iso.org/standard/66175.html "ISO 19107:2019 Geographic information — Spatial schema"
[ISO-19108]: https://www.iso.org/standard/26013.html "ISO 19108:2002 Geographic information — Temporal schema"
[ISO-19109]: https://www.iso.org/standard/59193.html "ISO 19109:2015 Geographic information — Rules for application schema"
[ISO-19505-2]: https://www.iso.org/standard/52854.html "ISO/IEC 19505-2:2012, Information technology — Object Management Group Unified Modeling Language (OMG UML) — Part 2: Superstructure"
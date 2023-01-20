---
layout: "default"
description: ""
id: "dokumentaatio"
status: "Keskeneräinen"
---
# Rakennetun ympäristön yhteiset tietokomponentit - loogisen tason tietomalli
{:.no_toc}

{% include common/note.html content="Kirjaston dokumentaatio on toistaiseksi puutteellinen. Täydelliset luokkien, niiden attribuuttien ja assosiaatioden kuvaukset laaditaan mahdollisimman pian" %}

1. 
{:toc}

## Yleistä

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
Kuvattu tietomalli perustuu [ISO 19109][ISO-19109]-standardin yleinen kohdetietomalliin (General Feature Model, GFM), joka määrittelee rakennuspalikat paikkatiedon ISO-standardiperheen mukaisten sovellusskeemojen määrittelyyn. GFM kuvaa muun muassa metaluokat ```FeatureType```, ```AttributeType``` ja ```FeatureAssociationType```. Kaavatietomallissa kaikki tietokohteet, joilla on tunnus ja jota voivat esiintyä erillään toisista kohteista on määritelty kohdetyypeinä (stereotyyppi ```FeatureType```. Sellaiset tietokohteet, joilla ei ole omaa tunnusta ja jotka voivat esiintyä vain kohdetyyppien attribuuttien arvoina on määritelty [ISO 19103][ISO-19103]-standardin ```DataType```-stereotyypin avulla.

[ISO 19109][ISO-19109] -standardin lisäksi tietomalli perustuu muihin paikkatiedon ISO-standardeihin, joista keskeisimpiä ovat [ISO 19103][ISO-19103] (UML-kielen käyttö paikkatietojen mallinnuksessa), [ISO 19107][ISO-19107] (sijaintitiedon mallintaminen) ja [ISO 19108][ISO-19108] (aikaan sidotun tiedon mallintaminen).

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

## Tietomallin yleispiirteet

## Ydin

### VersioituObjekti

### AlueidenkäyttöJaRakentamisasia

### AlueidenkäyttöJaRakentamispäätös

### AlueidenkäyttöJaRakentamismääräys

### Tietoryhmä

### Tietoyksikkö

### RakennetunYmpäristönKohde

### Liiteasiakirja

### Asiakirja

### Asiasana

### HallinnollinenAlue

### Henkilö

### Organisaatio

### Toimija

## Alueidenkäyttö

### Alueidenkäyttöasia

### Alueidenkäyttöpäätös

### Alueidenkäyttösuunnitelma

### AlueidenkäyttösuunnitelmanHyväksymispäätös

### Lähtötietoaineisto

## Rakennetun ympäristön luvat

### RakennetunYmpäristönLupa

### RakennetunYmpäristönLupaAsia

### RakennetunYmpäristönLupahakemus

### RakennetunYmpäristönLupapäätös

### LainTaiAsetuksenMääräys

### Säädösviite

### MääräyksestäPoikkeaminen

### Lupamääräys

## Suureet ja arvot

### AbstraktiSuureenArvo

### Suure

### SuureenArvo

### VaihtoehtoinenSuureenArvo

### EhdollinenSuureenArvo

### Ehto

### Ehtolause

### SuureenArvoEhto

### OminaisuudenArvo

### Aikaväliarvo

### Ajanhetkiarvo

### GeometriaArvo

### Koodiarvo

### NumeerinenArvo

### NumeerinenArvoväli

### Korkeuspiste

### Korkeusväli

### Tunnusarvo

### Tekstiarvo

## Tapahtumat

### Tapahtuma

### Käsittelytapahtuma

### Vuorovaikutustapahtuma

## Apuluokat

### OsallistumisJaArviointisuunnitelma

### Rajapiste

### SuunnitelmanLaatija

### Kiinteistö

### Osoite

## Koodistot

### AbstraktiAsiakirjanLaji

### AbstaktiAsianElinkaaritila

### AbstraktiKäsittelytapahtumanLaji

### AbstraktiLupamääräyksenLaji

### AbstraktiLähtötietoaineistonLaji

### AbstraktiPoikkeamisenLaji

### AbstraktiVuorovaikutustapahtumanLaji

### ArvonOperaattori

### AsiakirjanJulkisuusluokka

### DigitaalinenAlkuperä

### EhtolauseenKonnektiivi

### HenkilötietosisällönLaji

### KiinteistörakisteriyksikönLaji

### LupahakemuksenElinkaaritila

### LuvanElinkaaritila

### OikeusvaikutteisuudenLaji

### OsoitteenKäyttötarkoitus

### PäätöksenElinkaaritila

### PäätöksenTulos


[ISO-8601-1]: https://www.iso.org/standard/70907.html "ISO 8601-1:2019 Date and time — Representations for information interchange — Part 1: Basic rules"
[ISO-639-2]: https://www.iso.org/standard/4767.html "ISO 639-2:1998 Codes for the representation of names of languages — Part 2: Alpha-3 code"
[ISO-19103]: https://www.iso.org/standard/56734.html "ISO 19103:2015 Geographic information — Conceptual schema language"
[ISO-19107]: https://www.iso.org/standard/66175.html "ISO 19107:2019 Geographic information — Spatial schema"
[ISO-19108]: https://www.iso.org/standard/26013.html "ISO 19108:2002 Geographic information — Temporal schema"
[ISO-19109]: https://www.iso.org/standard/59193.html "ISO 19109:2015 Geographic information — Rules for application schema"
[ISO-19505-2]: https://www.iso.org/standard/52854.html "ISO/IEC 19505-2:2012, Information technology — Object Management Group Unified Modeling Language (OMG UML) — Part 2: Superstructure"
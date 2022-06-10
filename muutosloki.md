---
layout: "default"
description: ""
id: "muutosloki"
---
# Muutosloki

## 10.6.2022
Tuotu OsallistumisJaArviointisuunnitelma-luokka osaksi yhteisiä komponentteja. Tarvitaan täsmälleen samanlaisena ainakin Kaavatiedot-, Kaupunkiseutusuunnitelma- ja Rakennusjärjestys-tietomalleissa.

## 27.5.2022
Viety sekä Kaupunkiseutusuunnitelmassa että Rakennusjärjestyksessä tarvittava SuureenArvo ja siihen liittyvät luokat osaksi yhteisiä komponentteja.

* Lisätty luokat AbstraktiSuureenArvo, SuureenArvo, Suure, EhdollinenSuureenArvo, Ehtolause ja Ehto.
* Muutettu seuraavien attribuuttien nimet selkeyden vuoksi:
   * Ajanhetkiarvo.arvo -> aika,
   * Aikaväliarvo.arvo -> aikaväli,
   * GeometriaArvo.arvo -> geometria,
   * Koodiarvo.arvo -> koodi,
   * NumeerinenArvo.arvo -> numero,
   * Tunnusarvo.arvo -> tunnus ja
   * Tekstiarvo.arvo -> teksti
* Korvattu attribuutti Tietoyksikkö.arvo:OminaisuudenArvo attribuutilla Tietoyksikkö.ominaisuus:SuureenArvo ja poistettu AlueidenkäyttöJaRakentamismääräys.lisätieto. Käyttämällä SuureenArvo-luokkaa voidaan tarkemmin määrätä Tietoyksikön arvot, kun määrättävän arvo suure on eksplisiittisesti annettu. Näin samaan Tietoyksikköön voidaan lisätä useita eri suureiden arvoja
* Poistettu attribuutti MääräyksestäPoikkeaminen.poikkeavaLisätieto ja MääräyksestäPoikkeaminen.poikkeavaArvo, korvattu attribuutilla MääräyksestäPoikkeaminen.poikkeavaOminaisuus.
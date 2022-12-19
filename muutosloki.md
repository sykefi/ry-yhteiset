---
layout: "default"
description: ""
id: "muutosloki"
---
# Muutosloki

## 19.12.2022

* Lisätty RakennetunYmpäristönLupapäätös.päätöksenTulos, fixes #2.
* Lisätty uudet luokat Asiakirja (interface) ja Asiasana (DataType), muutettu Asiakirja-DataType rajapinnaksi (interface), fixes #3.
* Lisätty koodistot HenkilötietosisällönLaji, AsiakirjanJulkisuusluokka ja AsiakirjanSaavutettavuusluokka, viimeksi mainittu ilman viittausta Y-alustan koodistoon, fixes #4
* Lisätty AlueidenkäyttöJaRakentamisasia-luokkaan attribuutti diaarinumero, fixes #5
* Lisätty AlueidenkäyttöJaRakentamispäätös-luokkaan attribuutti ohjaavaSäädös:Säädösviite [0..*], fixes #6
* Lisätty RakennetunYmpäristönLupa-luokkaan attribuutti lupaTunnus:CharacterString, fixes #8
* Uudelleennimetty RakennetunYmpäristönLupa-luokan assosiaatiot siten, että luokalla ei ole kahva määräys-nimistä assosiaatiota, fixes #9.

## 8.12.2022

* Lisätty edellisessä päivityksessä vahingossa puuttumaan jäänyt UML-luokkakaaviosisältö.

## 29.11.2022

* Lisätty luokkaan AlueidenkäyttöJaRakentamispäätös uusi pakollinen attribuutti ```elinkaaritila```.

## 14.10.2022
Tarkennettu ehdollisen suuren arvon rakennetta. Muutos mahdollistaa monimutkaistenkin propositiologiikan ehtojen ilmaisemisen vaihtoehtoisten suureiden arvojen määräytymisen ehtoina.

* EhdollinenSuureenArvo on nyt uudelleennimetty kuvaavammin VaihtoehtoinenSuurenArvo-luokaksi.
* Luotu uusi luokka EhdollinenSuureenArvo, joka kuvaa yhtä vaihtoehtoista arvoista, jonka valinta riippuu ehdon totuusarvosta.
* Luotu uusi rajapintaluokka Ehto, jonka toteuttavat luokat Ehtolause ja SuureenArvoEhto (vastaa aiempaa Ehto-luokkaa).
* Korvattu negaatio-attribuutti yleisemmällä arvoOperaattori-attribuutilla ja luotu sille koodisto ArvonOperaattori.
* Poistettu tarpeettomaksi käynyt AbstraktiLisätiedonLaji-koodistoluokka.
* AbstraktiSuureenArvo-luokan arvo-attribuutin kardinaliteetti voi nyt olla enemmän kuin 1: konkreettinen tarve tähän tulee Kaavamääräyksistä, joissa tarvitaan useampia eri tyyppisiä arvoja, esim. [Vaatimus prof-ak/vaat-sallittu-kerrosala-lisatiedot](https://tietomallit.ymparisto.fi/kaavatiedot/soveltamisprofiili/asemakaava/v1.0/rakentamisen-maara-sijoittaminen/#prof-ak-vaat-sallittu-kerrosala-lisatiedot), jossa suureen "käyttötarkoituksen osuus kerrosalasta" tulee voida sisältää sekä käyttötarkoitukseen viittaavan koodiarvon että numeerisen arvon.

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
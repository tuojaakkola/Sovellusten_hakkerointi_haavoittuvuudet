# h2 Break & Unbreak.md

## OWASP 10
Broken access control: Käyttäjän pääsynhallinnan valvomisen epäonnistuminen. Eli käyttäjät voivat suorittaa toimintoja, jotka ovat heidän käyttäjäoikeuksiensa ulkopuolella. Tämä oli yleisin tietoturvavirhe 2021 OWASP Top 10-listassa.

## Karvinen 2023: Find Hidden Web Directories - Fuzz URLs with ffuf
ffuf on fuzzer, jolla voi automatisoida haavoittuvuuksien etsimisen web-sovelluksista. ffuf avulla voit valita kohteen ja syöttää sille sanalistan, jonka jälkeen se lähettää tuhansia pyyntöjä sivulle sekunneissa testaten kaikkia mahdollisia komboja sanalistasta esimerkiksi urliin.
Pyyntöjen jälkeen se kertoo eri pyyntöjen vastaukset joita voit tutkia ja tulkita. 

## PortSwigger: Access control vulnerabilities and privilege escalation
Verkkosovellusten kontekstissa pääsynhallinta on riippuvainen tunnistautumisesta ja istunnonhallinnasta. Se määrittää onko käyttäjän sallittua suorittaa toiminto, jota hän yrittää tehdä.
Tapoja estää pääsynhallinan haavoittuvuudete: 
Jos resurssin ei ole tarkoitus olla julkisesti saatavilla, kiellä pääsy oletuksena (deny by default).
Käytä aina yhtä sovelluksen laajuista mekanismia pääsynhallinnan valvontaan jos mahdollista.
Tee kooditasolla pakolliseksi, että kehittäjät määrittelevät kullekin resurssille sallitun pääsyn ja kiellä pääsy oletuksena.

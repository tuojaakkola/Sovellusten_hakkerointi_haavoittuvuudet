# Hardware Hacking

**Tehtävänanto:** [Haaga-Helia Moodle](https://hhmoodle.haaga-helia.fi/course/view.php?id=45171&section=3#tabs-tree-start)  
**Ympäristö:** Virtuaalikone (Ubuntu)

---

## Tehtävät

### 1) decrypt firmware image

Latasin tehtävänannon mukaisesti tp-link-decrypterin, jolla olisi tarkoitus muutttaa salattu 
firmware lataus, joka tehtävänannossa sanotaan lataamaan AWS S3 palvelimelta. 

Koitin ladata firmwarea suoraan linkillä joka tehtävänannossa oli, mutta koneeltani puuttui
aws cli jota lataukseen tarvitsi. Sain sen ladattua komennolla:

`sudo snap install aws-cli --classic`

Huomasin että tp-link-decrypterin kansiossa on makefile ja koitin ajaa sen komennolla "make".

<img width="512" height="114" alt="image" src="https://github.com/user-attachments/assets/3f391057-27d3-4059-9bf9-91a971987854" />

Se ei kuitenkaan vielö onnistunut vaan palautti virheen. Tunnilla tätä tehdessäni kuulin että
preinstall.sh sekä extract_keys.sh tiedostot tulee ajaa jotta tämän tp-link-decrypterin saa käännettyä.
Nämä tiedostot hakevat riippuvuudet ja tarvittavat avaimet ohjelmiston käännön toiminnalle.

<img width="512" height="322" alt="image" src="https://github.com/user-attachments/assets/f31b8622-3478-46e9-93fa-70aa6ba49316" />

extract_keys.sh tiedoston ajaminen törmäsi virheeseen ja vaati asentamaan työkalut jefferson ja ubireader.

Kokeilin erilaisia tapoja ladata noita työkaluja eri kommennoilla ja ajamalla extract_keys.sh uudestaan.
Lopulta sain työkalut asennettua komennoilla:

`sudo apt update`
`sudo apt install python3-pip python3-setuptools`
`sudo pip3 install jefferson ubi_reader --break-system-packages`

<img width="919" height="470" alt="image" src="https://github.com/user-attachments/assets/5155193c-e601-4806-a9cf-284cccac351e" />

Make komento meni läpi ja loi bin hakemiston eli ohjelma on nyt valmis käytettäväksi.

Nyt käytän tp-link-decrypteriä saamaan firmware tiedoston tutkittavaan muotoon. Teen sen ajamalla komennon:

`./bin/tp-link-decrypt ../Tapo_C200v5_en_1.2.3.bin`

<img width="919" height="470" alt="image" src="https://github.com/user-attachments/assets/5177ecf8-1ee3-4ada-baec-1794cf00ea4e" />

Komento onnistui ja loi decryptatun .dec tiedoston.

---

### 2) Analyse the image file

Luin annettua artikkelia ja kyselin tekoälyltä miten tälläistä tiedostoa olisi hyvä tutkia.
Artikkelissa mainittiin binwalk joten käytän sitä. Tutkin decryptattua tiedostoa binwalkilla komennolla "binwalk -e Tapo_C200v5_en_1.2.3.bin.dec". -e flagi komennossa purkaa analyysin omaan tiedostoonsa.

<img width="924" height="748" alt="image" src="https://github.com/user-attachments/assets/3ac8df60-8f22-4b8f-82f5-329abe9f448b" />
<img width="924" height="748" alt="image" src="https://github.com/user-attachments/assets/a67bdd81-16da-4552-a209-e17e1412a1af" />

Komento onnistui. Se analysoi tiedoston sisällön ja purki sen omaan extracted loppuiseen kansioonsa.

---

### 3) extract rootfs from the dump file

Suoritin saman komennon binwalkilla tehtävänannossa annetulle dump tiedostolle jolloin binwalk loi senkin analyysille oman extracted päättyvän tiedoston. Analyysi myös tunnisti tiedostosta Squashfs filesystemin. Artikkelissa kerrotaan että rootfs on SquashFS muodossa.

<img width="815" height="453" alt="image" src="https://github.com/user-attachments/assets/7c7f259d-7615-427c-bc13-02fa36d8e072" />

---

### 4) extract rootfs from the image file

2. kohdassa suoritimme jo binwalk -e komennon decryptatulle firmware tiedostolle. Analyysissä löytyi sama Squashfs filesystem, joka viime kohdassa todettiin laitteen juuritietojärjestelmäksi (root file system).

<img width="929" height="119" alt="image" src="https://github.com/user-attachments/assets/13706d1c-040b-49c1-9c6c-6c08a30c4e1e" />

---

### 5) search available applications

Tutkin binwalkin purkamaa analyysitiedostoa list komennolla.

<img width="927" height="757" alt="image" src="https://github.com/user-attachments/assets/815b0570-bb19-43a9-8d85-ff794a49b316" />

Komento listaa kansiot ja tiedostot jotka binwalk sai irti analyysissä. 

Ensimmäinen silmiinpistävä asia oli main tiedosto. 

<img width="930" height="43" alt="image" src="https://github.com/user-attachments/assets/d79aed0d-5326-428d-891a-0ddea5681a0e" />

Koitin ajaa sitä mutta se ei onnistunut vaan palautti suoritusmuoto virheen eli eivoi ajaa minun järjestelmässäni.

<img width="917" height="58" alt="image" src="https://github.com/user-attachments/assets/cc7851bc-238e-4a8d-89fc-bf74fe9dcf79" />

Ilmeisesti main binääriä tutktimalla salasanan voisi löytää. 

---

Tehtävään käytettävä aikani kuitenkin loppui tässä vaiheessa joten osa 6 ja salasanan selvittämien jäi tekemättä.

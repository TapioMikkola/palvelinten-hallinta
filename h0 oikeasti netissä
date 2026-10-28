# h0 oikeasti netissä

## a) Asenna Debian 13 paikallisesti virtuaalikoneeseen. 

Tein tämän harjoituksen mac m1 koneella. Kurssin materiaalista löytyi tämä avuksi macin käyttäjille: https://github.com/HortTemppa/horttemppa.github.io/blob/main/h1.md
Virtual Boxin sijasta UTM asennukseen osoitteesta https://getutm.app/ ja Debian 13 arm64 asennuslevy osoitteesta https://www.debian.org/distrib/netinst
Katsoin vielä varmuuden vuoksi pr0c:n videon aiheesta: https://www.youtube.com/watch?v=S8hR0CD4Kmc

Virtuaalikoneen alustuksen tein kurssin materiaalien mukaan. Ensimmäinen ongelma tuli eteen, kun Debianin asennuksen en jälkeen päässyt boottaamaan työpöydälle vaan asennus
alkoi alusta. Hetken pähkäilyn ja pr0cin videon katsomisen jälkeen tajusin poistaa asennuslevyn ja pääsin boottaamaan debianin omilla käyttäjäasetuksilla. 

## Debianin käyttö ensimmäistä kertaa.

Ensimmäisenä terminaali auki ja komento sudo apt-get upgrade. "User is not in the sudoers file". Käännyin chatGPT:n puoleen ja kysäisin neuvoa. 
Neuvoksi tuli suorittaa komento su - , jotta päästään root käyttäjälle. Seuraavaksi komento usermod -aG sudo omakäyttäjänimi. Tämän jälkeen kirjauduin ulos debianista 
ja takaisin sisään, jotta muutokset astuivat voimaan. Uuden karheilla super user -voimillani avasin terminaalin ja komennolla ping -c 4 google.com testasin, 
että virtuaalikoneeni pääsee nettiin. Lopuksi annoin komennon sudo apt full-upgrade. 

## f) Vaihtoehtotehtävät apache2 asennus yms.
  
En viitsinyt alkaa maksamaan yksityispalvelimesta joten tein kaiken paikallisesti virtuaalikoneessa.  
Asensin apache2:n komennolla sudo apt-get install apache2. Testasin virtuaalikoneen firefoxissa http://localhost se näytti onnistuneesti apache2 debian default sivua. 
Kokeilin myös terminaalissa komennolla curl -I http://locahost ja tila oli ok eli Apache vastasi HTTP pyyntöön mallikkaasti. Seuraavaksi asensin UFW palomuurin ja sallin
SSH- ja HTTP-liikenteen. sudo apt-get install ufw, sudo ufw allow OpenSSH, sudo ufw allow 80/tcp ja sudo ufw enable. 

Teron ohjeiden mukaisesti Userdir servulle komennolla sudo a2enmod userdir ja sen jälkeen sudo systemctl restart apache2. Menin kotihakemistoon komennolla cd ja loin
uuden kansion julkisille html tiedostoille komennolla mkdir public_html. Heitin sinne testisivun komennolla echo "toimi pliis" > ~/public_html/index.html. Testasin toimivuutta
selaimessa osoitteessa http://localhost/~munkäyttäjä/, mutta tuli vastoinkäyminen. "403 FORBIDDEN". Kysäisin taas tekoälyltä mikä auttaisi pulmaani ja muokkasin hakemiston
ja tiedoston käyttöoikeuksia komennoilla chmod 711 ~, chmod 755 ~/public_html, chmod644 ~/public_html/index.html. Jotenkin tämän jälkeen http://localhost/~munkäyttäjä/ näytti 
selaimessa sivun jonka ylälaidassa luki "toimi pliis". Onnistui.

## Nimipalvelun simuloiminen /etc/hosts -tiedostolla.  

Lopulta nimipalvelun simulointi. Komento sudo nano /etc/hosts ja lisäsin sinne rivin 127.0.0.1 jihuujahuu.test. Testasin firefoxissa http://jihuujahuu.test ja Apachen
oletussivusto aukesi. 

Lähteet:
ChatGPT: 2026, Promptit: "Im trying to sudo apt-get updates, but user is not in the sudoers file." "firefox "http://localhost/~munkäyttäjä/" komento avaa selaimessa
403 FORBIDDEN you do not have permission to access this resource Apache/2.4.69 (Debian) Server at localhost port 80."
Karvinen, Tero 2021: Install Debian on VirtualBox. https://terokarvinen.com/2021/install-debian-on-virtualbox/
Karvinen, Tero 2020: Command Line Basics Revisited. https://terokarvinen.com/2020/command-line-basics-revisited/
Karvinen, Tero 2008: Install Apache Web Server For Ubuntu: https://terokarvinen.com/2008/05/02/install-apache-web-server-on-ubuntu-4/index.html
Teemu Laine's instructions for running Debian 11 with UTM on Mac M1: https://github.com/HortTemppa/horttemppa.github.io/blob/main/h1.md
  

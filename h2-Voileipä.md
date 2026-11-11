# h2 Voileipä
x) 
-Ansible tarvitsee rootin orjalle.
-Yksi tapa on sallia sudo ilman salasanaa
-Sarjakuva, joka helpottaa sudon hahmottamista.
-Salasanattoman sudon luominen Ansiblella. 
-Roolin luominen komennolla 

    $ micro roles/antero/tasks/main.yml
    $ ansible-playbook site.yml -K

Copy:
-content: tiedoston sisältö
-dest: kohdepolku, johon tiedosto kopioidaan
-src: kopiotavan tiedoston lähde eli source
-owner: tiedoston omistaja
-group: tiedoston käyttäjäryhmä
-mode: tiedoston käyttöoikeudet

Apt:
-name: paketin nimi
-state: haluttu tila present, absent, latest
-update_cache: päivittää tai on päivittämättä pakettia ennen asennusta

File:
-path: polku tiedostoon tai hakemistoon
-recurse: muuttaa oikeudet rekursiivisesti alihakemistoihin
-src: lähdepolku
-state: haluttu tila file,directory, link, absent, touch
-owner: omistaja
-group: käyttäjäryhmä
-mode: käyttöoikeudet

User:
-name: käyttäjän nimi
-create_home: luo kotihakemiston
-comment: käyttäjän lisätiedot
-groups: käyttäjän ryhmät
-shell: käyttäjän komentorivi
-state: käyttäjätilin tila present tai absent
-system: luodaanko järjestelmäkäyttäjä

Authorized_key:
-user käyttäjä, jolle avain lisätään
-key: julkinen avain

## a) Sudoless

Loin uuden käyttäjän Teron mallin mukaan. https://terokarvinen.com/passwordless-sudo/

    $ sudo adduser antero
    $ sudo groupadd sudoless
    $ sudo adduser antero sudoless

Loin säännön, jotta sudoless ryhmän käyttäjät voivat käyttää sudoa ilman salasanaa:

    $ sudo visudo /etc/sudoers.d/sudoless
    $ %sudoless ALL = (ALL) NOPASSWD: ALL

Kirjauduin ulos ja takaisin sisään, jotta muutokset astuivat voimaan. Kokeilin voiko antero sudotella menemään ilman salasanaa kirjautumalla anterolle ssh:n kautta:

<img width="572" height="57" alt="Screenshot 2026-11-11 at 5 37 17" src="https://github.com/user-attachments/assets/c82aa7a6-427b-421f-8c08-c0e05f8048ec" />

## b) Antero. Salasanaton, automaattisesti ssh:lla kirjautuva tunnus Ansiblella

Loin käyttäjän antero ja sen jälkeen microlla tehtävätiedoston Teron pohjan mukaan.

    $ mkdir roles/antero/tasks
    $ micro roles/antero/tasks/site.yml

<img width="1011" height="417" alt="Screenshot 2026-11-11 at 7 18 34" src="https://github.com/user-attachments/assets/cc664330-d77b-4c16-b83e-0d072c850507" />

Seuraavaksi lisäsin site.yml tiedostoon 'become: true', jotta Ansible yrittäisi sudo-oikeuksia. Roolien alle lisäsin anteron. 

<img width="380" height="107" alt="Screenshot 2026-11-11 at 10 13 01" src="https://github.com/user-attachments/assets/ae954cc4-3bf7-481a-87e2-7359183cc7b5" />

Annoin komennon 

    $ ansible-playbook site.yml

Mutta se ei toiminut ilman -K lisäystä komentoon. -K eli --ask-become-pass antoi Ansiblelle tarvittavat korotetut käyttöoikeudet.

<img width="1008" height="508" alt="Screenshot 2026-11-11 at 7 08 52" src="https://github.com/user-attachments/assets/974f251b-2bfe-4ab4-84e5-5c73514b8628" />

## c) Package

Loin edellisen kohdan mukaisestti roolin 'package'. 

    $ mkdir -p roles/package/tasks

Ja lisäsin sinne seuraavanlaisen main.yml -tiedoston:

<img width="318" height="171" alt="Screenshot 2026-11-11 at 8 03 10" src="https://github.com/user-attachments/assets/8d716636-2e9a-48a3-9b4c-5e537f09d70c" />

Tämän jälkeen lisäsin roolin 'package' site.yml tiedostoon. Ajoin ansible-playbook site.yml -K. Testasin toimiko cowsay ja apache:

<img width="812" height="487" alt="Screenshot 2026-11-11 at 8 15 20" src="https://github.com/user-attachments/assets/df3bdfc2-b6da-4c1a-bb3c-e16499fbbfe2" />

Näytti toimivan.

## d) File

Jälleen sama alku kuin aikaisemmissa kohdissa. 

<img width="572" height="417" alt="Screenshot 2026-11-11 at 8 29 11" src="https://github.com/user-attachments/assets/1542ac46-db36-4b11-b2a6-a8b3c55c84fc" />

Loin seuraavanlaisen main.yml tiedoston:

<img width="598" height="324" alt="Screenshot 2026-11-11 at 8 48 40" src="https://github.com/user-attachments/assets/82e46e92-b611-4aa3-8112-5ebcb5f8f200" />

mode: "0640" tarkoittaa että omistaja (antero) saa kirjoittaa ja lukea tiedostoa, ryhmän jäsenet (sudoless) saavat lukea ja muut eivät saa mitään oikeuksia. 

Ajoin jälleen ansible-playbook site.yml -K. Edellisessä kohdassa asennettu cowsay koristeli tulostusta.

<img width="1050" height="578" alt="Screenshot 2026-11-11 at 8 50 36" src="https://github.com/user-attachments/assets/cd2ffb38-5a02-42f4-bd12-b4f9982fe34f" />

Tarkistin toimiiko tiedosto loogisesti. Anteron pitäisi pystyä kirjoittamaan ja lukemaan ja sudoless ryhmän pitäisi pystyä lukemaan. Pääkäyttäjä tapio ei ole omistaja tai kuulu sudoless ryhmään, joten tapiolla ei pitäisi olla mitään oikeuksia. 

<img width="560" height="81" alt="Screenshot 2026-11-11 at 8 53 30" src="https://github.com/user-attachments/assets/0fa766c7-5290-4010-a52a-72979ef04cf2" />

<img width="326" height="92" alt="Screenshot 2026-11-11 at 8 55 11" src="https://github.com/user-attachments/assets/669300e3-87b1-4114-9b98-5394cc0836ac" />

## e) Jotain muuta

Loin edellisten kohtien mukaisesti roolin 'debug' ja lisäsin uuden roolin site.yml tiedostoon. Nyt ansible-playbook site.yml -K tulosteessa pitäisi olla viesti "Ansible toimii!". 

<img width="358" height="467" alt="Screenshot 2026-11-11 at 11 19 03" src="https://github.com/user-attachments/assets/77768973-e43f-4038-85c2-2f0da38b5cfe" />

<img width="268" height="79" alt="Screenshot 2026-11-11 at 11 11 25" src="https://github.com/user-attachments/assets/525c02b6-aeee-4ab7-b29e-6418c64d5a21" />

<img width="382" height="161" alt="Screenshot 2026-11-11 at 11 20 37" src="https://github.com/user-attachments/assets/d7a1b603-f796-41bd-a9bf-95cc342ac8b2" />

Kokeilin ajaa 

    $ ansible-playbook site.yml -K

ja lopputuloksena:

<img width="1029" height="473" alt="Screenshot 2026-11-11 at 11 18 24" src="https://github.com/user-attachments/assets/7092f732-6ae2-406f-97c8-b8135cfa3eeb" />

Debug viesti toimii! 

## Lähteet:

Ansiblen sisäänrakennettu dokumentaatio
Karvinen, Tero 2026: Passwordless Sudo with Ansible. https://terokarvinen.com/passwordless-sudo-with-ansible/
Karvinen, Tero 2026: Sudo without password. https://terokarvinen.com/passwordless-sudo/
Microsoft Co-pilot tiettyjen sanojen suomentamisessa ja englannistamisessa ja neuvonantajana kohta e:hen


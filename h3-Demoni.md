#H3 Demoni

x) Apache2 asennus Ansiblella
-muutokset tulevat voimaan vasta kun apache2 käynnistetään uudelleen.
-handlers-roolin tehtävänä on uudelleenkäynnistää Apache2 Ansiblea ajaessa.

Handlers: running operations on change
-Verify-apache.yml esimerkissä handler käynnistää apache2:n uudellen kun kaikki playbookin taskit on suoritettu.
-Tasks-rooli voi ilmoittaa yhdelle tai useammalle handlerille execute-komennon avainsanalla "notify"

Ansible-doc service
-service-moduulilla voidaan käynnistää, pysäyttää ja restartata sovelluksia
-enabled: käynnistyykö palvelu automaattisesti järjerstelmän käynnistyessä (y/n)
-name: hallittavan palvelun nimi
-state: hallittavan palvelun tila (started, stopped, restarted, reloaded)
-EXAMPLES: miten kaikki käytännössä toimii Ansiblessa 

##y) Infrauutiset

Goodbye AWS: How We Kept ISO27001, Slashed Costs by 90% https://medium.com/@accounts_73078/goodbye-aws-how-we-kept-iso-27001-slashed-costs-by-90-914ccb4b89fc
-Tanskalainen firma kyllästyi AWS:n kuluihin ja koko operaation riippuvuuteen yhdelle amerikkalaisyhtiölle.
-Infra vaihdettiin eurooppalaisille Hetznerille ja OVHCloudille
-AWS-palvelut korvattiin Ansiblella, Prometheuksella, Grafanalla ja Lokilla
-Kuluista saatiin leikattua 90%


##a) Apassi

Paukutin Teron ohjeiden mukaisesti: https://terokarvinen.com/apache-ansible/

Apache2 oli jo valmiiksi asennettuna, joten loin roolin apache2 ja sen alle tarvittavat files, handlers ja tasks -hakemistot komennolla 

    $ mkdir -p roles/apache2/{files, handlers, tasks}

<img width="189" height="74" alt="Screenshot 2026-11-18 at 2 25 36" src="https://github.com/user-attachments/assets/df2df5cf-e0fa-40cf-8739-2f023fd53019" />

Loin jokaiseen main.yml tiedoston Teron ohjeiden mukaisesti:<img width="705" height="212" alt="Screenshot 2026-11-18 at 5 06 08" src="https://github.com/user-attachments/assets/93252fe7-4ce7-4076-a54d-bc91089ed89a" />
<img width="671" height="105" alt="Screenshot 2026-11-18 at 5 06 54" src="https://github.com/user-attachments/assets/16e7e492-78c4-4e44-83db-622ae8bf6081" />
<img width="716" height="403" alt="Screenshot 2026-11-18 at 5 07 40" src="https://github.com/user-attachments/assets/9b5b607e-1063-4efa-8922-65ba866cfb50" />

Loin kotihakemistoon publicsite hakemiston ja sinne html-tiedoston komennolla:

    $ echo "<h1>Apache-saitti</h1>" > ~/publicsite/index.html

Lisäsin lopuksi apache2 roolin site.yml tiedostoon ja ajoin playbookin. 

<img width="982" height="286" alt="Screenshot 2026-11-18 at 5 17 37" src="https://github.com/user-attachments/assets/eba09572-e51f-4878-9060-0b4e3a0ac730" />

Testasin curl komennolla näkyykö publicsite/index.html:

<img width="431" height="75" alt="Screenshot 2026-11-18 at 3 59 04" src="https://github.com/user-attachments/assets/a9a7ce41-c8eb-4bc8-ad0d-c5b706ab2624" />


##b) Moottorix

Sammutin ensiksi apachen:

    $ sudo systemctl stop apache2
    $ sudo systemctl disable apache2

Sen jälkeen asensin Nginx:

    $ sudo apt update
    $ sudo apt install nginx

Asennuksen jälkeen starttasin Nginxän ja tarkistin että se toimii:

<img width="767" height="93" alt="Screenshot 2026-11-18 at 5 31 47" src="https://github.com/user-attachments/assets/f5bcac5b-7191-4a77-8e69-09756f94969c" />

<img width="977" height="364" alt="Screenshot 2026-11-18 at 5 55 54" src="https://github.com/user-attachments/assets/b647ebde-600e-4e1e-944d-4755ef5c3286" />

Tein oman sivustokonfiguraation komennolla:

    $ sudo nano /etc/nginx/sites-available/publicsite

<img width="737" height="228" alt="Screenshot 2026-11-18 at 6 14 43" src="https://github.com/user-attachments/assets/c2fe4def-62e1-4a97-9edb-f8baa964be2f" />


Poistin Nginx oletussivun käytöstä ja loin symlinkin. Testasin että syntaksi on ok.

<img width="973" height="144" alt="Screenshot 2026-11-18 at 6 41 34" src="https://github.com/user-attachments/assets/a87892b5-3b8a-45c9-abd2-ebb2622a4152" />


Reloadasin Nginx:än ja totesin että kaikki näyttää vieläkin toimivan.

<img width="954" height="161" alt="Screenshot 2026-11-18 at 6 48 30" src="https://github.com/user-attachments/assets/925f71c4-9317-4897-9ec6-691848b710f7" />


Testasin curl komennolla toimivuutta ja muutin /publicsite/index.html tiedostoa ilman sudoa tai roottia ja testasin curlilla sekä selaimessa.

<img width="519" height="186" alt="Screenshot 2026-11-18 at 6 32 00" src="https://github.com/user-attachments/assets/205fd701-ee41-44bd-94b7-708d52e7eda7" />
<img width="714" height="112" alt="Screenshot 2026-11-18 at 6 55 17" src="https://github.com/user-attachments/assets/5521a3d9-f855-4502-b2dc-a6d5062d55b2" />

##c) Automoottorix

Loin roolin nginx ja sinne tarvittavat hakemistot komennolla:

    $ mkdir -p roles/nginx/{files, handlers, tasks}

tasks/main.yml:


<img width="909" height="794" alt="Screenshot 2026-11-18 at 7 19 02" src="https://github.com/user-attachments/assets/3046861a-16c7-4b0f-8f9c-4d2a1de4aa0a" />


handlers/main.yml:

<img width="707" height="129" alt="Screenshot 2026-11-18 at 7 20 50" src="https://github.com/user-attachments/assets/47f9d7b4-d3fd-46ca-bd93-ace93af095fc" />


files/publicsite.conf:

<img width="705" height="239" alt="Screenshot 2026-11-18 at 7 07 19" src="https://github.com/user-attachments/assets/7753d130-85bb-46e5-a8e1-c2509a027c11" />


Lopullinen rakenne:

<img width="323" height="182" alt="Screenshot 2026-11-18 at 7 26 16" src="https://github.com/user-attachments/assets/87802cd8-9a6a-4cd0-b027-c7a4c12b107b" />


Lisäsin nginx-roolin site.yml-tiedostoon ja ajoin ansible-playbookin:

<img width="984" height="634" alt="Screenshot 2026-11-18 at 7 29 19" src="https://github.com/user-attachments/assets/bfeb320f-b991-42f4-804c-2fcefa5a8e4d" />



Testasin vielä curl http://localhost sekä selaimessa, että päivitetty nginx-sivusto pyörii. 

<img width="475" height="193" alt="Screenshot 2026-11-18 at 7 55 10" src="https://github.com/user-attachments/assets/bdf7ff76-437a-4f69-961f-5781e50358bf" />




##Lähteet

Karvinen 2026: Apache installed with Ansible https://terokarvinen.com/apache-ansible/
Ansible Community Documentation 2026: Handlers: running operations on change https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html
Tekoäly Microsoft Co-pilot ja ChatGPT neuvonantoa nginx:n ajamisesta ansiblella

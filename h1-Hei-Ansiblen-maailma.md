# h1 Hei Ansiblen maailma!

a) Asensin Debian 13 virtuaalikoneeseen edellisessä kotitehtävässä: https://github.com/TapioMikkola/palvelinten-hallinta/blob/main/h0-oikeasti-netiss%C3%A4.md

b&c) Asensin ssh demonin ohjeiden mukaisesti: https://terokarvinen.com/ssh-public-key-login-without-password/

    $ sudo apt-get update
    $ sudo apt-get -y install ssh

Annoin komennon, jotta ssh demoni starttaa boottauksen yhteydessä ja nyt. 

    $ sudo systemctl enable --now ssh

Loin avainparin komennolla ssh-keygen ja kopioin sen hostille komennolla ssh-copy-id localhost.

    $ ssh-keygen
    $ ssh-copy-id localhost

d) Hei Ansible. 

Tein maailman ansiblella Teron ohjeiden mukaisesti: https://terokarvinen.com/hello-ansible/

Ansiblen asennus:

<img width="718" height="146" alt="Screenshot 2026-11-04 at 4 43 27" src="https://github.com/user-attachments/assets/b1563cf3-441a-4d45-a9d6-a39ec95e069b" />

Loin kansion Ansiblen konfiguroinnille. 

    $ cd
    $ mkdir ansible/
    $ cd ansible/

Loin hosts.ini tiedoston ja lisäsin sinne localhostin. 

<img width="410" height="58" alt="Screenshot 2026-11-04 at 4 54 57" src="https://github.com/user-attachments/assets/d55be03e-c4ba-4825-a233-9af961b85ac9" />

Laitoin Ansiblen suorittamaan komennon 'uptime' kaikilla hosteilla. Lisäsin hosts.ini tiedostoon mitä python interpreteriä Ansiblen tulee käyttää, jottei tarvitse aina katsella viestiä python versioista.

<img width="912" height="290" alt="Screenshot 2026-11-04 at 5 10 28" src="https://github.com/user-attachments/assets/54674cc3-63b4-4abf-8875-c0603bec57bc" />

Loin site.yml -tiedoston, joka listasi mikä host saa minkäkin roolin. Kaikille hosteille rooli "hello".

<img width="406" height="90" alt="Screenshot 2026-11-04 at 5 33 01" src="https://github.com/user-attachments/assets/d66585fb-2eae-4f30-af15-d26da7869011" />

Annoin komennon:

    $ ansible-playbook site.yml

Asible-playbook osasi lukea tiedoston, mutta roolia "hello" ei ole vielä luotu. Loin sen komenniolla:

    $ mkdir -p roles/hello/tasks/
    $ micro roles/hello/tasks/main.yml

<img width="915" height="285" alt="Screenshot 2026-11-04 at 5 42 35" src="https://github.com/user-attachments/assets/0aff6676-3097-4d43-b9e7-d0491b684b0c" />

Kokeilin uudestaan komentoa $ ansible-playbook site.ytml ja sain seuraavan tulosteen:

<img width="912" height="245" alt="Screenshot 2026-11-04 at 5 46 47" src="https://github.com/user-attachments/assets/d117785a-f146-4005-ab36-f80c2c3c1101" />

Jos kaikki meni oikein, komennolla 

    $ ssh localhost 'cat /tmp/hello-ansible' 

pitäisi tulostua "Hello world!"

<img width="654" height="41" alt="Screenshot 2026-11-04 at 5 51 20" src="https://github.com/user-attachments/assets/7f1f0d2c-e6fe-4791-ace6-5509e96868bb" />

## Lähteet

Karvinen 2026, Hello Ansible https://terokarvinen.com/hello-ansible/
Karvinen 2026, SSH public key - Login without password https://terokarvinen.com/ssh-public-key-login-without-password/

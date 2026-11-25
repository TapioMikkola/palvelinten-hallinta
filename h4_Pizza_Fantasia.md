# x)

-4.12.1 Salt ominaisuuksien määrä - 510, Puppet ominaisuuksien määrä - 113

-4.12.2 Mitä oikeasti käytetään?

    case
    file
    package
    exec
    service
    fw::rules
    registry_value
    if
    registry::value
    class
    anchor
    packages::pkgdmg

    augeas
    pam::changeparm
    node
    unmask-replace
    set-mount-options
    group
    filebucket
    cron

-4.12.3.1 Tärkeimmät rakennuspalikat
Funktiot: package, file, service (demonien ja applikaatioiden setup) user, group (käyttäjien hallinnointi) file, directory, symlink (tiedostojen käsittely)

## a) Räpylä

Asensin mariadb-palvelimen. 

    $ sudo apt update
    $ sudo apt install -y mariadb-server

Tarkistin asennuksen jälkeen mariadb:n tilan.

<img width="712" height="76" alt="Screenshot 2026-11-25 at 7 15 09" src="https://github.com/user-attachments/assets/0191eb5b-a9d7-4371-b002-bd0687068558" />

Avasin mariadb:n komennolla

    $ sudo mariadb

Tämän jälkeen loin testitietokannan mariadb:ssä komennolla

    CREATE DATABASE testi;

jonka jälkeen katsoin, että tietokanta luotiin ja valitsin testitietokannan.

    SHOW DATABASES;
    USE testi;

Loin testitaulun ja lisäsin testitiedon jonka jälkeen katsoin näkyykö lisätty tieto. Alla koko ruljanssi kaikessaan. 

<img width="453" height="651" alt="Screenshot 2026-11-25 at 7 33 48" src="https://github.com/user-attachments/assets/08ed2908-06b6-4c86-90cb-22a97fe414a3" />

## b) Automaatti

Loin oman kansion ja siirryin sinne työstämään.

    $ mkdir ~/ansible-mariadb
    $ cd ~/ansible-mariadb

Loin install_mariadb.yml tiedoston nanolla.

<img width="434" height="436" alt="Screenshot 2026-11-25 at 7 55 26" src="https://github.com/user-attachments/assets/3cf20e10-c7fd-46bc-b24a-fa96498cef9a" />

Tämän jälkeen ajoin playbookin. 

<img width="901" height="452" alt="Screenshot 2026-11-25 at 8 00 13" src="https://github.com/user-attachments/assets/f5d01da9-7c97-4ac7-88a4-06bb85f92e50" />

## c) Asetus

Muokkasin mariaDB:n asetuksia lisäämällä install_mariadb.yml tiedostoon taskin:

<img width="487" height="120" alt="Screenshot 2026-11-25 at 8 35 42" src="https://github.com/user-attachments/assets/7db47d0c-0c35-401e-aed4-948da8597941" />

Ja handlerin:

<img width="286" height="107" alt="Screenshot 2026-11-25 at 8 35 54" src="https://github.com/user-attachments/assets/9f332bbf-e9cd-4dab-ad9a-d18a0c52664c" />

Ajoin uudestaan playbookin komennolla

    $ ansible-playbook --ask-become-pass install_mariadb.yml

<img width="902" height="540" alt="Screenshot 2026-11-25 at 8 28 18" src="https://github.com/user-attachments/assets/ce8bc521-7d6f-4e38-8de8-7f8429e24493" />

Tarkistin, että asetukset tulivat käyttöön:

    $ sudo mariadb -e "SHOW VARIABLES LIKE 'max_connections';"

<img width="714" height="108" alt="Screenshot 2026-11-25 at 8 42 55" src="https://github.com/user-attachments/assets/b8cf651f-a27a-4398-8835-282bb3fd0b3b" />

## d) Paikka remonttiin

Poistin mariadb:n asennuksen komennolla

    $ sudo apt-get purge mariadb-server -y

Tarkistin, että mariadb on poistunut keskuudestamme:

<img width="868" height="94" alt="Screenshot 2026-11-25 at 8 55 12" src="https://github.com/user-attachments/assets/ebc508a2-9bb7-40a3-a631-e80c9955f7d6" />

Ajoin ansible playbookin ja testasin, että kaikki toimii niinkuin aiemminkin:

<img width="913" height="526" alt="Screenshot 2026-11-25 at 8 59 37" src="https://github.com/user-attachments/assets/e14a93b3-240f-48e4-9384-c40f987c25f8" />

## e) Idempotentti

Ajoin playbookin uudelleen ja mikään ei muuttunut:

<img width="904" height="380" alt="Screenshot 2026-11-25 at 9 18 46" src="https://github.com/user-attachments/assets/4e9a8669-2957-4af2-93ff-85c5b4e6fabf" />

## f)

Pari on hankittu.

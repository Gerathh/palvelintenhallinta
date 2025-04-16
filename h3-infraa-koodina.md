# h3 Infraa koodina

## x)
Karvinen 2014:  [Hello Salt Infra-as-Code](https://terokarvinen.com/2024/hello-salt-infra-as-code/)

 - Sivustolla kerrotaan miten saltilla voidaan ohjata jopa tuhansia koneita
 - Sivustolla tehdään esimerkki Hello world tekstitiedosto jamvarmistetaan sen olemassaolo minion  -koneella
 - Luodaan hello moduuli
 - Kerrotaan esimerkein mitä koodiin tulee kirjoittaa

Salt contributors: [Salt overview](https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml)

 - YAML renderer on oletus renderöintitapa kun käytetään Salttia
 - Sivustolla mainitaan useita sääntöjä joita ei saa käyttää YAML syntaksia käytettäessä kuten tabulaattori
 - Isoilla ja pienillä merkeillä ON väliä


## a)

#### Infrakoodi, paikallinen testaus

    cd /
    cd /srv
    sudo mkdir salt
    cd salt
    sudo mkdir hello
    cd hello
    sudo nano init.sls

Tänne lisäsin koodin:

    fallback
    /tmp/hellojere:
    file.managed

Suoritin komennon:

`
sudo salt-call --local state.apply hello`   

![local](https://github.com/Gerathh/palvelintenhallinta/blob/main/h31.png)

Tarkistin että tiedosto oli luotu /tmp/ kansioon.

## b)

#### Verkon yli

Ajoin komennon

    sudo salt 't001' state.apply hello

![Verkon yli](https://github.com/Gerathh/palvelintenhallinta/blob/main/h32.png)

## c)

#### Tilafunktiot verkon yli


Loin tiedoston:

    /srv/salt/demo/init.sls

Kirjoitin sinne seuraavan:

    create_demo_user:
      user.present:
        - name: demo
        - shell: /bin/bash
    
    create_welcome_file:
      file.managed:
        - name: /home/demo/welcome.txt
        - contents: |
            Hei demokäyttäjä!
            Käytin Salttia luodakseni tämän.
        - user: demo
        - group: demo
        - mode: '0644'
        - require:
          - user: create_demo_user
    
    ensure_cron_running:
      service.running:
        - name: cron
        - enable: True

Tämän jälkeen komento:

    sudo salt 't001' state.apply demo

![toimii](https://github.com/Gerathh/palvelintenhallinta/blob/main/h33.png)

Tarkistan toimivuuden:

    sudo salt 't001' cmd.run 'id demo'
![Toimii](https://github.com/Gerathh/palvelintenhallinta/blob/main/h34.png)

    sudo salt 't001' cmd.run 'cat /home/demo/welcome.txt'
    sudo salt 't001' cmd.run 'systemctl status cron'

![Nain se menee](https://github.com/Gerathh/palvelintenhallinta/blob/main/h35.png)

### Lähteet

[Tero Karvinen  Hello Salt Infra-as-Code](https://terokarvinen.com/2024/hello-salt-infra-as-code/)   

[Saltin käyttöohjeet](https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml)   

Yleensäkin nämä sivut olivat hyvät lähteenä https://docs.saltproject.io/

# h4 Pkg-file-service

#### aika 0:00

## x)
-Artikellissa käsitellään sitä miten Daemoneja kontrolloidaan Saltilla
-Artikkelissa näytetään yksinkertainen Salt tila miten SSH portti muutetaan

## a) Apache easy mode. Asenna Apache, korvaa sen testisivu ja varmista, että demoni käynnistyy



Kopioin tyhjän debian koneen.

Annoin käskyt:

`sudo apt update`

`sudo apt install apache2 -y`

`echo "Hei" | sudo tee /var/www/html/index.html`

sudo systemctl start apache2

![localhost](https://github.com/Gerathh/palvelintenhallinta/blob/main/h41.png "localhost")

Sitten ajoin seuraavat käskyt:

`sudo systemctl stop apache2`
`sudo apt-get remove --purge apache2 -y`
`sudo rm -rf /var/www/html/index.html`
`sudo apt-get install salt-master salt-minion -y` 
`sudo systemctl enable --now salt-master`
`sudo systemctl enable --now salt-minion`
`sudo nano /etc/salt/minion`

Lisäsin yo. tiedostoon rivin
`master: localhost`
`id: minion`
Sitten käynnistin salt-minionin uudelleen:
`sudo systemctl restart salt-minion`

Avainten hyväåksyntä:
`sudo salt-key -L`
`sudo salt-key -A`

Testaan yhteyden:
`sudo salt '*' test.ping`

Sitten:

`sudo mkdir -p /srv/salt/apache`
`sudo mkdir -p /srv/salt/ssh`

Sitten loin tiedoston
`sudo nano /srv/salt/apache/apache_easy.sls`

Lisäsin tiedostoon seuraavat tiedot:
    
        apache2:
          pkg.installed
        
    apache2:
      pkg.installed: []
    
    /var/www/html/index.html:
      file.managed:
        - source: salt://apache/index.html
        - mode: '0644'
        - user: root
        - group: root
    
    apache2_service:
      service.running:
        - name: apache2
        - enable: True
        - require:
          - pkg: apache2
          - file: /var/www/html/index.html
        
    
Sitten index.html:

`sudo nano /srv/salt/apache/index.html`

Jonne kirjoitin:
`<h1>Terve Salt!</h1>`


Ajoin:
`sudo salt '*' state.apply apache_easy`

![Toimii](https://github.com/Gerathh/palvelintenhallinta/blob/main/42.png "Toimii")

## b)

Ajoin seuraavat käskyt:

`sudo apt install openssh-server -y`
`sudo nano /etc/ssh/sshd_config`

Lisäsin rivit:

    Port 22
    Port 1234
    

Sitten käskyt:

`sudo systemctl restart ssh`
`nc -vz localhost 1234`

Sitten:

`sudo nano /srv/salt/sshouto.sls`

Tänne lisäsin tiedot:

    openssh-server:
      pkg.installed: []
    
    /etc/ssh/sshd_config:
      file.managed:
        - source: salt://ssh/sshd_config
        - mode: '0644'
        - user: root
        - group: root
    
    sshd_service:
      service.running:
        - name: ssh
        - enable: True
        - watch:
          - file: /etc/ssh/sshd_config
    


Sitten asetustiedosto saltille:

`sudo nano /srv/salt/ssh/sshd_config`


Tänne kirjoitin:

    Port 22
    Port 1234
    PermitRootLogin no
    PasswordAuthentication yes
    UsePAM yes
    

Sitten:
`sudo salt '*' state.apply sshouto`

![ssh](https://github.com/Gerathh/palvelintenhallinta/blob/main/44.png "ssh")

Ajoin vielä:

`ssh -p 1234 $(whoami)@localhost`

Sain vastauksen 


![sshhh](https://github.com/Gerathh/palvelintenhallinta/blob/main/45.png "sshhh")


### Lähteet

https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh   Yleiset ohjeet   
https://docs.saltproject.io/salt/  saltin toiminnnan ohjeet   


edit: 22.4 11:42 korjattu kirjoitusvirhe



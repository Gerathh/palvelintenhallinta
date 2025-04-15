### h2 soitto kotiin

---

**0:00**

## x)

[Two Machine Virtual Network With Debian 11 Bullseye and Vagrant](https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/)

- Sivusto kertoo miten Vgrant asennetaan

- Täällä kerrotaan myös miten sitä käytetään

- Erinomaisia troubleshooting juttuja myös

- Todella hyvin selitettty, mistä Vagrantissa on kyse





[Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux | Tero Karvinen](https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux)



- Täällä kerrotaan miten salt toikeastit toimii

- Hyviä esimerkkejä siiitä miten laajasti salttia voi käyttää.

- Tarpeelliset komennot löytää täältä



[Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux | Tero Karvinen](https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux)

- Salt Vagrant asnnusohjeet

- Jotkut opiskelijat ovat kokeilleet winkkarissakin tätä, ja siis tämähän toimii windowssissakin.

- Tällä sivustolla kerrotaan miten hallitaan useaa konetta samaa aikaa.

- Täältä löytyy myös jäätävä määrä hyödyllisiä ja pakollisikin komentoja jotta tämän saa toimimaan

### 0:30

## a)

Asensin Vagrantin komennolla

```
sudo apt-get install vagrant
```
Version sain komennolla

    vagrant --version

![Vagrant Version](https://github.com/Gerathh/palvelintenhallinta/blob/main/h21.png)

Ajoin komennon, joka mahdollistaa autocomplete komentoja.

    vagrant autocomplete install --bash --zsh

## b)

  Ajoin seuraavat komennot:

```
$ sudo mkdir twohost/; cd twohost/
$ sudo nano Vagrantfile
```
Copy pastesin [https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/](https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/) -osoitteesta löytyvän tekstin.

Kommentoin # aluksi toisen koneen

![Toinen kone kommentoitu](https://github.com/Gerathh/palvelintenhallinta/blob/main/h22.png)

Ajoin komennon

```
Vagrant up
```


Se valitti oikeuksien puutteesta joten:

```
cd ..
sudo chmod 777 /twohost/ #WRONG
cd twohost/
```

Komento jälleen:

```
vagrant up

```
Tuli seuraava error:

![Error](https://github.com/Gerathh/palvelintenhallinta/blob/main/h23.png)

Lisäsin Vagrantfile:n komennon

`config.vm.provider "virtualbox"`

jälleen vagrant up

Puuuttuu virtualbox. Joten asennnan sen. Repot puuttuu joten:

`echo "deb [arch=amd64] https://download.virtualbox.org/virtualbox/debian bullseye contrib" | sudo tee /etc/apt/sources.list.d/virtualbox.list
`

`wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
`

`sudo apt-get update`

`sudo apt-get install virtualbox-7.0`

Sitten taas 
* vagrant up   
Valitti väärästä IP -avaruudesta.

![errror](https://github.com/Gerathh/palvelintenhallinta/blob/main/h24.png)

Joten luettuani virheilmoituksessa olevaa URL osoitetta, tein ohjeiden mukaan ja lisäsin /etc/vbox/networks.conf tiedostoon
     * 10.0.0.0/8 192.168.0.0/16


Jälleen vagrant up

Jälleen error

![er](https://github.com/Gerathh/palvelintenhallinta/blob/main/h25.png)

Hyper-V on pois päältä.
BIOS/UEFI:ssa   AMD-V päällä
Editoitu rekisteriä että hyper-V ei varmasti ole päällä.   
Testattu myös 7800x3d prosessorilla. Sama ongelma. Olin päättänyt saada tämän toimimaan VirtualBoxilla (7.1.6 r167084 (Qt6.5.3)). Varmaan vagrant olisi toiminut muulla tavalla, kuten winkkarin päällä.   
En testannut koska vagrant tutttu ja en jaksanut enää tehdä sitä.


![enabloitu](https://github.com/Gerathh/palvelintenhallinta/blob/main/h26.png)
---
![vbox](https://github.com/Gerathh/palvelintenhallinta/blob/main/h27.png)


### Ongelma oli siis etten voinut asentaa virtualboxin päälle uusia koneita.

## Asennus windowssissa.

Asensin vagrantin. 

    vagrant --version
![Versio](https://github.com/Gerathh/palvelintenhallinta/blob/main/h28.png)

Ajoin komennon 


   `
vagrant init --minimal
`

Loi tiedoston Vagrantfile kansioon twohost jonka loin aiemmin.
Sitten komento:

    winget isntall tree

Muokkasin Vagranfileä seuraavasti:
Kommentoin linux käskyt 


    #-*- mode: ruby -*-
    #vi: set ft=ruby : 
    #Copyright 2019-2021 Tero Karvinen
    
    http://TeroKarvinen.com
    
    $tscript = <<TSCRIPT
    #set -o verbose
    #apt-get update
    #apt-get -y install tree
    echo "Done - set up test environment - https://terokarvinen.com/search/?q=vagrant"
    TSCRIPT
    
    Vagrant.configure("2") do |config|
    	config.vm.synced_folder ".", "/vagrant", disabled: true
    	config.vm.synced_folder "shared/", "/home/vagrant/shared", create: true
    	config.vm.provision "shell", inline: $tscript
    	config.vm.box = "debian/bullseye64"
    config.vm.define "t001" do |t001|
    		t001.vm.hostname = "t001"
    		t001.vm.network "private_network", ip: "192.168.88.101"
    	end
    #config.vm.define "t002", primary: true do |t002|
    	#	t002.vm.hostname = "t002"
    	#	t002.vm.network "private_network", ip: "192.168.88.102"
    	#end
    	end   
    	
Tämän jälkeen komento 

    vagrant up

![Success](https://github.com/Gerathh/palvelintenhallinta/blob/main/h29.png)

Ajoin komennon

    vagrant destroy
Ja painoin y ja enter jolloin kone tuhotaan   

---
## Kaksi uutta konetta

Sen jälkeen otin kommentit pois Vagrantfilen toisen koneen kohdalta. 

![Kommentit pois osasta](https://github.com/Gerathh/palvelintenhallinta/blob/main/h210.png)

Ja ajoin komennon 

    vagrant up

Asentui kaksi konetta.


Tehtävän mukaisesti pingasin toisia koneita sekä googlen nimiaalvelinta ja kaikki meni läpi.

![Yhteydet toimii](https://github.com/Gerathh/palvelintenhallinta/blob/main/h211.png)

## Mini ja master

Suoritin seuraavat komennot:

    sudo mkdir -p /etc/apt/keyrings
    sudo apt update
    sudo apt install curl
    curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp
    sudo apt install ufw
    sudo ufw allow 4505
    sudo ufw allow 4506
    sudo cat /etc/apt/sources.list.d/salt.list deb [signed-by=/etc/apt/keyrings/salt-archive-keyring.pgp arch=amd64] https://packages.broadcom.com/artifactory/saltproject-deb/ stable main
    sudo apt update
    sudo apt install salt-minion
    
![minion version](https://github.com/Gerathh/palvelintenhallinta/blob/main/h212.png)
    
Lisää ajettavia komentoja:

    sudo salt-call --local -l info state.single pkg.installed tree
    sudo salt-call --local -l info state.single pkg.removed tree
    sudo nano /etc/salt/minion

Lisäsin tiedostoon rivin:

    master: 192.168.88.102

    sudo systemctl restart salt-minion

Sitten 

    exit

---

Seuraavaksi master

    vagrant ssh t002
    sudo apt update
    sudo mkdir -p /etc/apt/keyrings
    sudo apt install curl
    curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp
    sudo apt install ufw
    sudo ufw allow 4505
    sudo ufw allow 4506
    sudo nano /etc/apt/sources.list.d/salt.list
    
Lisäsin sinne rivin:

    deb [signed-by=/etc/apt/keyrings/salt-archive-keyring.pgp arch=amd64] https://packages.broadcom.com/artifactory/saltproject-deb/ stable main

Sitten komennnot

    sudo apt update
    sudo apt install salt-master
    sudo systemctl restart salt-minion



##### Lähteet

Tehtävät [Palvelinten Hallinta - Configuration Management Systems course - 2025 spring](https://terokarvinen.com/palvelinten-hallinta/)

komentoja jotka olivat tarpeellisia tehtävän tekoon [Two Machine Virtual Network With Debian 11 Bullseye and Vagrant](https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/)

komentoja msalt master ja slavelle. [Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux | Tero Karvinen](https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux)

Komentoja saltin käyttöön [Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux | Tero Karvinen](https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux)

Vagrantin kaikki komennot ja toiminnot [Documentation | Vagrant | HashiCorp Developer](https://developer.hashicorp.com/vagrant/docs)

[Apuna windowssin puolella asetusten varmistukseen](https://dev.to/sannae/setting-up-windows-virtual-test-environments-with-vagrant-4k1b)

## Edit 9.4.2025 lisätty error warning.
## Edit 15.4.2025 Windowssin kautta asennus

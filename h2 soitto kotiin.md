### h2 soitto kotiin

---

**0:00**

## x)

[Two Machine Virtual Network With Debian 11 Bullseye and Vagrant](https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/)

- Sivusto kertoo miten Vgrant asennetaan

- TRäällä kerrotaan myös miten sitä käytetään

- Erinomaisia troubleshooting juttuja myös

- Todella hyvin selitettty, mistä Vagrantissa on kyse





[Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux | Tero Karvinen](https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux)



- Täällä kerrotaan miten salt toikeastit toimii

- Hyviä esimerkkejä siiitä miten laajasti salttia voi käyttää.

- tarpeelliset kommmpot löytää täältä



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
sudo chmod 777 /twohost/
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

sitten taas vagrant up. Valitti väärästä IP -avaruudesta.

![errror](https://github.com/Gerathh/palvelintenhallinta/blob/main/h24.png)

Joten luettuani virheilmoituksessa olevaa URL osoitetta, tein ohjeiden mukaan ja lisäsin /etc/vbox/networks.conf tiedostoon
     * 10.0.0.0/8 192.168.0.0/16


Jälleen vagrant up

Jälleen error

![er](https://github.com/Gerathh/palvelintenhallinta/blob/main/h25.png)

Hyper-V on pois päältä.
BIOS/UEFI:ssa   AMD-V päällä
Editoitu rekisteriä että hyper-V ei varmasti ole päällä.


![enabloitu](https://github.com/Gerathh/palvelintenhallinta/blob/main/h26.png)
---
![vbox](https://github.com/Gerathh/palvelintenhallinta/blob/main/h27.png)




##### Lähteet

Tehtävät [Palvelinten Hallinta - Configuration Management Systems course - 2025 spring](https://terokarvinen.com/palvelinten-hallinta/)

komentoja jotka olivat tarpeellisia tehtävän tekoon [Two Machine Virtual Network With Debian 11 Bullseye and Vagrant](https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/)

komentoja msalt master ja slavelle. [Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux | Tero Karvinen](https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux)

Komentoja saltin käyttöön [Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux | Tero Karvinen](https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux)

Vagrantin kaikki komennot ja toiminnot [Documentation | Vagrant | HashiCorp Developer](https://developer.hashicorp.com/vagrant/docs)



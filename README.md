# Zencash on TOR

### Linux

#### Connect to tor nodes only:
first do `sudo apt install tor`
then go to your zen.conf and add
```
proxy=127.0.0.1:9050
onlynet=tor
addnode=arrdjplw5rtjpyqm.onion
addnode=gowy6xruajuu2tln.onion
``` 

Restart zend and your node will officially be hidden and outta sight :D

#### Setting up zend to be reachable by other tor nodes:
Go to your `/etc/tor/torrc` and add
```
HiddenServiceDir /var/lib/tor/zen-service/
HiddenServicePort 9033 127.0.0.1:9033
```

Restart tor with `service tor restart` Then do `cat /var/lib/tor/zen-service/hostname` and keep track of the onion address it gives you. Go to your zen.conf and add 
```
bind=127.0.0.1
externalip=ONION_ADDR
``` 
where ONION_ADDR is your onion address.
Restart zend and then give out your onion address, nodes will then be able to connect to it :D

### Windows
Submit a PR for windows instructions for 5 zen :0

### List of TOR nodes:
```
addnode=arrdjplw5rtjpyqm.onion
addnode=gowy6xruajuu2tln.onion
```

Submit a PR to have yours added!

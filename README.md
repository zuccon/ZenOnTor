# Zencash on Tor

### Linux

#### Install Tor from the official repository
First add the Tor repository to your package sources
```
sudo su -c "echo 'deb http://deb.torproject.org/torproject.org '$(lsb_release -c | cut -f2)' main' > /etc/apt/sources.list.d/torproject.list"
gpg --keyserver keys.gnupg.net --recv A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89
gpg --export A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89 | sudo apt-key add -
```
and install it
```
sudo apt-get update
sudo apt-get install tor deb.torproject.org-keyring
```
then add your current user to the `debian-tor` group to be able to use cookie authentication.
```
sudo usermod -a -G debian-tor $(whoami)
```

Now log out and back in, then edit `/etc/tor/torrc` to enable the control port, enable cookie authentication and make the auth file group readable.
```
sudo sed -i 's/#ControlPort 9051/ControlPort 9051/g' /etc/tor/torrc
sudo sed -i 's/#CookieAuthentication 1/CookieAuthentication 1/g' /etc/tor/torrc
sudo su -c "echo 'CookieAuthFileGroupReadable 1' >> /etc/tor/torrc"
```
Now restart the Tor service.
```
sudo systemctl restart tor.service
```

#### Setting up zend to be reachable by other Tor nodes

If you've configured Tor as previously described there is nothing more you need to do. When you start zend without any command line options e.g. `./zend` it will automatically create a HiddenService and be reachable via the Tor network and clearnet. The node should have created a file `~/.zen/onion_private_key`, this file stores the private key of your HiddenService address.

When you use `./zen-cli getnetworkinfo` you should see that your node is reachable as a HiddenService like so:
```
[...]
    {
      "name": "onion",
      "limited": false,
      "reachable": true,
      "proxy": "127.0.0.1:9050",
      "proxy_randomize_credentials": true
    }
  ],
  "relayfee": 0.00000100,
  "localaddresses": [
    {
      "address": "lbfuusf2bg5e2t3j.onion",
      "port": 9033,
      "score": 4
    }
[...]
```

You can now give out your onion address, nodes will then be able to connect to it :D

#### Connect to Tor nodes only:
edit your zen.conf and add
```
proxy=127.0.0.1:9050
onlynet=tor
addnode=arrdjplw5rtjpyqm.onion
addnode=gowy6xruajuu2tln.onion
addnode=ot35wzoyrnaurgz6.onion
addnode=etqmwbdd4u6kyxlq.onion
``` 

Restart zend and your node will officially be hidden and outta sight :D

### Windows
Submit a PR for windows instructions for 5 zen :0

### List of TOR nodes:
```
addnode=arrdjplw5rtjpyqm.onion
addnode=gowy6xruajuu2tln.onion
addnode=ot35wzoyrnaurgz6.onion
addnode=etqmwbdd4u6kyxlq.onion
addnode=d2y2vsq5rxkcpk6f.onion
```

Submit a PR to have yours added!

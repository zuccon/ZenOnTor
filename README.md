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
sudo su -c "echo 'LongLivedPorts 9033' >> /etc/tor/torrc"
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
addnode=o5wxdwgg6g2fd2y5.onion
addnode=etqmwbdd4u6kyxlq.onion
addnode=d2y2vsq5rxkcpk6f.onion
addnode=nd4tfjxr6youzbbd.onion
addnode=5xxjoc3t5h6zaj2p.onion
addnode=eorrku3hauy53zup.onion
addnode=mbkpyfwjudi423xp.onion
addnode=ayeypndttmb54eeu.onion
addnode=bdfdc3ch5feattda.onion
addnode=g5k4nu5wuyhxhjht.onion
addnode=e3uspj25k24hsxh2.onion
addnode=nvebpobdrsobn7oq.onion
``` 

Restart zend and your node will officially be hidden and outta sight :D

### Windows

#### Install and configure Tor

Download the Tor expert bundle for Windows from `https://www.torproject.org/download/download`and extract the zip file to a subfolder.

Now create a new folder at `%APPDATA%\tor` and create a new text file `%APPDATA%\tor\torrc` and put this in it (note that the file needs to be saved without a `.txt` extension):
```
ControlPort 9051
CookieAuthentication 1
LongLivedPorts 9033
```

Now open a command prompt and change to the directory you extracted Tor to e.g.
```
cd c:\Downloads\tor-win32-0.3.0.10
```
and start tor.exe with
```
Tor\tor.exe
```
you should see Tor's log output in the command prompt window.

#### Setting up zend to be reachable by other Tor nodes

Same as the Linux guide but use `.\zend.exe`, `.\zen-cli.exe getnetworkinfo` and `%APPDATA%\Zen\onion_private_key`.

#### Connect to Tor nodes only:

Same as the Linux guide but edit the config at `%APPDATA%\Zen\zen.conf`.

### List of Tor nodes:
```
addnode=arrdjplw5rtjpyqm.onion
addnode=gowy6xruajuu2tln.onion
addnode=ot35wzoyrnaurgz6.onion
addnode=o5wxdwgg6g2fd2y5.onion
addnode=etqmwbdd4u6kyxlq.onion
addnode=d2y2vsq5rxkcpk6f.onion
addnode=nd4tfjxr6youzbbd.onion
addnode=5xxjoc3t5h6zaj2p.onion
addnode=eorrku3hauy53zup.onion
addnode=mbkpyfwjudi423xp.onion
addnode=ayeypndttmb54eeu.onion
addnode=bdfdc3ch5feattda.onion
addnode=g5k4nu5wuyhxhjht.onion
addnode=e3uspj25k24hsxh2.onion
addnode=nvebpobdrsobn7oq.onion
```

Submit a PR to have yours added!

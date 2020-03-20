# Atelier1

######################################

CAPTURE 1

1/ Veuillez identifier votre serveur GNS3 qui est capgchambX.goffinet.net où X est le numéro de votre serveur. 
capgchamb9.goffinet.net
Pouvez-vous interpréter de la manière la plus précise la commande suivante adaptée de la valeur X de votre serveur ? Quel résultat fournit-elle ? 

curl http://capgchamb9.goffinet.net:8003

Voici la réponse de la console :

{
  name: "capgchamb9.goffinet.net"
  ipv4: "51.15.243.43"
}

On remarque que le fichier est de type JSON, avec des champs et un contenu associé. « capgchamb9.goffinet.net » est l’hôte de destination et l’ip 51.15.243.43 est l’adresse IP du client envoyeur de cette demande. C’est une demande de type HTTP de la couche application, on peut donc conclure que l’émetteur de cette demande va tenter d’établir une connexion TCP vers l’hôte « capgchamb9.goffinet.net » afin de s’y connecter sur le port 8003.

2/ Fichier de capture enregistré.

3/ Le port est 8003.

4/No4 : ici le protocol HTTP commence (couche 4 application du modèle TCP/IP). La machine ubuntu2 (192.168.1.3) effectue une requête HTTP avec la méthode « GET » sur l’url qui est décrite ici :
- http:// : on utilise le protocole HTTP
- Host: capgchamb9.goffinet.net:8003 : on se connecte à cette hôte sur un port spécifique.
Le destinataire répond à cette demande par le code « 200 » qui signifie OK.

5/ 
On observe que la commande envoie une requête TCP afin de mettre en place un procotole de la couche transport (couche 3 modèle TCP/IP).
En paquet No.1, le terminal client source 192.168.1.3 entame une procédure « three way match » afin d’établir une connexion avec la machine 51.15.243.43, avec un premier paquet SYN (synchronize).
En No.2, le terminal de destination répond à la SYN au bout de 0,001933 sec avec un paquet ACK (acknowledge),.afin de faire une demande de connexion à son tour, il envoie également un paquet SYN.
En No.3, la connexion est bien établit avec le paquet ACK à 1 pour les deux machines.

Le protocole de fin de session correspond à une procédure du protocol TCP :
No.10, la machine émettrice de cette connexion TCP émet une demande de fin de connexion avec le paquet FIN, ACK de valeur seq=93 ack= 298. 
No.11 : La machine receveur de ce paquet répond également par FIN, ACK de valeur ack=94 (seq de l’envoyeur+1) afin d’attester de la fin de connexion et par un paquet seq = 298.
No 12 : la machine client reçoit la demande de fin de connexion à son tour et répond par ack=299, soit le paquet seq+1.
La connexion prend définitivement fin dans ce paquet No12


######################################

CAPTURE 2

1/ root@pc2:~# dig @ns1.google.com -t TXT o-o.myaddr.l.google.com +short -4

dig : La commande dit est un outils qui permet de diagnostiquer les dysfonctionnements du serveur DNS.

@ns1.google.co : il s’agit du serveur DNS (domain name service) à observer. le @  annonce le DNS dans la commande.

TXT : cette option indique que le retour doit être un retour de chaîne de caractères.

+short : option qui permet de n’obtenir que l’ipv4 de l’enregistrement.

Le résultat fournit est le suivant : "51.15.243.43 », c’est une chaîne de caractères (type String) qui indique un ipv4. C’est l’adresse iP du serveur DNS et non pas du site correspondant à ce nom domaine.

2/ fichier enregistré.

3/ Voici les protocoles:
- protocol DNS
- protocol ARP
- 4 paquets de protocole CDP : lié à l’option -4 dans la ligne de commande
- protocol ICMPv6

 Il y a donc une transaction DNS, une transaction ARP ainsi que 4 ping lié au protocole ICMPv6.

Il y a plusieurs paquets car plusieurs machines sont ici sollicitées.

4/ 

j’observe un protocole DNS en premier, car avant d’adresser des paquets à la machine de destination, l’envoyeur doit connaître l’adresse de la couche réseau, donc l’adresse, afin de faire parvenir sa requête. C’est la standard query « 0xebce ».

Paquet No.3 : Pour le protocole ARP, le routeur interroge sa table de routage afin de connaître la prochaine destination, il utilise l’adresse de diffusion pour interroger tous les éléments de son réseau. L’appareil qui reconnaît son adresse MAC répond, sinon il ne fait rien . En paquet No.4 la réponse à « Who has 192.168.1.254 ?» est une adresse mac physique : « 0c:19:8a:7e:fe:00 »

Le protocole CPD est un protocole de niveau 2 qui permet de connaître les périphériques voisins.

5/
No.1 : 192.168.1.3 est l’ordinateur client ubuntu qui fait la demande à destination de l’ip 216.239.32.10 qui est le commutateur qui relie ubuntu au routeur

No.2 : le commutateur atteste de la bonne réception de la standard query DNS

No3. 0c:19:8a:4f:4e:00 est l’adresse mac source (couche 2 Internet), celle du ubuntu PC2. C’est à destination de l’adresse mac 0c:19:8a:7e:fe:00, le routeur.

No.4 : le routeur atteste réception au PC ubuntu 

No.5 : protocole CDP est en destination depuis le routeur pour connaître la localisation du serveur DNS sur le WAN. 

No.6 @ 10 : une fois l’adresse ipv4 à destination de ns1.google.com connue, le réseau local fe80::1 effectue un PING de cet IP. Finalement si l’IP est bien vérifié, elle est renvoyé à la console en résultat de commande.

Note : on remarque une erreur au paquet 8, mais un deuxième envoi des 4 paquets de No.12 @ No.15 confirme l’adresse.



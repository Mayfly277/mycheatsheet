Linux
=====

SSH
----
- Port forwarding : option -L  
- exemple : `ssh -L 3307:localhost:3306 monserver`
- redirige le port 3306 de la localhost sur la machine distante vers le port 3307 de la machine appellante
- si plusieurs, plusieurs -L

ssh -L 3307:mydb1:3306 -L 8984:localhost:8983 hostname

- Supprimer un host de la liste des know host :
`ssh-keygen -R "hostname"`

XDEBUG en CLI
-------------
export PHP_IDE_CONFIG=serverName=myhostname.local
REMOTE_HOST=`echo $SSH_CLIENT | cut -d ' ' -f 1`
export XDEBUG_CONFIG=remote_host=$REMOTE_HOST idekey=PHPSTORM remote_enable=1 remote_connect_back=1


Debian squeeze source.list
--------------------------
```
deb http://archive.debian.org/debian squeeze main non-free contrib
deb http://security.debian.org/ squeeze/updates main contrib non-free
```

Service enable/disable
---------------------
- sudo update-rc.d myservice defaults
- sudo update-rc.d -f myservice remove
- sudo systemctl enable myservice.service
- sudo systemctl disable myservice.service

VARNISH
-------
```
varnishlog -g request -q "ObjHeader ~ 'text/html'"
varnishlog -g request -q "ObjHeader ~ 'text/html' and ReqHeader ~ '127.0.0.1'"
varnishlog -g request -q "ReqUrl ~ '/?titi' and RespStatus ~ 302"
varnishlog -g request -q "ReqHeader ~ 127.0.0.1 and ReqHeader ~ localhost.com"
```

sed
---
- transformer les espaces multiples en espace simple pour utiliser cut

```
$echo "ma    ligne   avec plusieurs     espaces"| sed 's/  */ /g'
ma ligne avec plusieurs espaces
```

- supprimer le dernier caractère : `sed 's/.$//g'`

cut
---
- `cut -d ' ' -f 3`
- -d delimiteur
- -f le numéro du champ délimité que l'on veut

nombre de ligne dans des fichiers heures par heures
------

```
year="2016";month="11";day="02"; for i in {00..23}; do find . -newermt "$year-$month-$day $i:00:00" ! -newermt "$year-$month-$day $i:59:59" -type f | xargs cat | wc -l | xargs echo "$i:00:00-$i:59:59 :"; done
```
regroupe les fichiers heures par heures, les concatènes, compte les lignes et renvoie le résultat

Disque usage
------------
- `ncdu`

CURL
----
- `curl -I -H 'Host:siteurl.local' http://127.0.0.1/`
- -I : demande les informations d'entete seulement (affiche l'entete de response)
- -H : Précise des informations dans le header
- autentification : -u username:password
- -o /dev/null : pas de sortie
- -sv pour afficher les headers
- `curl -o /dev/null -sv <url>` : permet d'afficher les headers en faisant une requète en GET

WGET
----
- `usage : `wget <mon url>`
- sans cache : --no-cache
- non verbose : -nv
- préciser la sortie
- ajouter l'autentification : http://monuser:monpassword@monurl/

Find
----
- trouver une liste d'extension de fichiers:
```
find . -regextype grep -regex "\./.*\.\(php\|pl\|py\|jsp\|asp\|htm\|shtml\|sh\|cgi\|php3\|phtml\|phps\)"
```

- liste de crons magento :
```
find . -name 'config.xml' -exec grep -H 'cron_expr' {} \;
```

Watch
-----
- suivi d'une commande : ` watch -n 1 -x`
- -n 1 pour toutes les secondes
- -x la commande à lancer
- ex: commande mysql show processlist toutes les secondes : `watch -n 1 -x mysql -u root -e 'show processlist;'`

Screen
------
- Lancement : `screen`
- Détacher le screen : ctrl + a puis d
- revenir dans le screen : `screen -r`

Oh my Zsh
---------
- Alt + gauche / Alt +droite : navigation
- `j recherche` : smart completion jump
- h : history
- j : jobs
- p : process


Mail
----
- `mail -s "mon sujet"` -A mon_fichierattaché mon@destinataire

SOLR
----
Delete core :
 - `curl http://localhost:8983/solr/core1/update?commit=true -d '<delete><query>*:*</query></delete>'`

SVN
---
- Merge :
  - Se placer dans le répertoire cible
  - Voir les révisions éligibles : `svn mergeinfo svn://urlsource --show-revs eligible`
  - Faire un merge à blanc : `svn merge --dry-run svn://urlsource`
  - Faire le merge officiel : `svn merge svn://urlsource`

- Pour faire par révision :
  - Ajouter l'option -c suivie par la liste des révisions voulues séparées par des virgules

SSH
---
- Régénération sans passphrase : `ssh-keygen -f .ssh/id_rsa -p`

DNS
---
- Trouver ip d'un nom de domaine :
	+ `dig mydomain.com A`
- Trouver les Name servers (NS) d'un nom de domaine :
	+ `dig mydomain.com NS`
- Trouver nom de domaine d'une ip (reverse lookup):
	+ `dig -x IP`
-Version courte : ajouter `+short`

- Utiliser également le fichier /etc/hosts systeme :
	+ `getent hosts domain`

- Juste voir l'ip :
	+ hosts domain

Linux desktop raccourcis
------------------------
- ~/.local/share/applications
- ~/.gnome/apps/
- /usr/local/applications

# TMUX 

- search ctrl+b s
- next : n
- previous : shift + n 

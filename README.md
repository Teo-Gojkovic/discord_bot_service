# Créer un service pour un bot discord en Java-Script

![Last update](https://img.shields.io/badge/Ubuntu_Server-22.04-orange.svg)
![Last update](https://img.shields.io/badge/Node_JS-v22.14.0-green.svg)


![Last update](https://img.shields.io/badge/Last_update-17/02/2025-red.svg)



## Préaparation des dossiers

Dans chaque dossier il faudra bien placé les differents programmes qui servent à votre bot.

```bash
mkdir -p /opt/bots/bot1 && mkdir -p /opt/bots/bot2
```

>*L'option `-p` signifie "parents" et permet de créer l'intégralité du chemin de répertoires spécifié, même s'ils n'existent pas encore.*

## Création d'un user

```bash
useradd -r -s /bin/false botuser
```

```bash
chown -R botuser:botuser /opt/bots
```

## Téléchargement de node

Misa a jour du système :

```bash
sudo apt update -y && sudo apt upgrade -y
```

Téléchargement de `node.js 22` :

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | bash -
```

```bash
apt install -y nodejs
```

Véfifier la vesion :

```bash
node -v
```

Sortie : 
>```bash
>v22.14.0
>```

## Installer les dépendances

Lorsque vous avez mis tout les fichiers qui étais nécessaire pour votre bot on va installer les dépendances : 

```bash
cd /opt/bots/bot1 && npm install
```

```bash
cd /opt/bots/bot2 && npm install
```

## Créer un service Systemd pour chaque bot

Crée un fichier service pour chaque bot. Exemple pour ``bot1`` :

```bash
sudo nano /etc/systemd/system/bot1.service
```
 
Copiez ceci :

```bash
[Unit]
Description=Bot Discord 1
After=network.target

[Service]
WorkingDirectory=/opt/bots/bot1
ExecStart=/usr/bin/node index.js
Restart=always
User=botuser
Environment=NODE_ENV=production
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=bot1

[Install]
WantedBy=multi-user.target
```

>Pour quitter : `ctrl + X`  

Faire de meme pour le bot2, bot3 ...


## Activer et démarrer les services

Actualiser Systemd pour prendre en compte les nouveaux fichiers :

```bash
systemctl daemon-reload
```

Pour chaque bot il faudra faire les 2 commandes suivante : 
```bash
systemctl enable bot1
```
```bash
systemctl start bot1
```

Voila votre bot est en ligne !

## Logs et gestion des bots

Voir les logs d'un bot en temps réel :

```bash
journalctl -u bot1 -f
```

Redémarrer un bot :

```bash
systemctl restart bot1
```

Arrêter un bot :

```bash
systemctl stop bot1
```

Démarrer un bot manuellement :

```bash
systemctl start bot1
```
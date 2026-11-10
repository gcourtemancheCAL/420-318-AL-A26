# Gestion des services 

## systemctl

`systemctl` est l'outil principal pour gérer les services sur les systèmes Linux basés sur `systemd`.
Il permet notamment de:

- démarrer/arrêter un service;
- activer/désactiver le démarrage automatique;
- vérifier l'état d'un service;
- lister les unités disponibles.

Commandes de base:
	- status
	- start / stop / restart
	- enable / disable

**Exemple :** 

```bash
systemctl status sshd
```

### Identifier les services installées

```bash
systemctl list-units --type=service
```

<img src="img/Pasted image 20261020123317.png" width="800" />

Services réseau courants à valider durant un audit:

- `ssh` / `sshd`
- `apache2` ou `httpd`
- `nginx` 
- `mariadb` ou `mysql`
- `postgresql`
- `vsftpd` / `proftpd` 
- `named` (DNS/BIND)
- ...

Points de vérification rapides:

- le service est-il nécessaire?
- est-il actif alors qu'il ne devrait pas l'être?
- la version est-elle à jour et maintenue?

## journalctl

`journalctl` sert à consulter les journaux gérés par `systemd-journald`.
Il centralise les logs de plusieurs composants (kernel, services, applications).

Peut être invoqué avec `sudo`. Les membres du groupe `systemd-journal` y ont aussi accès.

```bash
journalctl -u apache2

# peut être combiné à grep
journalctl -u ssh | egrep -i "failed|invalid|authentication failure"
```

### Autres sources de logs

Bien que le standard soit d'utiliser systemd pour la gestion des journaux, certains services vont conserver leurs journaux dans des fichiers spécifiques.

Le répertoire /var/log est un bon point de départ à investiguer.

Par exemple, le service apache2 va conserver les logs d'accès et d'erreurs dans /var/log/apache2.
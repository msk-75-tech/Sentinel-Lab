# Phase 1 — Déploiement du serveur Wazuh

## Objectif

L’objectif de cette phase est de déployer un serveur Wazuh afin de centraliser, corréler et analyser les événements de sécurité provenant des différentes machines du lab.

Ce serveur constituera le cœur du SOC dans le projet Sentinel-Lab.

Il permettra notamment de :

* collecter les journaux système et de sécurité
* superviser les postes et serveurs
* détecter les comportements suspects
* centraliser les alertes de sécurité

Cette première phase pose les fondations de l’ensemble du laboratoire.

---

## Préparation de l’environnement

Afin d’optimiser les performances du laboratoire, les machines virtuelles sont hébergées sur un SSD dédié installé spécifiquement pour ce projet.

Cette approche présente plusieurs avantages :

* amélioration des performances d’entrée/sortie
* réduction des temps de démarrage
* meilleure réactivité des machines virtuelles
* isolation du lab par rapport au système principal

Le dossier de stockage des machines virtuelles a été configuré sur ce disque dédié.

Chemin :

`D:\VMware\Sentinel-Lab`

Cette organisation facilite également la gestion des snapshots, des sauvegardes et de l’évolution future du laboratoire.


---

## Création de la machine virtuelle

La première machine virtuelle du laboratoire est le serveur Wazuh, qui assurera les fonctions de SIEM.

### Caractéristiques de la machine

* **Nom** : `Wazuh-Server`
* **Hyperviseur** : VMware Workstation
* **Système d’exploitation** : Ubuntu Server 22.04 LTS
* **Mémoire vive (RAM)** : 4 Go
* **Processeurs virtuels** : 2 vCPU
* **Disque virtuel** : 80 Go
* **Type de provisionnement** : allocation dynamique

### Justification des ressources

Cette configuration offre un bon équilibre entre performances et consommation de ressources pour un environnement de laboratoire.

* 4 Go de RAM permettent d’exécuter Wazuh dans de bonnes conditions
* 2 vCPU assurent une réactivité suffisante
* 80 Go offrent l’espace nécessaire pour :

  * le système d’exploitation
  * les composants Wazuh
  * les index
  * les journaux
  * les futurs événements de sécurité

Cette machine constitue l’élément central de l’architecture Sentinel-Lab.



---

## Configuration réseau

Le serveur Wazuh dispose de deux interfaces réseau virtuelles afin de séparer l’accès Internet du réseau interne du laboratoire.

### Interface 1 : NAT

* Fournit un accès à Internet
* Permet :

  * l’installation des paquets
  * les mises à jour du système
  * le téléchargement des composants Wazuh

### Interface 2 : Host-Only

* Reliée au réseau interne du laboratoire
* Permet la communication entre :

  * le serveur Wazuh
  * la machine Windows 10
  * la machine Kali Linux

### Plan d’adressage interne

| Machine      | Adresse IP    |
| ------------ | ------------- |
| Wazuh Server | 192.168.56.10 |
| Windows 10   | 192.168.56.20 |
| Kali Linux   | 192.168.56.30 |

Cette architecture permet :

* une isolation complète du réseau de test
* un accès Internet contrôlé
* une communication sécurisée entre les machines du lab



---

## Installation du système d’exploitation

Le serveur Wazuh repose sur **Ubuntu Server 24.04 LTS**, une distribution stable, performante et largement utilisée en environnement professionnel.

### Paramètres d’installation

* Version : Ubuntu Server 24.04 LTS
* Type d’installation : minimale
* Interface graphique : non installée
* Serveur OpenSSH : activé

### Pourquoi une installation minimale ?

Ce choix permet de :

* réduire la consommation de ressources
* diminuer la surface d’attaque
* limiter les services inutiles
* améliorer les performances globales

### Configuration initiale

Lors de l’installation, les éléments suivants sont définis :

* nom d’hôte : `wazuh-server`
* création d’un compte administrateur
* configuration du fuseau horaire
* activation d’OpenSSH pour l’administration à distance

Cette base système servira de fondation au déploiement de la plateforme Wazuh.



---

## Configuration de l’adresse IP statique

Afin de garantir une communication stable entre les machines du laboratoire, le serveur Wazuh utilise une adresse IP statique sur le réseau interne.

### Adresse attribuée

* Interface interne : `ens37`
* Adresse IP : `192.168.56.10/24`

### Configuration Netplan

Fichier de configuration :

`/etc/netplan/00-installer-config.yaml`

Exemple de configuration :

```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: true
    ens37:
      dhcp4: no
      addresses:
        - 192.168.56.10/24
```

### Application de la configuration

```bash
sudo netplan apply
```

### Vérification

```bash
ip a
```

Cette configuration permet au serveur Wazuh d’être accessible en permanence par les autres machines du laboratoire.



---

## Optimisation du stockage des journaux

Un serveur SIEM génère une quantité importante de journaux. Il est donc nécessaire de limiter la taille des logs système pour éviter une saturation du stockage.

### Fichier concerné

`/etc/systemd/journald.conf`

### Paramètre à modifier

```ini id="fkfe7t"
SystemMaxUse=500M
```

### Application des changements

```bash id="4w5w4m"
sudo systemctl restart systemd-journald
```

### Bénéfices

* maîtrise de l’espace disque utilisé
* prévention de la saturation du stockage
* meilleure stabilité à long terme
* conservation d’un historique pertinent

Cette optimisation est particulièrement importante dans un environnement de laboratoire où le volume de logs peut rapidement augmenter.

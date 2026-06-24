# Phase 2 — Déploiement et supervision d'un poste Windows 10

## Objectif

Cette phase a pour objectif d'intégrer un poste Windows 10 au serveur Wazuh afin de commencer la supervision des terminaux du laboratoire.

Contrairement à la première phase, qui consistait à mettre en place l'infrastructure du SIEM, cette étape permet d'ajouter le premier équipement supervisé.

Le poste Windows deviendra la machine victime utilisée lors des différents scénarios d'attaque réalisés depuis Kali Linux.

À l'issue de cette phase, le serveur Wazuh sera capable de :

* détecter la présence du poste Windows ;
* recevoir les journaux Windows ;
* superviser les processus en cours d'exécution ;
* détecter les connexions réseau ;
* surveiller les modifications du système ;
* recevoir les événements générés par Sysmon.

Cette étape constitue le point de départ des futures détections de menaces.

---

# Architecture

À partir de cette phase, l'architecture du laboratoire devient la suivante :

Internet
│
└── VMware Workstation
│
├── Ubuntu Server
│      ├── Wazuh Manager
│      ├── Wazuh Indexer
│      └── Wazuh Dashboard
│
├── Windows 10
│      ├── Agent Wazuh
│      └── Sysmon
│
└── Kali Linux
└── Machine d'attaque

---

# Préparation de la machine virtuelle

Une nouvelle machine virtuelle est créée sous VMware Workstation.

Configuration retenue :

| Paramètre    | Valeur                           |
| ------------ | -------------------------------- |
| Nom          | Windows-10-Lab                   |
| Système      | Windows 10 Professionnel         |
| Mémoire vive | 4 Go                             |
| Processeurs  | 2 vCPU                           |
| Disque       | 60 Go                            |
| Firmware     | UEFI                             |

Cette configuration offre suffisamment de ressources pour exécuter Windows, Sysmon ainsi que l'agent Wazuh sans dégrader les performances du laboratoire.

---

# Configuration réseau

Comme le serveur Wazuh, deux cartes réseau sont utilisées.

## Carte NAT

Cette interface permet :

* l'accès Internet ;
* les mises à jour Windows ;
* le téléchargement des logiciels.

## Carte Host-Only

Cette interface est réservée au réseau interne du laboratoire.

Adresse IP :

192.168.56.20

Masque :

255.255.255.0

Passerelle :

Aucune

DNS :

Aucun

---

# Vérification de la communication

Une fois la configuration réseau terminée, la communication entre le poste Windows et le serveur Wazuh est vérifiée.

Depuis Windows :

```cmd
ping 192.168.56.10
```

Depuis Ubuntu :

```bash
ping 192.168.56.20
```

Les deux machines doivent pouvoir communiquer avant toute installation de l'agent.

---

# Installation de l'agent Wazuh

L'agent Windows sera téléchargé depuis le serveur Wazuh.

Après installation, il sera enregistré auprès du serveur afin que celui-ci puisse recevoir les journaux de sécurité du poste.

Cette étape permettra de faire apparaître la machine Windows dans le tableau de bord Wazuh.

---

# Installation de Sysmon

Afin d'obtenir une visibilité avancée sur les activités du poste Windows, Sysmon sera installé.

Contrairement aux journaux Windows classiques, Sysmon permet notamment de surveiller :

* les créations de processus ;
* les connexions réseau ;
* les chargements de pilotes ;
* les créations de services ;
* les modifications du registre ;
* les créations de fichiers.

Toutes ces informations seront ensuite envoyées vers Wazuh grâce à l'agent installé précédemment.

---

# Validation

La phase sera considérée comme terminée lorsque :

* Windows apparaîtra dans la liste des agents Wazuh ;
* l'état de l'agent sera « Active » ;
* des événements Windows seront visibles dans le Dashboard ;
* les événements Sysmon commenceront à être collectés.

---

# Captures prévues

Les captures suivantes seront réalisées :

* Configuration VMware de la machine Windows
* Installation de Windows
* Configuration IP
* Test de communication avec Wazuh
* Installation de l'agent Wazuh
* Enregistrement de l'agent
* Apparition de l'agent dans Wazuh
* Installation de Sysmon
* Premiers événements Sysmon dans Wazuh

---

# Conclusion

À l'issue de cette phase, le laboratoire disposera de son premier poste supervisé.

Cette machine constituera la cible des différents scénarios d'attaque réalisés lors des phases suivantes, permettant ainsi de valider les capacités de détection offertes par Wazuh et Sysmon.

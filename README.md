# Sentinel-Lab — Homelab SOC de détection de menaces

## Présentation

Sentinel-Lab est un laboratoire de cybersécurité conçu pour simuler des attaques informatiques et analyser leur détection dans un environnement de type SOC (Security Operations Center).

L’objectif est de reproduire un environnement réaliste afin de comprendre comment les événements de sécurité sont collectés, corrélés et analysés.

---

## Objectifs

* Mettre en place un SIEM pour centraliser les logs
* Superviser un poste Windows avec une visibilité avancée
* Simuler des attaques contrôlées
* Analyser les alertes générées

---

## Architecture

Le lab repose sur trois machines virtuelles :

* Un serveur Wazuh (SIEM)
* Une machine Windows 10 (poste cible)
* Une machine Kali Linux (poste attaquant)

---

## Réseau

Deux types de réseaux sont utilisés dans VMware :

* NAT : accès à Internet (installation, mises à jour)
* Réseau interne (Host-Only) : communication isolée entre les machines

Plan d’adressage :

| Machine      | Adresse IP    |
| ------------ | ------------- |
| Wazuh Server | 192.168.56.10 |
| Windows 10   | 192.168.56.20 |
| Kali Linux   | 192.168.56.30 |

---

## Infrastructure

Les machines virtuelles sont hébergées sur un SSD dédié afin d’améliorer les performances et d’isoler le lab du système principal.

Répartition du stockage :

* Wazuh Server : 80 Go
* Windows 10 : 60 Go
* Kali Linux : 40 Go

Un espace supplémentaire est conservé pour les snapshots et les logs.

---

## Technologies utilisées

* Wazuh (SIEM)
* Sysmon
* Ubuntu Server 24.04
* Windows 10
* Kali Linux
* VMware

---

## Phases du projet

1. Déploiement du serveur Wazuh
2. Intégration d’un poste Windows avec Sysmon
3. Simulation d’attaques
4. Détection et analyse des événements

---

## Méthodologie

Chaque phase du projet comprend :

* une documentation technique
* des captures d’écran
* une validation du fonctionnement

---

## Auteur

Moussa Sakho
Étudiant en cybersécurité

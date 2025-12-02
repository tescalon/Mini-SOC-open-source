# 🛡️ Déploiement d'un SOC Open Source Pédagogique (LAB CYBER)

[![Statut du Projet](https://img.shields.io/badge/Statut-En%20Cours-orange)](./documentation/objectifs.md)
[![Technologies Principales](https://img.shields.io/badge/Tech-SIEM%20(ELK)%2C%20MISP%20%2C%20SOAR%20(TheHive%2FCortex)-blue)](./documentation/architecture.md)
[![Focus Technique](https://img.shields.io/badge/Focus-Cybers%C3%A9curit%C3%A9%20Avanc%C3%A9-red)](./documentation/rapport_technique.md)


## 🎯 Executive Summary

Ce projet vise à concevoir et opérer une chaîne de sécurité défensive (Blue Team) complète dans un environnement contraint. L'objectif est de simuler un **SOC d'entreprise** capable de traiter un incident de bout en bout : de la détection d'une anomalie réseau à l'enrichissement via Threat Intelligence.

L'infrastructure repose sur une architecture **distribuée et conteneurisée**, répondant à des exigences strictes de gestion de ressources (Capacity Planning sur 2 nœuds physiques de 16Go RAM).

---

## 🔄 Operational Workflow (Flux de Données)

Le laboratoire est conçu pour orchestrer le cycle de vie complet d'une alerte de sécurité.

![Architecture Schema](./assets/architecture-v2.png)

### 1. Phase de Menace (Zone Rouge)
* **Vecteur :** Simulation d'attaques automatisées (Hydra, Nmap) via des conteneurs "Red Team".
* **Cible :** Services vulnérables exposés volontairement dans une zone isolée (DMZ Docker).

### 2. Phase de Détection (Zone Bleue - Nœud B)
* **Collecte :** L'agent Wazuh remonte les logs systèmes et d'authentification en temps réel.
* **Corrélation :** Le SIEM analyse les patterns (Règles XML) et génère une alerte de sécurité qualifiée.

### 3. Phase de Réponse (Zone Intelligence - Nœud A)
* **Escalade :** L'alerte est transmise via API au SOAR (TheHive).
* **Enrichissement :** Interrogation automatique de MISP pour vérifier la réputation des IOCs (IP, Hash).
* **Décision :** Prise en charge par l'analyste pour remédiation.

---

## 🏗️ Infrastructure Design (Hardware & Stack)

Pour pallier les limitations matérielles, les services sont répartis selon leur profil de consommation (CPU-bound vs I/O-bound).

| Nœud Physique | Rôle GRC | Stack Technologique | Justification |
| :--- | :--- | :--- | :--- |
| **PC A (Intel i7)** | **Intelligence Node** | `TheHive 5`, `Cortex`, `MISP`, `Elastic` | Hôte dédié aux traitements analytiques lourds (Java Heap intensive). |
| **PC B (Intel i5)** | **Detection Front** | `Wazuh`, `OPNsense`, `Kali` | Hôte dédié à l'ingestion de flux et au routage réseau. |

---

## 📸 Evidence & Reporting

Les captures ci-dessous illustrent le traitement d'un scénario "Brute Force SSH".

| SIEM Dashboard (Wazuh) | Incident Management (TheHive) |
| :---: | :---: |
| ![Wazuh](./assets/dashboard-wazuh.png) | ![TheHive](./assets/alert-thehive.png) |
| *Visualisation des pics d'attaques* | *Ticket généré automatiquement* |

---

## 🚀 Getting Started

L'installation est automatisée via Docker Compose, mais nécessite une configuration réseau préalable.

### Pré-requis
* 2x Hôtes Linux (Ubuntu Server 22.04 LTS recommandé)
* IP Statiques configurées : `192.168.1.50` (Node A) et `192.168.1.51` (Node B)
* Tuning Sysctl : `vm.max_map_count=262144`

### Installation Rapide

**1. Déploiement du Nœud Frontal (PC B)**
```bash
git clone https://github.com/TON-USER/LAB-SOC-DIST-01.git
cd LAB-SOC-DIST-01/node-detection-i5
docker compose up -d
```

**2. Déploiement du Nœud Intelligence (PC A)**
```bash
cd ../node-intelligence-i7
docker compose up -d
```

*(Consulter le dossier `/docs` pour le guide d'intégration API complet)*

---

## 💡 Compétences Validées
* **Architecture :** Conception distribuée et segmentation réseau.
* **SecOps :** Maîtrise de la chaîne Wazuh / TheHive / MISP.
* **Ingénierie :** Optimisation Docker et gestion des ressources systèmes.

---

**Le projet est actuellement en cours de réalisation. La documentation détaillée de l'architecture, de la configuration et des analyses de sécurité sera mise à jour et publiée dans les prochains jours / semaines / mois.**

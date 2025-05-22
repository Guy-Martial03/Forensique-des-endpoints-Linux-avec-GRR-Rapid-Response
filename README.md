
# Forensique des Points de Terminaison Linux avec GRR Rapid Response

## Introduction

GRR Rapid Response est un cadre de réponse aux incidents axé sur la forensique à distance en temps réel. Il permet aux professionnels de la sécurité de réaliser des investigations approfondies sur plusieurs points de terminaison à partir d'un serveur central. Ce laboratoire de niveau avancé vous guidera à travers l'installation et l'utilisation de GRR Rapid Response pour effectuer des analyses forensiques sur des systèmes Linux, couvrant l'installation, la configuration, la collecte de données à distance et l'analyse.

## Prérequis


- Connaissance avancée des systèmes Linux et de l'interface en ligne de commande
- Compréhension des principes et des techniques forensiques
- Connaissance de base des concepts de sécurité réseau et système
- Connaissances de base en scripting et expressions régulières

### Installation de GRR Rapid Response

1. Installer les dependances:
    ```bash
    sudo apt update
    sudo apt install python3 python3-pip
    sudo pip3 install virtualenv
    ```

2. Cloner le GRR repository:
    ```bash
    git clone https://github.com/google/grr.git
    cd grr
    ```

3. Configurer un environnement virtuel :
    ```bash
    virtualenv venv
    source venv/bin/activate
    ```

4. Installer GRR:
    ```bash
    pip install -e .
    ```

5. Initialiser le serveur GRR :
    ```bash
    grr_config_updater initialize
    ```

## Steps 

### Step 1: Configuration du Serveur GRR

**Objectif**: Installer et configurer le serveur GRR pour la gestion des points de terminaison.

1. Configurer les paramètres du serveur GRR :
    ```bash
    grr_config_updater set_var --section=Server.server --varname="AdminUI.url" --value="http://localhost:8000"
    ```
2. Démarrer le serveur GRR 
    ```bash
    grr_server --start
    ```
3. Accéder à l'interface web de GRR à l'adresse http://localhost:8000 et se connecter avec les identifiants par défaut

**Résultat attendu**: Serveur GRR en cours d'exécution et accessible via l'interface web.

### Step 2 : Déploiement des Agents GRR sur les Points de Terminaison

**Objectif**: Installer et configurer les agents GRR sur les points de terminaison Linux pour les capacités de forensique à distance.

1. Générer l'installateur GRR client 
    ```bash
    grr_client_build build --platform linux
    ```
2. Transférer l'installateur sur le point de terminaison Linux.
3. Installer l'agent GRR sur le point de terminaison :
    ```bash
    sudo dpkg -i grr-client_*.deb
    ```
4.  Vérifier que le point de terminaison apparaît dans l'interface web de GRR.

**Résultat attendu**: Les agents GRR sont installés et communiquent avec le serveur GRR
   

### step 3: Effectuer une Forensique en Temps Réel

**Objective**: Utiliser GRR pour collecter des données forensiques en temps réel à partir d'un point de terminaison distant.

1. Dans l'interface web de GRR, sélectionner un point de terminaison et aller dans "Flows" > "Launch New Flow".
2. Choisir "File Finder" et configurer la collecte de fichiers spécifiques (par exemple, /var/log/auth.log).
3. Lancer le flow et attendre que la collecte des données soit terminée.
4. Examiner les données collectées dans l'interface GRR.

**Résultat attendu **: Données forensiques collectées depuis le point de terminaison distant, accessibles dans l'interface GRR.

### step 4: Analyse de la Mémoire

**Objectif**: Capturer et analyser la mémoire d'un point de terminaison distant en utilisant GRR.
 
1. Dans l'interface web de GRR, sélectionner un point de terminaison et aller dans "Flows" > "Launch New Flow".
2. Choisir "Memory Collector" pour capturer l'image mémoire.
3. Lancer le flow et attendre que la capture de la mémoire soit terminée.
4. Télécharger l'image mémoire et l'analyser avec Volatility :
    ```bash
    volatility -f memory.img --profile=LinuxProfile linux_pslist
    ```

**Résultat attendu**: Image mémoire capturée et analysée pour des artefacts forensiques

### Step 5: Détection des Mécanismes de Persistance

**Objectif**: Identifier et analyser les mécanismes de persistance sur un point de terminaison distant

1. Dans l'interface web de GRR, sélectionner un point de terminaison et aller dans "Flows" > "Launch New Flow".
2. Choisir "ListProcesses" pour lister tous les processus en cours.
3. Lancer le flow et examiner la liste des processus pour détecter des entrées suspectes.

Analyser les scripts de démarrage et les services :
    ```bash
    grr_client --list_startup_items
    grr_client --list_services
    ```

**Résultat attendu**: Identification des mécanismes de persistance et analyse des processus et services suspects

## Conclusion

En complétant ces exercices, vous avez acquis des compétences avancées dans l'utilisation de GRR Rapid Response pour la forensique des points de terminaison Linux. Vous avez appris à configurer et déployer le serveur GRR, à installer des agents sur les points de terminaison, à collecter des données forensiques en temps réel, à effectuer une analyse mémoire et à détecter des mécanismes de persistance. Ces compétences sont essentielles pour mener des investigations forensiques à distance et répondre efficacement aux incidents de sécurité
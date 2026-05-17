# LAB-16-Inspection-HTTPS-Android-D-sactivation-du-SSL-Pinning-avec-Objection-Proxy-Burp-mitmproxy-

## 1. Objectif du laboratoire

Ce laboratoire a pour objectif de mettre en place une interception du trafic réseau d’une application Android à l’aide d’un proxy HTTP/HTTPS, puis de contourner le mécanisme de SSL Pinning en utilisant Frida et Objection.

L’objectif principal est de vérifier que les requêtes générées par l’application Android peuvent être interceptées et analysées dans Burp Suite, après avoir configuré le proxy, installé le certificat CA et exécuté la commande de désactivation du SSL Pinning.

## 2. Environnement utilisé

Le laboratoire a été réalisé dans l’environnement suivant :

Système hôte : Windows
Émulateur Android : Android 11
Application cible : InsecureBankv2
Package Android : com.android.insecurebankv2
Proxy utilisé : Burp Suite Community Edition
Port du proxy : 8080
Adresse proxy configurée sur Android : 192.168.11.130

Les outils utilisés sont :

ADB
Frida
frida-server
Objection
Burp Suite

## 3. Application utilisée

L’application utilisée dans ce laboratoire est InsecureBankv2, une application Android volontairement vulnérable, souvent utilisée dans les travaux pratiques de sécurité mobile.

Le package Android utilisé est :

com.android.insecurebankv2

## 4. Configuration du proxy Android

Dans l’émulateur Android, le réseau AndroidWifi a été configuré avec un proxy manuel.

La configuration appliquée est la suivante :

Proxy : Manual
Proxy hostname : 10.0.2.2
Proxy port : 8080
IP settings : DHCP

<img width="852" height="1846" alt="image" src="https://github.com/user-attachments/assets/2e345d0a-f5cd-4a8e-9d6c-f875a7c10dd7" />

Cette configuration permet de rediriger le trafic HTTP et HTTPS de l’émulateur Android vers Burp Suite, afin de pouvoir l’observer et l’analyser.

## 5. Configuration de Burp Suite

Dans Burp Suite, un listener proxy a été configuré sur le port 8080.

La configuration utilisée est la suivante :

Bind to port : 8080
Bind to address : All interfaces

<img width="876" height="587" alt="image" src="https://github.com/user-attachments/assets/6a71a6b4-bee0-4d54-99bc-3aa02d22c12d" />

Le choix de l’option All interfaces permet à l’émulateur Android de communiquer avec Burp Suite en utilisant l’adresse IP de la machine hôte.

## 6. Téléchargement du certificat CA Burp

Depuis le navigateur de l’émulateur Android, l’adresse suivante a été ouverte :

http://burp

<img width="759" height="376" alt="image" src="https://github.com/user-attachments/assets/fc4e3c30-b46e-4c2c-b4f2-9fe2eadf2a8d" />

L’affichage de la page Burp Suite Community Edition confirme que l’émulateur communique correctement avec le proxy Burp.

Le certificat CA a ensuite été téléchargé en cliquant sur le bouton :

CA Certificate

Le fichier obtenu est :

cacert.der

## 7. Installation du certificat CA sur Android

Après le téléchargement du certificat Burp, celui-ci a été installé dans Android à partir du menu de gestion des certificats.

Le chemin utilisé est le suivant :

Settings → Security → Encryption & credentials → Install a certificate → CA certificate

<img width="365" height="291" alt="image" src="https://github.com/user-attachments/assets/07ba0377-f287-4aa3-86c8-ac18b8a31d9b" />

Le certificat CA a été installé avec succès, comme l’indique le message :

CA certificate installed

Cette étape permet à Android de faire confiance aux certificats générés par Burp Suite lors de l’interception du trafic HTTPS.

## 8. Lancement de l’application avec Objection

L’application cible a été lancée avec Objection en utilisant son package Android :

<img width="1691" height="930" alt="image" src="https://github.com/user-attachments/assets/5511629e-1509-499f-83f2-a15d338de65e" />

## 9. Validation dans Burp Suite

Après la configuration du proxy, l’installation du certificat CA et la désactivation du SSL Pinning, l’application InsecureBankv2 a généré du trafic réseau visible dans Burp Suite.

Dans l’onglet HTTP history, plusieurs requêtes ont été interceptées, notamment vers l’adresse suivante :

<img width="2174" height="723" alt="image" src="https://github.com/user-attachments/assets/c5d6dde8-65cd-4242-9cd2-4db267db9ff0" />

Ces requêtes correspondent au trafic généré par l’application Android vers le backend InsecureBankv2.

Cette étape confirme que l’interception HTTPS fonctionne correctement et que le SSL Pinning a été désactivé avec succès.

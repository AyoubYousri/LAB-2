<img width="1532" height="917" alt="Capture d&#39;écran 2026-02-14 090058" src="https://github.com/user-attachments/assets/ea784d03-4f37-4238-a5de-7d8d07464105" />
Pour pouvoir modifier les fichiers système via la commande adb remount, j'ai lancé l'émulateur Pixel 6 avec l'argument spécifique -writable-system. Cette option est indispensable car elle lève la protection par défaut qui verrouille la partition système en lecture seule. J'ai également ajouté le paramètre -no-snapshot pour forcer un démarrage à froid et garantir que mes modifications soient bien prises en compte. Comme on peut le voir sur ma capture d'écran, le message "System image is writable" confirme que l'environnement est maintenant prêt pour l'injection de mes configurations.

<img width="1594" height="1042" alt="Capture d&#39;écran 2026-02-14 090111" src="https://github.com/user-attachments/assets/06ce3ea4-5e0b-4c81-b3ea-2f64df85965c" />
Pour finaliser l'accès en écriture, j'ai d'abord désactivé la vérification d'intégrité du système (dm-verity), ce qui a permis d'activer overlayfs sur les partitions clés comme /system et /vendor. Après avoir redémarré l'émulateur avec adb reboot et m'être assuré de posséder les privilèges root avec adb root, j'ai exécuté la commande adb remount. Comme le confirme le succès de l'opération ("Remount succeeded"), toutes les partitions système sont désormais montées en mode RW (Read-Write), me donnant ainsi la liberté totale de modifier les fichiers protégés de l'OS.

<img width="1576" height="535" alt="Capture d&#39;écran 2026-02-14 091723" src="https://github.com/user-attachments/assets/7eb2d08a-b780-4acb-b092-dd2dd947dae4" />
Pour confirmer la réussite de mes manipulations, j'ai vérifié mon identité système avec adb shell id, ce qui a confirmé que je possède bien les droits root (uid=0). J'ai également interrogé les propriétés du système pour vérifier l'état de sécurité : la valeur "orange" pour ro.boot.verifiedbootstate indique que le chargeur de démarrage est déverrouillé, confirmant que le système accepte mes modifications personnalisées.

 Étape 2 — Fiche périmètre 
Application : DivaApplication v1.0
Support : Émulateur Android Pixel 6 (AVD)
Objectif : Comprendre le rooting et ses impacts sur la sécurité Android
Données : Données fictives uniquement
Réseau : Environnement de test isolé
 Pourquoi on fait cette étape ?
Parce qu’en sécurité :
On définit toujours ce qu’on teste
On précise l’environnement
On précise qu’on ne touche pas à des données réelles
On montre que le test est contrôlé

<img width="1477" height="184" alt="Capture d&#39;écran 2026-02-14 180347" src="https://github.com/user-attachments/assets/e1713758-8f9a-415d-8cfc-98ec8a8da1f9" />
Une fois l'émulateur configuré et les accès sécurisés déverrouillés, j'ai procédé à l'installation de mon application de test. En utilisant la commande adb install app-debug.apk depuis le répertoire de compilation de mon projet Android Studio, j'ai obtenu une installation réussie ("Success"). Cela confirme que l'environnement est pleinement opérationnel pour le déploiement et le débogage de mes développements.

<img width="560" height="1160" alt="Capture d&#39;écran 2026-02-14 181148" src="https://github.com/user-attachments/assets/d64a43d3-9fc2-49c6-94e0-29d6ffdd1724" />
Après avoir finalisé l'installation, j'ai accédé à l'écran d'accueil de l'émulateur Pixel 6 pour vérifier la présence de mon application. On peut observer que l'interface système est fluide et que l'icône de mon projet (représentée par le logo Android par défaut) apparaît correctement dans la barre des favoris, confirmant le succès du déploiement.
<img width="564" height="1063" alt="Capture d&#39;écran 2026-02-14 181158" src="https://github.com/user-attachments/assets/39368b1f-078e-4f2f-be87-087cd049f585" />
Pour tester l'interaction utilisateur, j'ai ouvert la barre de recherche globale du système. En saisissant une requête (ici la lettre "y"), j'ai pu vérifier que l'émulateur indexe correctement les applications installées et propose des suggestions pertinentes vers des services comme YouTube, démontrant que les fonctionnalités de recherche et de navigation sont pleinement opérationnelles après mes modifications système.

8 Risques et 8 Mesures défensives :

Risques	Mesures défensives

Intégrité non garantie :	Réseau isolé.

Surface d’attaque accrue :	Données fictives uniquement.

Données sensibles exposées :	Device/AVD dédié aux tests.

Instabilité système	Snapshots : / wipe fin de séance.

Mélange comptes perso/test :	Journal de configuration détaillé.

Mauvais nettoyage fin de séance	 :Aucun compte personnel.

Réseau non isolé :	Contrôle strict des APK installées.

Traçabilité insuffisante :	Horodatage + captures pour traçabilité.

MASVS (2 exigences résumées)

STORAGE-1 → Stockage sécurisé des données sensibles (mots de passe, tokens, API keys)

NETWORK-1 → Communications réseau sécurisées via TLS et vérification des certificats

Fiche environnement

Date/auteur : 14/02/2026 – Ayoub Yousri

Support : AVD Pixel 6 API 33 (Google APIs, sans Play Store)

App + version : TP1 debug

3 scénarios : Écran d’accueil, recherche, détail

Observations : Fonctionnement normal, aucune erreur

Limites : Fastboot non disponible sur AVD

Reset effectué : Oui (via wipe / suppression userdata)




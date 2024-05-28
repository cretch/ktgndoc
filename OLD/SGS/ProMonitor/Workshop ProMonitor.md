## General presentation
---
ProMonitor Cockpit est la nouvelle évolution de ProMonitor.

Le module de base se charge de rapatrier les données d'alarmes (Collector) , le Cockpit permet de les visualiser, de voir leur historique, de les modifier (DROP l'alerte X par exemple)

L'opération quotidienne ne se fait que via le cockpit, le login se fait avec le compte standard, le SSO n'est pas possible.

---
### État de l'implémentation

- Le produit Cockpit est encore en plein développement (pas de release grand public)
- ProMonitor lui-même est mature, les mises à jour sont plus espacées
- L'installation du serveur de prod va commencer
- Suivi avec Agentil chaque semaine
- DATE DE RELEASE VERSION FINALE

---
## Organisation

---
### Utilisateurs
- Comptes locaux (admin)
- Comptes LDAP

Ces comptes sont soit admin, soit sont rattachés à des Teams, qui ont des **authorization profiles**. Il y a d'un côté les actions pouvant être effectuées sur un système et de l'autre les systèmes auxquels il a accès. Ceci permet de laisser par exemple le visuel sur le monitoring à d'autres équipes. 

Les comptes sont à faire à la main via l'admin, impossible de prendre un  groupe AD pour cela. 

---
### Collectors
- Les collectors sont les instances de ProMonitor, dans notre cas, il n'y a qu'un seul.
- On doit associer les collecteurs à un groupe de systèmes pour pouvoir remonter des données

---
### Systèmes

- Le classement se fait en  pulsieurs niveaux, dont les trois premiers sont juste pour classer. Ils ne contiennent pas de configuration particulière
- Organisation > Groupes > Systèmes
- Les systèmes ont un ou plusieurs connecteurs qui dépendent d'un utilisateur (ITS-NIMSRV)
- C'est dans les connecteurs eux-mêmes que se passe la réelle communication. Cela nécessite d'indiquer host, client, port et l'utilisateur SAP

---
### Alarmes
- Les alarmes, elles, sont regroupées en profils selon le type de système. Ces profils sont assignés aux connecteurs
- Des alarm rules s'appliquent ensuite, pour pouvoir filtrer, auto-classer, envoyer vers Derdack pour le piquet.

---
## Actions opérationnelles
---
### Nouveau compte
- Aller dans Settings > Users > Invite LDAP user
- Assigner une équipe et le rôle Admin au besoin

---
### Nouveau système
Dans le client 000
- DL la fiche de profil par défaut (ProMonitor)
- Importer et générer les autorisations
- Importer le profil dans le rôle NIMSOFT `ZA_U_XX_NIM_00`
- Transporter le rôle
- Vérifier que ITS-NIMSRV l'a bien
---
- Dans Cockpit > Monitoring > users, créer le compte ITS-NIMSRV liée à l'instance
- Dans Cockpit > Systèmes, ajouter le système et un/des connecteurs via clic-droit

---
### Modifier une alarme
- Aller dans le connecteur désiré
- Changer le taux de rafraîchissement, le seuil (par ex espace disque)
- Désactiver/réactiver
---
### Gestion des alertes
- Aller dans Alarms
- Quittancer
- Snooze
- Effacer
- Voir l'historique
---
### Mettre un noeud en maintenance
- Il suffit de déconnecter le connecteur
---
## Questions ?
---
## Discussion
- Destinations des mails
- Modification par admin uniquement VS par Basis?
- Classement des instances ? 
- Ouvrir la lecture à qui?
- Quelles instances monitorer?
- Quelles alarmes garder ?
- Lesquelles déclenchent le piquet ?
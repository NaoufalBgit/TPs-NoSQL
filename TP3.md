# TP3 – Sharding MongoDB

## 1. Qu’est-ce que le sharding dans MongoDB et pourquoi est-il utilisé ?
Le sharding est un mécanisme de partitionnement horizontal des données sur plusieurs serveurs afin d’améliorer la scalabilité et répartir la charge. 

## 2. Différence entre sharding et réplication
Le sharding répartit les données, la réplication duplique les données pour la tolérance aux pannes. 

## 3. Composants d’une architecture shardée
- Shards (stockage des données)
- Config servers (métadonnées)
- Mongos (routeur) 

## 4. Rôle des config servers (CSRS)
Ils stockent les métadonnées de sharding (chunks, clés, shards). Sans eux, le cluster est inutilisable. 

## 5. Rôle du mongos
Il agit comme routeur, reçoit les requêtes clients et les redirige vers les shards concernés. 

## 6. Comment MongoDB choisit le shard d’un document ?
À partir de la valeur de la clé de sharding, comparée aux plages de chunks. 

## 7. Clé de sharding : définition et importance
C’est l’attribut utilisé pour répartir les données. Elle conditionne performances et équilibre. 

## 8. Critères d’une bonne clé de sharding
- Forte cardinalité
- Bonne distribution
- Présente dans les requêtes 

## 9. Qu’est-ce qu’un chunk ?
Un chunk est une plage de valeurs de clé de sharding stockée sur un shard. 

## 10. Fonctionnement du splitting
Quand un chunk dépasse une taille seuil, il est automatiquement divisé en deux.

## 11. Rôle du balancer
Il équilibre la charge en redistribuant les chunks entre shards. 

## 12. Déplacement des chunks par le balancer
Il agit en arrière-plan lorsque des déséquilibres sont détectés.

## 13. Hot shard : définition et prévention
Shard surchargé. À éviter avec une bonne clé de sharding. 

## 14. Problèmes d’une clé monotone
Concentration des écritures sur un seul shard (hotspot).

## 15. Activation du sharding
- sh.enableSharding("db")
- sh.shardCollection("db.col", {key:1})

## 16. Ajouter un shard
Avec la commande :
sh.addShard("replicaSet/host:port") 

## 17. Vérifier l’état du cluster
Commandes : sh.status(), db.stats(), rs.status() 

## 18. Quand utiliser une clé hashée ?
Pour répartir uniformément les écritures et éviter les hotspots. 

## 19. Quand utiliser une clé range ?
Pour les requêtes par intervalles (range queries). 

## 20. Zone sharding : définition et intérêt
Associer des plages de clés à des shards spécifiques (localisation, SLA). 

## 21. Gestion des requêtes multi-shards
Le mongos diffuse la requête et agrège les résultats. 

## 22. Optimisation des performances
- Bon choix de clé
- Index adaptés
- Requêtes ciblant un shard 

## 23. Indisponibilité d’un shard
Les données du shard deviennent inaccessibles, mais le cluster reste partiellement opérationnel si réplication. 

## 24. Migrer une collection existante
Activer le sharding puis shardCollection ; MongoDB migre les chunks automatiquement. 

## 25. Outils et métriques de diagnostic
sh.status(), logs MongoDB, monitoring des chunks et du balancer.


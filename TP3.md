# TP – Sharding MongoDB : réponses aux questions

## 1. Qu’est-ce que le sharding dans MongoDB et pourquoi est-il utilisé ?
Le sharding est un mécanisme de partitionnement horizontal des données sur plusieurs serveurs afin d’améliorer la scalabilité et répartir la charge. :contentReference[oaicite:0]{index=0}

## 2. Différence entre sharding et réplication
Le sharding répartit les données, la réplication duplique les données pour la tolérance aux pannes. :contentReference[oaicite:1]{index=1}

## 3. Composants d’une architecture shardée
- Shards (stockage des données)
- Config servers (métadonnées)
- Mongos (routeur) :contentReference[oaicite:2]{index=2}

## 4. Rôle des config servers (CSRS)
Ils stockent les métadonnées de sharding (chunks, clés, shards). Sans eux, le cluster est inutilisable. :contentReference[oaicite:3]{index=3}

## 5. Rôle du mongos
Il agit comme routeur, reçoit les requêtes clients et les redirige vers les shards concernés. :contentReference[oaicite:4]{index=4}

## 6. Comment MongoDB choisit le shard d’un document ?
À partir de la valeur de la clé de sharding, comparée aux plages de chunks. :contentReference[oaicite:5]{index=5}

## 7. Clé de sharding : définition et importance
C’est l’attribut utilisé pour répartir les données. Elle conditionne performances et équilibre. :contentReference[oaicite:6]{index=6}

## 8. Critères d’une bonne clé de sharding
- Forte cardinalité
- Bonne distribution
- Présente dans les requêtes :contentReference[oaicite:7]{index=7}

## 9. Qu’est-ce qu’un chunk ?
Un chunk est une plage de valeurs de clé de sharding stockée sur un shard. :contentReference[oaicite:8]{index=8}

## 10. Fonctionnement du splitting
Quand un chunk dépasse une taille seuil, il est automatiquement divisé en deux. :contentReference[oaicite:9]{index=9}

## 11. Rôle du balancer
Il équilibre la charge en redistribuant les chunks entre shards. :contentReference[oaicite:10]{index=10}

## 12. Déplacement des chunks par le balancer
Il agit en arrière-plan lorsque des déséquilibres sont détectés. :contentReference[oaicite:11]{index=11}

## 13. Hot shard : définition et prévention
Shard surchargé. À éviter avec une bonne clé de sharding. :contentReference[oaicite:12]{index=12}

## 14. Problèmes d’une clé monotone
Concentration des écritures sur un seul shard (hotspot). :contentReference[oaicite:13]{index=13}

## 15. Activation du sharding
- sh.enableSharding("db")
- sh.shardCollection("db.col", {key:1}) :contentReference[oaicite:14]{index=14}

## 16. Ajouter un shard
Avec la commande :
sh.addShard("replicaSet/host:port") :contentReference[oaicite:15]{index=15}

## 17. Vérifier l’état du cluster
Commandes : sh.status(), db.stats(), rs.status() :contentReference[oaicite:16]{index=16}

## 18. Quand utiliser une clé hashée ?
Pour répartir uniformément les écritures et éviter les hotspots. :contentReference[oaicite:17]{index=17}

## 19. Quand utiliser une clé range ?
Pour les requêtes par intervalles (range queries). :contentReference[oaicite:18]{index=18}

## 20. Zone sharding : définition et intérêt
Associer des plages de clés à des shards spécifiques (localisation, SLA). :contentReference[oaicite:19]{index=19}

## 21. Gestion des requêtes multi-shards
Le mongos diffuse la requête et agrège les résultats. :contentReference[oaicite:20]{index=20}

## 22. Optimisation des performances
- Bon choix de clé
- Index adaptés
- Requêtes ciblant un shard :contentReference[oaicite:21]{index=21}

## 23. Indisponibilité d’un shard
Les données du shard deviennent inaccessibles, mais le cluster reste partiellement opérationnel si réplication. :contentReference[oaicite:22]{index=22}

## 24. Migrer une collection existante
Activer le sharding puis shardCollection ; MongoDB migre les chunks automatiquement. :contentReference[oaicite:23]{index=23}

## 25. Outils et métriques de diagnostic
sh.status(), logs MongoDB, monitoring des chunks et du balancer. :contentReference[oaicite:24]{index=24}


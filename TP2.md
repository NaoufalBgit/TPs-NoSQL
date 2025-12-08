# TP2 NoSQL

# Réplication MongoDB

## Objectif  
Comprendre comment fonctionne la réplication dans MongoDB, comment un cluster réagit aux pannes, et comment mettre en place un replica set localement.

# 1. Principes généraux des systèmes distribués

### Communication entre nœuds  
Tous les nœuds échangent des messages pour :  
- répliquer les données  
- vérifier l’état des nœuds  
- détecter les pannes  

### Panne d’un nœud secondaire  
Le primary détecte l’absence de réponse et marque le nœud inactif. La charge peut être réaffectée.

### Panne du primary  
Un processus d’élection automatique démarre (algorithmes type Paxos/Raft).  
Les nœuds restants élisent un nouveau maître.

### Partition réseau (split-brain)  
Si le cluster est coupé en deux :  
- chaque groupe pourrait élire un primary → incohérence  
- seule la partie majoritaire continue à fonctionner  
MongoDB applique cette règle.

# 2. Rappels sur la réplication MongoDB

### Architecture Primary / Secondary  
Equivalent à maître / esclave.  
- Toutes les écritures vont au primary.  
- Par défaut, les lectures aussi → cohérence forte.

### Réplication asynchrone  
Le primary :  
1. écrit dans un journal (log)  
2. envoie l’ack au client  
3. commence la réplication vers les secondaires  

→ Les secondaires peuvent être en retard.

### Lecture sur un secondaire  
Possible uniquement si on active :  
rs.secondaryOk()

Peut renvoyer des données non à jour.

### Importance du journal  
Écriture séquentielle = performance stable.

# 3. Pannes et élections

### Si le primary tombe  
- les secondaires élisent un nouveau primary  
- si aucun groupe n’a la majorité → mode dégradé (pas d’écriture possible)

### Ajout d’un arbitre  
Nœud spécial qui :  
- ne stocke pas de données  
- participe au vote  
- sert à atteindre une majorité lors des élections

# 4. Mise en place d’un Replica Set local

### Étapes

1. Créer un dossier par serveur  
disque1/, disque2/, disque3/

2. Lancer trois instances mongod  
mongod --replSet monReplica --port 27018 --dbpath disque1
mongod --replSet monReplica --port 27019 --dbpath disque2
mongod --replSet monReplica --port 27020 --dbpath disque3

3. Initialiser le replica set  
mongo --port 27018
rs.initiate()

4. Ajouter les autres nœuds  
rs.add("localhost:27019")
rs.add("localhost:27020")

5. Vérifier l’état  
- rs.config() -> configuration statique  
- rs.status() -> état temps réel  
- db.isMaster() -> savoir si on est sur le primary

6. Lecture / écriture  
- écriture -> uniquement sur le primary  
- lecture secondaire -> rs.secondaryOk()

# 5. Démonstrations et pannes

### Insertion de données sur primary  
OK.

### Lecture sur secondaire  
Possible seulement après rs.secondaryOk().

### Écriture sur secondaire  
Toujours impossible.

### Panne du primary  
Arrêt du port 27018 -> les secondaires déclenchent une élection -> un nouveau primary apparaît.

### Ajout d’un arbitre  
mongod --replSet monReplica --port 27021 --dbpath arbitre1
rs.addArb("localhost:27021")

# À retenir

- MongoDB favorise une cohérence forte.  
- Le primary gère toutes les écritures.  
- La réplication est asynchrone.  
- La majorité des nœuds est indispensable pour élire un primary.  
- Lecture secondaire = risque de données obsolètes.  
- Le replica set != montée en charge (c’est le sharding qui sert à cela).  
- Un arbitre peut éviter des blocages lors des élections.

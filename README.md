
# Zabbix Template - Galera Cluster Monitoring

## 📌 Description
Ce template **Zabbix 7.4** permet de superviser un **cluster Galera** via l'agent Zabbix.
Il s'appuie sur des **UserParameters** pour interroger les variables `wsrep`.

### **Fonctionnalités**
- Vérification de l'intégrité du cluster
- Surveillance de l'état des nœuds
- Suivi des performances de réplication
- Détection des problèmes réseau
- Statistiques diverses (transactions, files d'attente, etc.)

---

## ✅ **Installation**
   1a. **Telecharger le fichier galera.conf** dans le dossier `zabbix_agent2.d/plugins.d` :
   ```bash
   apt update -y && apt install -y wget
   wget https://raw.githubusercontent.com/thorbeorn/Zabbix-Galera-Cluster-Monitoring/refs/heads/main/galera.conf -O /etc/zabbix/zabbix_agent2.d/plugins.d/galera.conf
   ```
   1b. **Copiez galera.conf** dans le fichier `zabbix_agent2.d/plugins.d/galera.conf` :
   ```bash
   nano /etc/zabbix/zabbix_agent2.d/plugins.d/galera.conf
   ```
2. Redémarrez l'agent Zabbix :
   ```bash
   systemctl restart zabbix-agent2
   ```
3. **Testez une clé** pour vérifier :
   ```bash
   zabbix_agent2 -t "galera.node.connected"
   ```
4. **Assurez-vous que l'utilisateur Zabbix a accès à MySQL** :
   ```bash
   mysql -u zabbix -p
   ```
---

## ✅ **Clés Zabbix et Description (galera.conf)**

|              Clé Zabbix             |       Variable Galera      |                                Description                                |
|:-----------------------------------:|:--------------------------:|:-------------------------------------------------------------------------:|
| galera.cluster.uuid                 | wsrep_cluster_state_uuid   | UUID unique représentant l'état actuel du cluster.                        |
| galera.cluster.conf_id              | wsrep_cluster_conf_id      | ID de configuration actuel du cluster (nombre de changements de membres). |
| galera.cluster.size                 | wsrep_cluster_size         | Nombre total de nœuds dans le cluster.                                    |
| galera.cluster.status               | wsrep_cluster_status       | Statut global du cluster (Primary, Non-Primary, Disconnected).            |
| galera.cluster.last_committed       | wsrep_last_committed       | Dernier ID de transaction validée dans le cluster.                        |
| galera.cluster.cluster_weight       | wsrep_cluster_weight       | Poids total du cluster (somme des poids des nœuds).                       |
| galera.cluster.evs_delayed          | wsrep_evs_delayed          | Liste des nœuds ayant des messages retardés.                              |
| galera.cluster.evs_evict_list       | wsrep_evs_evict_list       | Liste des nœuds exclus du cluster.                                        |
| galera.cluster.evs_state            | wsrep_evs_state            | État interne du protocole EVS.                                            |
| galera.cluster.gcomm_uuid           | wsrep_gcomm_uuid           | UUID interne du protocole GComm.                                          |
| galera.cluster.cluster_capabilities | wsrep_cluster_capabilities | Capacités activées pour le cluster.                                       |
| galera.cluster.provider_name        | wsrep_provider_name        | Nom du provider Galera (ex: Galera).                                      |
| galera.cluster.provider_vendor      | wsrep_provider_vendor      | Fournisseur du provider (ex: Codership).                                  |
| galera.cluster.provider_version     | wsrep_provider_version     | Version du provider Galera.                                               |
| galera.cluster.protocol_version     | wsrep_protocol_version     | Version du protocole Galera.                                              |
| galera.node.ready                   | wsrep_ready                   | Indique si le nœud est prêt à accepter des transactions (ON/OFF).   |
| galera.node.connected               | wsrep_connected               | Indique si le nœud est connecté au cluster.                         |
| galera.node.status                  | wsrep_local_state_comment     | Statut local du nœud (Synced, Donor, Joiner).                       |
| galera.node.local_cert_failures     | wsrep_local_cert_failures     | Nombre d’échecs de certification des transactions locales.          |
| galera.node.local_bf_aborts         | wsrep_local_bf_aborts         | Nombre d’abandons dus aux conflits de certification locale.         |
| galera.node.flow_control_paused_ns  | wsrep_flow_control_paused_ns  | Temps total en pause à cause du Flow Control (en ns).               |
| galera.node.apply_oool              | wsrep_apply_oool              | Transactions appliquées hors ordre.                                 |
| galera.node.apply_waits             | wsrep_apply_waits             | Nombre de fois que l’applier a attendu l’ordre des transactions.    |
| galera.node.commit_oool             | wsrep_commit_oool             | Transactions validées hors ordre.                                   |
| galera.node.desync_count            | wsrep_desync_count            | Nombre de fois que le nœud est passé en mode désynchronisé.         |
| galera.node.gmcast_segment          | wsrep_gmcast_segment          | Segment du cluster auquel appartient le nœud.                       |
| galera.node.applier_thread_count    | wsrep_applier_thread_count    | Nombre de threads appliquant les transactions.                      |
| galera.node.provider_capabilities   | wsrep_provider_capabilities   | Capacités activées pour le provider sur ce nœud.                    |
| galera.node.rollbacker_thread_count | wsrep_rollbacker_thread_count | Nombre de threads rollbacker sur le nœud.                           |
| galera.node.thread_count            | wsrep_thread_count            | Nombre total de threads wsrep (applier/rollbacker).                 |
| galera.node.ist_receive_status      | wsrep_ist_receive_status      | État actuel de la synchronisation IST (Incremental State Transfer). |
| galera.replication.flow_control_paused | wsrep_flow_control_paused  | Fraction du temps depuis le dernier FLUSH STATUS où le flow control a suspendu la réplication.                           |
| galera.replication.cert_deps_distance  | wsrep_cert_deps_distance   | Distance moyenne entre le plus haut et le plus bas seqno pouvant être appliqué en parallèle (potentiel de parallélisme). |
| galera.network.recv_queue_avg          | wsrep_local_recv_queue_avg | Longueur moyenne de la file d’attente de réception depuis le dernier FLUSH STATUS.                                       |
| galera.network.recv_queue_max          | wsrep_local_recv_queue_max | Longueur maximale de la file d’attente de réception depuis le dernier FLUSH STATUS.                                      |
| galera.network.recv_queue_min          | wsrep_local_recv_queue_min | Longueur minimale de la file d’attente de réception depuis le dernier FLUSH STATUS.                                      |
| galera.network.send_queue_avg          | wsrep_local_send_queue_avg | Longueur moyenne de la file d’attente d’envoi depuis le dernier FLUSH STATUS.                                            |
| galera.network.send_queue_max          | wsrep_local_send_queue_max | Longueur maximale de la file d’attente d’envoi depuis le dernier FLUSH STATUS.                                           |
| galera.network.send_queue_min          | wsrep_local_send_queue_min | Longueur minimale de la file d’attente d’envoi depuis le dernier FLUSH STATUS.                                           |
| galera.network.incoming_addresses      | wsrep_incoming_addresses   | Liste des adresses entrantes du cluster.                                                                                 |
| galera.network.evs_repl_latency        | wsrep_evs_repl_latency     | Latence de réplication EVS (min/avg/max/stddev/sample size).                                                             |
| galera.misc.replicated             | wsrep_replicated             | Nombre total de write-sets répliqués vers les autres nœuds.                         |
| galera.misc.replicated_bytes       | wsrep_replicated_bytes       | Taille totale en octets des write-sets répliqués.                                   |
| galera.misc.repl_keys              | wsrep_repl_keys              | Nombre total de clés répliquées.                                                    |
| galera.misc.repl_keys_bytes        | wsrep_repl_keys_bytes        | Taille totale en octets des clés répliquées.                                        |
| galera.misc.repl_data_bytes        | wsrep_repl_data_bytes        | Taille totale des données répliquées.                                               |
| galera.misc.repl_other_bytes       | wsrep_repl_other_bytes       | Taille totale des autres données répliquées.                                        |
| galera.misc.received               | wsrep_received               | Nombre total de write-sets reçus depuis d’autres nœuds.                             |
| galera.misc.received_bytes         | wsrep_received_bytes         | Taille totale en octets des write-sets reçus.                                       |
| galera.misc.local_commits          | wsrep_local_commits          | Nombre total de transactions locales validées.                                      |
| galera.misc.local_replays          | wsrep_local_replays          | Nombre total de replays de transactions locales à cause de verrous asymétriques.    |
| galera.misc.local_send_queue       | wsrep_local_send_queue       | Longueur instantanée de la file d’envoi locale.                                     |
| galera.misc.local_recv_queue       | wsrep_local_recv_queue       | Longueur instantanée de la file de réception locale.                                |
| galera.misc.local_cached_downto    | wsrep_local_cached_downto    | Plus petit seqno dans le cache de write-set (GCache).                               |
| galera.misc.local_state            | wsrep_local_state            | État FSM interne du nœud Galera.                                                    |
| galera.misc.local_index            | wsrep_local_index            | Index du nœud dans le cluster (base 0).                                             |
| galera.misc.flow_control_sent      | wsrep_flow_control_sent      | Nombre d’événements FC_PAUSE envoyés par le nœud.                                   |
| galera.misc.flow_control_recv      | wsrep_flow_control_recv      | Nombre d’événements FC_PAUSE reçus par le nœud.                                     |
| galera.misc.flow_control_active    | wsrep_flow_control_active    | Indique si le flow control est actif (réplication suspendue).                       |
| galera.misc.flow_control_requested | wsrep_flow_control_requested | Indique si le nœud a demandé la suspension de la réplication.                       |
| galera.misc.apply_oooe             | wsrep_apply_oooe             | Fréquence à laquelle les write-sets ont été appliqués hors ordre.                   |
| galera.misc.apply_window           | wsrep_apply_window           | Distance moyenne entre le seqno le plus haut et le plus bas appliqué simultanément. |
| galera.misc.commit_oooe            | wsrep_commit_oooe            | Fréquence à laquelle les transactions ont été validées hors ordre.                  |
| galera.misc.commit_window          | wsrep_commit_window          | Distance moyenne entre le seqno le plus haut et le plus bas validé simultanément.   |
| galera.misc.cert_index_size        | wsrep_cert_index_size        | Nombre d’entrées dans l’index de certification.                                     |
| galera.misc.cert_interval          | wsrep_cert_interval          | Nombre moyen de transactions reçues pendant la réplication d’un write-set.          |
| galera.misc.open_transactions      | wsrep_open_transactions      | Nombre de transactions locales en cours.                                            |
| galera.misc.open_connections       | wsrep_open_connections       | Nombre de connexions ouvertes dans le provider wsrep.                               |

## ✅ **Nom du Trigger, Macro(s), Gravité, Valeur par défaut (template_galera_zabbix.xml)**
|                          Nom du Trigger                         |                   Macro(s)                   |    Gravité    |                Valeur par défaut               |
|:---------------------------------------------------------------:|:--------------------------------------------:|:-------------:|:----------------------------------------------:|
| Galera Cluster: Le cluster n'est plus primaire                  | (Aucune macro)                               | Désastre      | (n/a)                                          |
| Galera Cluster: Quorum perdu                                    | {$CLUSTER.NBR_NODE}, {$CLUSTER.NBR_ARBITROR} | Désastre      | {$CLUSTER.NBR_NODE}=2, {$CLUSTER.NBR_ARBITROR}=1 |
| Galera Cluster: Taille réduite                                  | {$CLUSTER.NBR_NODE}, {$CLUSTER.NBR_ARBITROR} | Moyen         | {$CLUSTER.NBR_NODE}=2, {$CLUSTER.NBR_ARBITROR}=1 |
| Galera Cluster: UUID changé                                     | (Aucune macro)                               | Avertissement | (n/a)                                          |
| Galera Misc: Application hors ordre fréquente                   | {$GALERA_APPLY_OOOE_WARN}                    | Information   | 10                                             |
| Galera Misc: File locale d'envoi trop longue                    | {$GALERA_LOCAL_SEND_QUEUE_WARN}              | Avertissement | 100                                            |
| Galera Misc: File locale de réception trop longue               | {$GALERA_LOCAL_RECV_QUEUE_WARN}              | Avertissement | 100                                            |
| Galera Misc: Flow control actif"                                | (Pas de macro)                               | Information   | (n/a)                                          |
| Galera Misc: Flow control demandé"                              | (Pas de macro)                               | Avertissement | (n/a)                                          |
| Galera Misc: Trop d'événements Flow Control envoyés             | {$GALERA_FLOW_CTRL_SENT_WARN}                | Avertissement | 10                                             |
| Galera Misc: Trop de connexions ouvertes                        | {$GALERA_OPEN_CONN_WARN}                     | Information   | 500                                            |
| Galera Misc: Trop de transactions ouvertes                      | {$GALERA_OPEN_TX_WARN}                       | Moyen         | 100                                            |
| Galera Network: Longueur moyenne de la file de réception élevée | {$GALERA_RECV_QUEUE_WARN}                    | Avertissement | 100                                            |
| Galera Network: Temps de réplication trop élevé                 | {$GALERA_REPL_LATENCY_WARN}                  | Haut          | 0.1 (100 ms)                                   |
| Galera Network: Variation importante dans la file de réception  | {$GALERA_QUEUE_DIFF_WARN}                    | Information   | 50                                             |
| Galera Node: Bloqué en mode Donor                               | (Pas de macro)                               | Moyen         | (n/a)                                          |
| Galera Node: Disconnected                                       | (Pas de macro)                               | Désastre      | (n/a)                                          |
| Galera Node: Haut temps de pause pour le flow control           | {$GALERA_FC_PAUSED_WARN}                     | Moyen         | 0.2 (20%)                                      |
| Galera Node: Le node n'est pas synchronisé                      | (Pas de macro)                               | Haut          | (n/a)                                          |
| Galera Node: Node rejette les requêtes                          | (Pas de macro)                               | Haut          | (n/a)                                          |
| Galera Node: Pas prêt                                           | (Pas de macro)                               | Haut          | (n/a)                                          |
| Galera Replication: Flow control activé plus de 10% du temps    | {$GALERA_FLOW_CONTROL_WARN}                  | Moyen         | 0.1 (10%)                                      |
| Galera Replication: Parallélisme faible                         | {$GALERA_CERT_DEPS_WARN}                     | Information   | 0.1                                            |
---

## 🔗 **Référence**
- [Documentation officielle Galera wsrep status variables](https://galeracluster.com/library/documentation/galera-status-variables.html#wsrep-ist-receive-status)

---

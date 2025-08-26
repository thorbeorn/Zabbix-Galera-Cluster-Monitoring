
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
   wget https://raw.githubusercontent.com/thorbeorn/Zabbix-Galera-Cluster-      Monitoring/refs/heads/main/galera.conf -O /etc/zabbix/zabbix_agent2.d/plugins.d/galera.conf
   ```
   1b. **Copiez galera.conf** dans le fichier `zabbix_agent2.d/plugins.d/galera.conf` :
   ```bash
   nano /etc/zabbix/zabbix_agent2.d/plugins.d/galera.conf
   ```
2. **Testez une clé** pour vérifier :
   ```bash
   zabbix_agent2 -t "galera.node.connected"
   ```
3. **Assurez-vous que l'utilisateur Zabbix a accès à MySQL** :
   ```bash
   mysql -u zabbix -p
   ```
4. Redémarrez l'agent Zabbix :
   ```bash
   systemctl restart zabbix-agent
   ```

---

## ✅ **Clés Zabbix et Description**

| **Clé Zabbix** | **Description** |
|----------------------------------------|---------------------------------------------------------------------------------|
| `galera.cluster.uuid` | UUID unique représentant l'état actuel du cluster. |
| `galera.cluster.conf_id` | ID de configuration actuel du cluster. |
| `galera.cluster.size` | Nombre total de nœuds dans le cluster. |
| `galera.cluster.status` | Statut global du cluster (Primary ou Non-Primary). |
| `galera.cluster.last_committed` | Dernier ID de transaction validée dans le cluster. |
| `galera.cluster.cluster_weight` | Poids total du cluster (pour le quorum). |
| `galera.cluster.evs_delayed` | Nœuds ayant des messages retardés. |
| `galera.cluster.evs_evict_list` | Liste des nœuds exclus du cluster. |
| `galera.cluster.evs_state` | État interne du protocole EVS. |
| `galera.cluster.gcomm_uuid` | UUID interne du protocole GComm. |
| `galera.cluster.cluster_capabilities` | Capacités activées pour le cluster. |
| `galera.cluster.provider_name` | Nom du provider Galera (ex: Galera). |
| `galera.cluster.provider_vendor` | Fournisseur du provider (ex: Codership). |
| `galera.cluster.provider_version` | Version du provider Galera. |
| `galera.cluster.protocol_version` | Version du protocole Galera. |
| `galera.node.ready` | Indique si le nœud est prêt à accepter des transactions (ON/OFF). |
| `galera.node.connected` | Si le nœud est connecté au cluster. |
| `galera.node.status` | Statut local du nœud (Synced, Donor, Joiner). |
| `galera.node.local_cert_failures` | Nombre d'échecs de certification des transactions. |
| `galera.node.local_bf_aborts` | Nombre d'abandons dus aux conflits de certification. |
| `galera.node.flow_control_paused_ns` | Temps total en pause à cause du Flow Control (en ns). |
| `galera.node.apply_oool` | Transactions appliquées hors ordre. |
| `galera.node.commit_oool` | Transactions validées hors ordre. |
| `galera.node.desync_count` | Nombre de fois que le nœud est passé en mode désynchronisé. |
| `galera.node.applier_thread_count` | Nombre de threads appliquant les transactions. |
| `galera.node.ist_receive_status` | État actuel de la synchronisation IST (Incremental State Transfer). |
| `galera.replication.flow_control_paused` | Pourcentage du temps passé en pause par le Flow Control. |
| `galera.replication.cert_deps_distance` | Distance moyenne des dépendances de certification. |
| `galera.network.recv_queue_avg` | Moyenne des messages dans la file de réception. |
| `galera.network.send_queue_avg` | Moyenne des messages dans la file d'envoi. |
| `galera.network.evs_repl_latency` | Latence de réplication entre les nœuds. |
| `galera.misc.replicated` | Nombre total de transactions répliquées. |
| `galera.misc.local_commits` | Nombre de transactions validées localement. |
| `galera.misc.open_connections` | Nombre actuel de connexions ouvertes. |

---

## 🔗 **Référence**
- [Documentation officielle Galera wsrep status variables](https://galeracluster.com/library/documentation/galera-status-variables.html#wsrep-ist-receive-status)

---

# Cassandra Docker Labs

Due laboratori alternativi, ciascuno con il proprio server Jupyter (anch'esso container):

| Laboratorio | Cartella | Servizi | RF |
|---|---|---|---|
| Single node | [`single-node/`](single-node) | `cassandra` + `jupyter` | 1 |
| Cluster a 3 nodi | [`three-node/`](three-node) | `cassandra1..3` + `jupyter` | 3 |

Il notebook [`notebooks/cassandra_basics.ipynb`](notebooks/cassandra_basics.ipynb) è lo stesso
per entrambi: legge `CASSANDRA_HOSTS` e `REPLICATION_FACTOR` dall'ambiente del container.

## Avvio

Requisiti: Docker con Compose v2. Avvia **un solo laboratorio alla volta** (usano le stesse
porte 9042 e 8888).

```bash
# Single node
cd single-node && docker compose up -d --build

# oppure, cluster a tre nodi (il bootstrap sequenziale richiede alcuni minuti)
cd three-node && docker compose up -d --build
```

Poi apri <http://localhost:8888/lab?token=cassandra> e il notebook `cassandra_basics.ipynb`.
Cassandra è raggiungibile anche dal tuo computer su `localhost:9042` (es. `cqlsh`).

## Contenuto del notebook

Connessione e topologia · keyspace (`NetworkTopologyStrategy`) · partition/clustering key ·
CRUD · prepared statement e batch · TTL · lightweight transaction · collezioni e UDT ·
modellazione una-tabella-per-query · indici secondari · consistency level ·
replica e token · integrazione con pandas.

## Comandi utili

```bash
docker compose ps                         # stato dei container
docker exec -it cassandra-single cqlsh    # shell CQL (single node)
docker exec -it cassandra1 nodetool status  # stato dell'anello (tre nodi)
docker stop cassandra3                    # simula un guasto (tre nodi)
```

## Arresto e pulizia

```bash
docker compose down        # ferma, mantiene i dati
docker compose down -v     # ferma e cancella i dati (volumi)
```

## Note

* Heap di Cassandra limitato a 512 MB per nodo: il cluster a tre nodi richiede circa 3–4 GB di RAM per Docker.
* Jupyter usa il token `cassandra` (solo laboratorio, modificabile in `JUPYTER_TOKEN`).
* I notebook sono montati da `notebooks/`: le modifiche persistono sul tuo disco.

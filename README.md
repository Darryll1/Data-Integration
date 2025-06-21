#  Projet Data Integration – Pipeline de Traitement Distribué

##  Objectif

Ce projet met en œuvre une **architecture de traitement de données distribuée** autour d’un pipeline complet depuis la collecte jusqu’à l’analyse, en s’appuyant sur des technologies Big Data modernes. Le projet s’inscrit dans une logique d’**intégration continue, d’automatisation, de scalabilité** et de **monitoring**.

---

##  Architecture générale

Le projet repose sur les composants suivants :

- **Kafka** : ingestion temps réel des données via un producteur Python
- **Spark Streaming** : traitement distribué des données entrantes
- **HDFS** : stockage distribué des fichiers sources et agrégés
- **Airflow** : orchestration des tâches de traitement batch
- **FastAPI** : exposer des résultats via une API REST
- **Prometheus & Grafana** : monitoring et visualisation des métriques
- **Docker Compose** : containerisation de l’ensemble des services

---

##  Modules principaux

| Dossier/Fichier | Description |
|------------------|-------------|
| `kafka_producer/producer.py` | Envoie les messages dans Kafka |
| `spark_streaming/consumer.py` | Consomme les messages Kafka et les traite avec Spark |
| `hdfs_data/hdfs_reader.py` | Lecture des fichiers CSV stockés dans HDFS |
| `spark_streaming/cube_population.py` | Création de cubes analytiques sur les données |
| `api/main.py` | API REST en FastAPI pour exposer les données |
| `dags/data_pipeline_dag.py` | DAG Airflow pour orchestrer les étapes batch |
| `docker-compose.yml` | Lancement coordonné de tous les conteneurs |
| `prometheus.yml` | Configuration du monitoring avec Prometheus |

---

Démarrage du projet
 
### 1. Lancer les conteneurs Docker
 
```bash
docker compose up --build -d
 
Cela démarre tous les services nécessaires : Kafka, Spark, Airflow, FastAPI, Prometheus, Grafana…
 
Accéder aux interfaces
 
Composant	URL
API FastAPI	http://localhost:8000/docs
Grafana	http://localhost:3000 (admin/admin)
Prometheus	http://localhost:9090
Airflow	http://localhost:8080 (admin/admin)
 
Monitoring
Prometheus collecte les métriques exposées par le traitement Spark Streaming.
Grafana visualise ces métriques (ex. messages traités, erreurs, durée par message)
## Traitement des données – Pipeline
1. Kafka Producer
Envoie des lignes de total-population.csv en boucle toutes les 10 secondes dans le topic population_data.
2. HDFS Reader
Lit plusieurs fichiers CSV (revenus, assurance santé, travail indépendant)
Nettoie, harmonise et effectue une jointure sur l’ID
Génère un fichier enrichi all_data_joined.csv
3. Spark Consumer
Consomme les messages Kafka
Enrichit les données en rejoignant avec all_data_joined.csv
Nettoie, transforme et insère les résultats dans donnees_formatées (base SQLite)
Expose des métriques Prometheus
4. Cube Generator
Calcule des cubes par tranche d'âge, secteur ou revenus
Agrège les données par quartier (Neighborhood)
Stocke les cubes dans cube_analytics.db
## API FastAPI
L’API REST expose les données agrégées :
Endpoint	Description
/api/health-insurance/18-34	Santé 18–34 ans
/api/self-employment	Revenus indépendants
/api/income	Revenu global
 
Toutes les données sont lues directement dans la base cube_analytics.db.
Orchestration Airflow
Le DAG Airflow exécute automatiquement les étapes suivantes :
start_producer : Simulation Kafka
transform_hdfs : Nettoyage et jointure des données HDFS
start_consumer : Traitement Spark Streaming
compute_cube : Calcul et stockage des cubes
Planification : quotidienne (schedule_interval="daily")


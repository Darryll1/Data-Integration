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


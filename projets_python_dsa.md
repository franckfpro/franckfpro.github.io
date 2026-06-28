# Projets Python : Expertise en Structures de Données (DSA) & Clean Code

Ce document regroupe trois propositions de projets conçus pour approfondir la maîtrise de Python, des structures de données et des architectures logicielles propres.

---

## 1. Moteur d'Indexation et de Recherche Textuelle (Inverted Index)

Ce projet permet de comprendre les mécanismes fondamentaux des bases de données textuelles et des moteurs de recherche modernes à petite échelle.

* **Le Besoin :** Charger un grand volume de fichiers texte, les indexer efficacement en mémoire, et effectuer des recherches par mots-clés instantanées avec un calcul de score de pertinence.
* **Concepts DSA clés :**
  * *Hash Maps* (Dictionnaires) pour l'accès en $O(1)$.
  * *Sets* (Ensembles) pour les opérations d'intersection et d'union lors de recherches multi-mots.
  * Algorithme de scoring (TF-IDF simplifié).
* **Éléments et architecture requis :**
  * **Pipeline de Tokenisation :** Un module chargé de lire les fichiers, normaliser le texte (minuscules, suppression de la ponctuation) et filtrer les *stop words* (le, la, un, etc.).
  * **L'Index Inversé :** Une structure de données de type `dict[str, dict[str, int]]` où chaque clé est un mot unique, et la valeur est un sous-dictionnaire associant le chemin du fichier au nombre d'occurrences.
  * **Module de Requête :** Une fonction qui décompose une phrase (ex: `"python automation"`), extrait les documents pertinents et applique une formule de score pour trier les résultats par pertinence.
* **Bonnes pratiques & Approche Pythonic :**
  * Utilisation de `pathlib` pour la gestion robuste des chemins de fichiers.
  * Utilisation de `collections.defaultdict` pour simplifier la construction de l'index sans vérifications manuelles d'existence de clés.

---

## 2. Planificateur de Tâches avec Priorités (Task Scheduler)

Ce projet simule le comportement d'un orchestrateur de jobs ou d'un gestionnaire de processus (OS) capable d'ordonnancer des tâches asynchrones interdépendantes.

* **Le Besoin :** Un système d'ordonnancement capable de recevoir des tâches dotées de niveaux de priorité variables et de dépendances mutuelles, puis de les exécuter dans l'ordre optimal sans blocage.
* **Concepts DSA clés :**
  * *Min-Heap / Max-Heap* (File de priorité) pour l'extraction de la tâche prioritaire en $O(\log n)$.
  * *Graphe Dirigé Acyclique (DAG)* pour la modélisation des dépendances.
  * *Tri Topologique* (algorithme de Kahn ou DFS) pour déterminer l'ordre d'exécution.
* **Éléments et architecture requis :**
  * **Gestion des Priorités :** Utilisation du module natif `heapq` de Python pour maintenir la structure d'arbre (tas) en mémoire.
  * **Résolution des Dépendances :** Validation du graphe pour détecter d'éventuels cycles (deadlocks) avant de générer la file d'attente finale.
  * **Moteur d'Exécution :** Un worker simulé via `asyncio` ou `concurrent.futures` pour consommer la file et exécuter les tâches de manière asynchrone.
* **Bonnes pratiques & Approche Pythonic :**
  * Implémentation du design pattern *Command* pour encapsuler le comportement de chaque tâche.
  * Utilisation de `dataclasses` en surchargeant les opérateurs de comparaison (`__lt__`) pour permettre au module `heapq` de trier les objets de manière native et élégante.

---

## 3. Système de Rate-Limiting Haute Performance (API Throttler)

Un composant infrastructurel indispensable pour protéger les architectures d'API contre les surcharges, les abus ou les attaques par déni de service (DoS).

* **Le Besoin :** Un middleware ou composant de sécurité capable de valider ou rejeter (HTTP 429) une requête entrante en quelques millisecondes, selon l'identité du client (IP, token) et son quota.
* **Concepts DSA clés :**
  * *Sliding Window Log* (Fenêtre glissante via une file à double entrée / Deque).
  * *Token Bucket* (Algorithme à seau de jetons).
* **Éléments et architecture requis :**
  * **Algorithme de Fenêtre Glissante :** Utilisation d'une `collections.deque` pour stocker les horodatages (*timestamps*) des requêtes. Chaque appel nettoie les entrées obsolètes et vérifie si le seuil maximal est atteint.
  * **Algorithme Token Bucket :** Approche optimisée en mémoire calculant le nombre de jetons disponibles à la volée en fonction du temps écoulé depuis le dernier accès, évitant l'historisation de chaque timestamp.
  * **Interface Abstraite :** Création d'une classe de base abstraite (via le module `abc`) nommée `RateLimiter` pour permettre l'interopérabilité et le changement dynamique de stratégie de limitation (*Strategy Pattern*).
* **Bonnes pratiques & Approche Pythonic :**
  * Gestion de la concurrence et de la thread-safety en protégeant les structures de données à l'aide de `threading.Lock` ou `asyncio.Lock` pour simuler des conditions de production réelles.

# Importer une base de données depuis un fichier `.sql`

Le fichier `bdd_exercice.sql` est un **dump** : un simple fichier texte qui contient
toutes les commandes SQL nécessaires pour recréer les tables et les données.

> ⚠️ Un dump ne crée **pas** la base de données : il remplit une base **déjà existante et vide**.
> La première étape est donc toujours la même : créer une base vide.

Deux méthodes sont possibles, au choix :

- [Méthode 1 — en ligne de commande avec `psql.exe`](#méthode-1--en-ligne-de-commande-psql)
- [Méthode 2 — avec l'interface graphique pgAdmin](#méthode-2--avec-pgadmin-interface-graphique)

---



## Méthode 1 — En ligne de commande (`psql`)

### Étape 1 : ouvrir une invite de commande

Touche `Windows` → taper `cmd` → `Entrée`.

### Étape 2 : se placer dans le dossier de PostgreSQL

```bat
cd "C:\Program Files\PostgreSQL\14\bin"
```

> Adaptez `14` au numéro de version installée sur la machine.

### Étape 3 : créer la base de données vide

```bat
createdb.exe -U postgres -h localhost -p 5432 bdd_rappels
```

Le mot de passe est demandé → taper `test` (rien ne s'affiche pendant la saisie, c'est normal) → `Entrée`.

### Étape 4 : importer le fichier `.sql` dans cette base

```bat
psql.exe -U postgres -h localhost -p 5432 -d bdd_rappels -f "C:\chemin\vers\bdd_exercice.sql"
```

Redonner le mot de passe `test`.

> 💡 **Astuce pour le chemin du fichier** : dans l'explorateur Windows, faites
> `Maj + clic droit` sur `bdd_exercice.sql` → *Copier en tant que chemin d'accès*,
> puis collez-le (avec les guillemets) après le `-f`.

### Que veulent dire les options ?

| Option | Signification |
|---|---|
| `-U postgres` | **U**ser : l'utilisateur de connexion |
| `-h localhost` | **H**ost : le serveur (ici votre propre machine) |
| `-p 5432` | **P**ort d'écoute de PostgreSQL (5432 par défaut) |
| `-d bdd_rappels` | **D**atabase : la base dans laquelle on importe |
| `-f "…\fichier.sql"` | **F**ile : le fichier SQL à exécuter |

### Résultat attendu

Des lignes défilent : `SET`, `CREATE TABLE`, `COPY 1234`, `ALTER TABLE`…
S'il n'y a pas de message `ERROR` à la fin, **l'import a réussi**.

### Étape 5 : vérifier

Ouvrir pgAdmin → `Servers` → `PostgreSQL 14` → `Databases` →
faire un clic droit sur `Databases` → **Refresh** → la base `bdd_rappels` apparaît
avec ses tables dans `Schemas > public > Tables`.

---

## Méthode 2 — Avec pgAdmin (interface graphique)

### Étape 1 : se connecter au serveur

Ouvrir **pgAdmin** → double-clic sur `PostgreSQL 14` dans le panneau de gauche →
saisir le mot de passe `test`.

### Étape 2 : créer la base de données vide

1. Clic droit sur **Databases** → `Create` → `Database…`
2. Dans **Database**, saisir : `bdd_rappels`
3. Cliquer sur **Save**.

### Étape 3 : ouvrir le fichier `.sql` dans l'éditeur de requêtes

1. **Clic gauche sur la base `bdd_rappels`** pour la sélectionner
   (⚠️ étape capitale : la requête s'exécute sur la base sélectionnée).
2. Menu `Tools` → `Query Tool` (ou l'icône ⚡).
3. Dans la fenêtre du Query Tool, cliquer sur l'icône **Open File** (📁)
   et choisir `bdd_exercice.sql`.

### Étape 4 : exécuter

Cliquer sur le bouton **▶ Execute/Refresh** (ou appuyer sur `F5`).

En bas de la fenêtre, l'onglet **Messages** affiche le déroulement.
Le message final `Query returned successfully` indique que **l'import a réussi**.

### Étape 5 : vérifier

Clic droit sur `bdd_rappels` → **Refresh**, puis dérouler
`Schemas` → `public` → `Tables` : les tables doivent être visibles.

> ⚠️ **Limite de cette méthode** : le Query Tool charge tout le fichier en mémoire.
> Pour un dump volumineux (> 50 Mo), pgAdmin peut ramer ou planter :
> dans ce cas, utilisez la **méthode 1** (`psql`), beaucoup plus rapide et robuste.

---


## À retenir

```
1. Créer une base VIDE
2. Y exécuter le fichier .sql
3. Rafraîchir (Refresh) pour voir les tables dans pgAdmin
```

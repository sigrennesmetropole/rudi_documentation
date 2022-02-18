# Documentation de rudi.bzh

Ce dépôt correspond au contenu du site [doc.rudi.bzh](https://doc.rudi.bzh), site consacré à la documentation de la plateforme [rudi.bzh](https://rudi.bzh).

## Contribuer

Les contributions sont les bienvenues. Elles peuvent prendre la forme de _pull requests_ effectuées [depuis un fork de ce dépôt](https://help.github.com/articles/fork-a-repo/), ou de discussions dans les [tickets GitHub du dépôt](https://github.com/sigrennesmetropole/rudi_documentation/issues).

Les pages sont rangées dans le dossier [articles](articles).
Elles sont organisées par catégorie.

### Ajouter une page

Le plus simple est de copier une page existante et d'adapter l'en-tête [Front Matter](https://jekyllrb.com/docs/front-matter/) (premières lignes entre `---`).

Exemple de page : [Authentification](articles/_api/authentification.md). Son en-tête se présente comme suit :

```yaml
order: 2 # La page est affichée en 2e position parmi les autres pages de la même catégorie dans le menu latéral
title: Authentification # Le titre de la page (dans le menu latéral, mais aussi dans le titre du navigateur)
```

### Ajouter une catégorie

Pour créer une nouvelle catégorie :

- Créer un nouveau-dossier "_macategorie" dans le dossier [articles](articles) ;
- Ajouter une nouvelle collection dans la [config Jekyll](_config.yml) sans le préfixe "_".

Exemple avec le dossier contenant des pages : [_api](articles/_api).

## Démarrage rapide

### Ruby natif

Installer Jekyll en suivant la documentation adaptée à votre système : https://jekyllrb.com/docs/installation/.

Cloner le projet et se positionner dans son dossier :

```
git clone https://github.com/sigrennesmetropole/rudi_documentation
cd rudi_documentation
```

Pour tester localement avec une version de Ruby supérieure à la 2.3, supprimer le fichier Gemfile.lock.

Compiler et démarrer le serveur Jekyll :

```
bundle install
bundle exec jekyll serve --watch
```

La documentation apparaîtra alors à l’adresse suivante : <a href="http://localhost:4000">http://localhost:4000</a>.

### Docker Compose

Vous pouvez utiliser [docker-compose](https://docs.docker.com/compose/) pour tester vos modifications localement avant de les soumettre. Pour ce faire, il suffit simplement d’exécuter la commande :

```
docker-compose up
```

Ceci va télécharger l'image docker de [Jekyll](https://www.jekyll.io/) et installer les dépendances nécessaires avant d'effectuer le rendu du site. Le résultat sera disponible sur http://localhost:4000. Les modifications sont automatiquement prises en compte et provoquent un nouveau rendu du site.

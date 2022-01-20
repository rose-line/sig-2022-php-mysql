# Introduction

## PHP

- **PHP** = PHP: Hypertext Processor
- http://php.net
- Conçu comme un langage de « résolution de problèmes » (dixit son auteur)
- Relativement facile à apprendre
- Parts de marché sur le web : ~40 %
- « Top ten » des langages les plus utilisés depuis 1999 (index tiobe.com)
- Tourne sur pratiquement toutes les architectures standards

## Installation

- http://php.net/downloads.php
- http://windows.php.net (pour les _binaries_ Windows)
- Mais on peut pour débuter utiliser un « pack _All-in-one_ » qui contient tout ce dont on a besoin pour développer une application web avec PHP :
  - www.apachefriends.org : **XAMPP** (**A**pache + **M**ySQL + **P**HP + **P**erl, le **X** signifiant qu'il existe une version pour chaque système populaire -- Windows, Mac...)
  - Apache : le serveur web
  - MySQL : SGBD (Système de Gestion de Bases de Données)
  - PHP : le langage lui-même
  - (Perl : un autre langage de programmation)
- Une fois XAMPP installé, on lance XAMPP Control Panel et on démarre le service Apache.
- On peut alors tester que tout fonctionne bien en ouvrant un navigateur et en allant à l'adresse `http://localhost` : la page XAMPP devrait alors s'afficher.

## Fonctionnement

- Code PHP est :
  - soit inclut à l'intérieur de la page HTML : le code PHP inclus produit alors du code HTML qui est inséré exactement à l'endroit où était le code PHP ;
  - soit dans son propre fichier .php, pour tout le code qui n'est pas purement de présentation (accès à une base de données, code de traitement de ces données, logique « métier », etc.). Une application de grande taille contient plusieurs modules, chacun composé de plusieurs fichiers PHP.
- Délimiteurs de code PHP : `<?php ... ?>`
- La commande `echo` est celle qui permet de produire le texte de sortie (le code HTML à produire).
- Le code PHP peut être écrit dans n'importe quel éditeur de texte, mais on utilisera de préférence un outil ayant au moins la capacité d'éditer le code de manière plus confortable (couleurs qui permettent de différencier les parties du code, formatage, etc.).

## Hello World

- Répertoire `htdocs` du répertoire de XAMPP = répertoire de base du serveur web.
- On y crée un nouveau répertoire pour notre premier projet, par exemple : `mon-projet-php`
- On accédera au contenu de ce répertoire sur le serveur en allant à l'adresse http://localhost/mon-projet-php
- Sous l'éditeur, on ouvre le répertoire `mon-projet-php` et on y crée un nouveau fichier : `bonjour.php`

```php
<!-- bonjour.php -->
<!DOCTYPE  html>
<html lang="fr">
<head>
  <meta  charset="UTF-8">
  <meta  name="viewport"  content="width=device-width, initial-scale=1.0">
  <meta  http-equiv="X-UA-Compatible"  content="ie=edge">
  <title>PHP !</title>
</head>
<body>
  <p>Bonjour !</p>
</body>
</html>
```

- Vérifiez que tout va bien en affichant la page http://localhost/projet-php/bonjour.php dans le navigateur.
- Entre les balises du paragraphe, on va remplacer le texte par du code PHP qui va générer le même texte grâce à la commande `echo` :

```php
<body>
  <p><?php
        echo "Bonjour !";
      ?>
  </p>
</body>
```

- Note : toute instruction PHP doit se terminer par un point-virgule.
- Ce code PHP est lancé sur le serveur, il est remplacé par la sortie produite (la chaîne de caractères `"Bonjour !"`) et renvoyé au navigateur, qui affiche le rendu.
- On peut afficher le code source de la page reçue par le navigateur pour vérifier que toute trace du code PHP a disparu.

## `phpinfo`

- Créez un nouveau fichier `phpinfo.php`
- Cette fois, plus de HTML du tout, juste un appel de fonction :

```php
<?php
  phpinfo();
?>
```

- Testez : http://localhost/projet-php/phpinfo.php
- L'appel de la fonction a généré un code HTML considérable avec tout un tas d'informations sur le système, la version de PHP et les extensions installées, la configuration du serveur web Apache, etc.

## Documentation

- http://php.net/manual
- http://php.net/nom_de_fonction pour accéder rapidement à la doc de `nom_de_fonction`
- On peut sélectionner la langue.
- On peut télécharger toute la doc (_offline_).

# PHP et les _forms_

1. Intro sur les _forms_
2. Traitement des données des _forms_
3. Validation et pré-remplissage d'une _form_

## HTML `form`

```html
<form method="post" action=""></form>
```

### L'attribut `method`

- De nombreux verbes définis, mais les 2 plus importants sont :
  - Méthode **GET** (par défaut) :
    - les données de la _form_ sont ajoutées à l'URL ;
    - taille limitée (~500-2000 caractères), donc à n'utiliser que pour des choses simples ;
    - accès aux données en PHP par le tableau `$_GET`
  - Méthode **POST** :
    - les données sont transmises par la requête HTTP ;
    - pas de limite de taille, upload de fichiers possible ;
    - accès aux données en PHP par le tableau `$_POST`

### Les éléments d'une `form`

- Champs texte
  - Texte : `<input type="text" name="txt">`
  - Mot de passe : `<input type="password" name="pass">`
  - Texte multi-ligne : `<textarea name="area"></textarea>`
- _Checkboxes_ et boutons radio
  - _Checkbox_ : `<input type="checkbox" name="cb" value="on">`
  - boutons radio : `input type="radio" name="mongroupe" value="option1">option 1` (mettre tous les boutons dans le même groupe)
- Listes :
  - _Dropdown_ : `<select name="ddl"> <option value="choix1">Choix 1</option>...</select>`
  - Liste à choix multiple : `<select name="lcm[]" multiple size="5"> <option value="choix1">Choix 1</option>...</select>`

```html
<!-- entree.php - HTML uniquement -->
<!DOCTYPE html>
<html>
  <head>
    <title>PHP</title>
  </head>
  <body>
    <form method="post" action="">
      Nom : <input type="text" name="nom" /><br />
      Mot de passe: <input type="password" name="mdp" /><br />
      Sexe :
      <input type="radio" name="sexe" value="f" />femme
      <input type="radio" name="sexe" value="m" />homme<br />
      Couleur favorite:
      <select name="couleur">
        <option value="#f00">rouge</option>
        <option value="#0f0">vert</option>
        <option value="#00f">bleu</option></select
      ><br />
      Langues:
      <select name="langues[]" multiple size="3">
        <option value="fr">Français</option>
        <option value="en">Anglais</option>
        <option value="it">Italien</option></select
      ><br />
      Commentaires: <textarea name="comments"></textarea><br />
      <input type="checkbox" name="termes" value="ok" />J'accepte les termes<br />
      <input type="submit" name="submit" value="Envoyer" />
    </form>
  </body>
</html>
```

### Récupération des valeurs et affichage en PHP

- l'attribut `action` de la _form_ est vide, donc la _form_ va être POSTée au même fichier (`entrees.php`).
- Il nous faut donc inclure dans ce fichier le code PHP qui va traiter les informations éventuellement reçue au moment du POST.
- `isset(machin)` permet de savoir si `machin` a bien une valeur ; ici on vérifie si oui ou non le bouton a été pressé : si non, le code du `if` n'est pas exécuté et on se contente d'afficher le HTML qui est dessous ; si oui, alors ça veut dire que l'utilisateur a entré des informations et qu'on peut les afficher.
- `printf('Nom : %s, Couleur : %s', $nom, $couleur);` `printf` est comme `echo` (ce qu'elle produit va être affiché sur la page), mais elle est pratique car on peut écrire toute la chaîne d'un coup en remplaçant les chaînes dynamiques par des `%s` ; les valeurs sont fournies après, séparées par des virgules, et injectée dans la chaîne, dans l'ordre, à la place des `%s`.
- La fonction `htmlspecialchars()` s'assure que l'on injecte pas de code HTML malicieux dans notre sortie HTML (_XSS -- Cross-Site Scripting_)

```php
<!-- entree_affiche.php - Affichage avec PHP des valeurs entrées -->
<!DOCTYPE html>
<html>
<head>
    <title>PHP</title>
</head>
<body>
<?php
  if (isset($_POST['submit'])) {
      printf('Nom utilisateur: %s
      <br>Mot de passe: %s
      <br>Sexe: %s
      <br>Couleur: %s
      <br>Langue(s): %s
      <br>Commentaires: %s
      <br>Termes: %s',
          htmlspecialchars($_POST['nom']),
          htmlspecialchars($_POST['mdp']),
          htmlspecialchars($_POST['sexe']),
          htmlspecialchars($_POST['couleur']),
          htmlspecialchars(implode(' ', $_POST['langues'])),
          htmlspecialchars($_POST['comments']),
          htmlspecialchars($_POST['termes']));
  }
?>
<form method="post" action="">
    Nom : <input type="text" name="nom"><br>
    Mot de passe: <input type="password" name="mdp"><br>
    Sexe :
        <input type="radio" name="sexe" value="f">femme
        <input type="radio" name="sexe" value="m">homme<br>
    Couleur favorite:
        <select name="couleur">
            <option value="#f00">rouge</option>
            <option value="#0f0">vert</option>
            <option value="#00f">bleu</option>
        </select><br>
    Langues:
        <select name="langues[]" multiple size="3">
            <option value="fr">Français</option>
            <option value="en">Anglais</option>
            <option value="it">Italien</option>
        </select><br>
    Commentaires: <textarea name="comments"></textarea><br>
    <input type="checkbox" name="termes" value="ok">J'accepte les termes<br>
    <input type="submit" name="submit" value="Envoyer">
</form>
</body>
</html>
```

### Validation partielle des entrées

- Il faut maintenant faire de la validation :
- vérifier que tous les champs texte requis ont été remplis ;
- qu'un bouton radio a bien été sélectionné (sinon l'accès `$_POST['sexe']` ne fonctionnera pas) ;
- que la _checkbox_ a été cochée (ici ce serait obligatoire mais même si ça ne l'est pas, en l'état, `$_POST['termes']` ne fonctionnera pas) ;
- on pourrait ajouter une entrée "_Veuillez choisir_" dans la liste déroulante et il faudra alors vérifier que l'utilisateur n'a pas laissé ça comme sélection.
- JavaScript est un autre langage qui permet (notamment) de faire de la validation côté client (navigateur) sans rien renvoyer au serveur (_feedback_ immédiat, meilleure expérience utilisateur) mais de toute façon tout ce qui arrive au serveur devra être revalidé en PHP.

```php
<!-- entree_valide.php - Validation sommaire des entrées -->
<!DOCTYPE html>
<html>
<head>
    <title>PHP</title>
</head>
<body>
<?php
  if (isset($_POST['submit'])) {
      $ok = true;
      // === teste égalité ET vérifie
      // que les deux variables ont le même type
      if (!isset($_POST['nom']) || $_POST['nom'] === '') {
          $ok = false;
      }
      if (!isset($_POST['mdp']) || $_POST['mdp'] === '') {
          $ok = false;
      }
      if (!isset($_POST['comments']) || $_POST['comments'] === '') {
          $ok = false;
      }
      if (!isset($_POST['sexe']) || $_POST['sexe'] === '') {
          $ok = false;
      }
      if (!isset($_POST['termes']) || $_POST['termes'] === '') {
          $ok = false;
      }
      if (!isset($_POST['couleur']) || $_POST['couleur'] === '') {
          $ok = false;
      }
      if (!isset($_POST['langues'])
              ||  !is_array($_POST['langues'])
              || count($_POST['langues']) === 0) {
          $ok = false;
      }
      if ($ok) {
          printf('Nom utilisateur: %s
          <br>Mot de passe: %s
          <br>Sexe: %s
          <br>Couleur: %s
          <br>Langue(s): %s
          <br>Commentaires: %s
          <br>Termes: %s',
              htmlspecialchars($_POST['nom']),
              htmlspecialchars($_POST['mdp']),
              htmlspecialchars($_POST['sexe']),
              htmlspecialchars($_POST['couleur']),
              htmlspecialchars(implode(' ', $_POST['langues'])),
              htmlspecialchars($_POST['comments']),
              htmlspecialchars($_POST['termes']));
      }
  }
?>
<form method="post" action="">
    Nom : <input type="text" name="nom"><br>
    Mot de passe: <input type="password" name="mdp"><br>
    Sexe :
        <input type="radio" name="sexe" value="f">femme
        <input type="radio" name="sexe" value="m">homme<br>
    Couleur favorite:
        <select name="couleur">
            <option value="">Veuillez choisir</option>
            <option value="#f00">rouge</option>
            <option value="#0f0">vert</option>
            <option value="#00f">bleu</option>
        </select><br>
    Langues:
        <select name="langues[]" multiple size="3">
            <option value="fr">Français</option>
            <option value="en">Anglais</option>
            <option value="it">Italien</option>
        </select><br>
    Commentaires: <textarea name="comments"></textarea><br>
    <input type="checkbox" name="termes" value="ok">J'accepte les termes<br>
    <input type="submit" name="submit" value="Envoyer">
</form>
</body>
</html>
```

### Réinjecter les entrées avec PHP

-Maintenant faisons en sorte que, en cas d'erreur/oubli, on n'ait pas à entrer de nouveau toutes les données.

- On récupère toutes les données dans des variables individuelles : `$nom`, `$mdp`...
- On les initialise à `''` (rien), et on va les réaffecter si on a des valeurs effectives pour pouvoir ensuite les réinjecter dans le code HTML, afin que l'utilisateur n'ait plus à les réécrire.

```php
<!-- entree_reinjecte.php - Réinjection des entrées valides si erreur -->
<!DOCTYPE html>
<html>
<head>
    <title>PHP</title>
</head>
<body>
<?php
  $nom = '';
  $mdp = '';
  $comments = '';
  $sexe = '';
  $termes = '';
  $couleur = '';
  $langues = array();

  if (isset($_POST['submit'])) {
    $ok = true;
    if (!isset($_POST['nom']) || $_POST['nom'] === '') {
        $ok = false;
    } else {
        $nom = $_POST['nom'];
    }
    if (!isset($_POST['mdp']) || $_POST['mdp'] === '') {
        $ok = false;
    } else {
        $mdp = $_POST['mdp'];
    }
    if (!isset($_POST['comments']) || $_POST['comments'] === '') {
        $ok = false;
    } else {
        $comments = $_POST['comments'];
    }
    if (!isset($_POST['sexe']) || $_POST['sexe'] === '') {
        $ok = false;
    } else {
        $sexe = $_POST['sexe'];
    }
    if (!isset($_POST['termes']) || $_POST['termes'] === '') {
        $ok = false;
    } else {
        $termes = $_POST['termes'];
    }
    if (!isset($_POST['couleur']) || $_POST['couleur'] === '') {
        $ok = false;
    } else {
        $couleur = $_POST['couleur'];
    }
    if (!isset($_POST['langues']) || !is_array($_POST['langues'])
          || count($_POST['langues']) === 0) {
        $ok = false;
    } else {
        $langues = $_POST['langues'];
    }

    if ($ok) {
      printf('Nom utilisateur : %s
      <br>Mot de passe : %s
      <br>Sexe : %s
      <br>Couleur : %s
      <br>Langue(s) : %s
      <br>Commentaires : %s
      <br>Termes : %s',
          htmlspecialchars($nom),
          htmlspecialchars($mdp),
          htmlspecialchars($sexe),
          htmlspecialchars($couleur),
          htmlspecialchars(implode(' ', $langues)),
          htmlspecialchars($comments),
          htmlspecialchars($termes));
    }
  }
?>
<form method="post" action="">
   Nom : <input type="text" name="nom" value="<?php
        echo htmlspecialchars($nom);
    ?>"><br>
    Mot de passe : <input type="password" name="mdp" value="<?php
        echo htmlspecialchars($mdp);
    ?>"><br>
    Sexe :
        <input type="radio" name="sexe" value="f"<?php
            if ($sexe === 'f') {
                echo ' checked';
            }
        ?>>femme
        <input type="radio" name="sexe" value="m"<?php
            if ($sexe === 'm') {
                echo ' checked';
            }
        ?>>homme<br>
    Couleur favorite :
        <select name="couleur">
            <option value="">Veuillez choisir</option>
            <option value="#f00"<?php
                if ($couleur === '#f00') {
                    echo ' selected';
                }
            ?>>rouge</option>
            <option value="#0f0"<?php
                if ($couleur === '#0f0') {
                    echo ' selected';
                }
            ?>>vert</option>
            <option value="#00f"<?php
                if ($couleur === '#00f') {
                    echo ' selected';
                }
            ?>>bleu</option>
        </select><br>
    Langues :
        <select name="langues[]" multiple size="3">
            <option value="fr"<?php
                if (in_array('fr', $langues)) {
                    echo ' selected';
                }
            ?>>Français</option>
            <option value="en"<?php
                if (in_array('en', $langues)) {
                    echo ' selected';
                }
            ?>>Anglais</option>
            <option value="it"<?php
                if (in_array('it', $langues)) {
                    echo ' selected';
                }
            ?>>Italien</option>
        </select><br>
    Commentaires: <textarea name="comments"><?php
        echo htmlspecialchars($comments);
        ?></textarea><br>
    <input type="checkbox" name="termes" value="ok"<?php
      if ($termes === 'ok') {
        echo ' checked';
      }
    ?>>J'accepte les termes<br>
    <input type="submit" name="submit" value="Envoyer">
</form>
</body>
</html>
```

# Inclure une base de données

On va voir :

- la configuration de la base de données avec phpMyAdmin ;
- l'insertion de données ;
- la lecture de données ;
- la suppression de données.

Pour pouvoir utiliser la BDD, il faut d'abord lancer le serveur MySQL depuis _XAMPP Control Panel_.

## phpMyAdmin

- XAMPP inclut phpMyAdmin, pré-configuré.
- http://localhost/phpmyadmin
- C'est une simple page web depuis laquelle on va pouvoir configurer le serveur MySQL.
- XAMPP configure MySQL et phpMyAdmin pour que la connexion par défaut soit en utilisateur _root_ (administrateur, tous les droits) et sans mot de passe. Ceci n'est absolument pas sécurisé et devrait être changé sur une installation en production.
- MySQL peut gérer plusieurs bases de données, et une base de données peut contenir plusieurs tables (relations).
- Créer une base de données (onglet _Databases_), nommée "sig".
- Créer une table "utilisateurs" de quatre colonnes (quatre attributs ; nous ne mémoriserons qu'une partie des champs rencontrés dans les sections précédentes).
  - **id** (clé primaire) ; type INT ; A*I (\_auto-increment*) ; PRIMARY KEY
  - **nom** ; type VARCHAR(20)
  - **sexe** ; type CHAR(1)
  - **couleur** ; type VARCHAR(10)
- Cliquez sur _save_
- On peut utilisez phpMyAdmin pour créer un jeu de données, parcourir et interroger les tables, les modifier, exactement comme on le ferait avec des requêtes SQL. Mais on va tout de suite repasser en PHP pour manipuler notre table.

## Avertissement

- Il existe plusieurs façon de se connecter à une base de données depuis PHP, on fera ça « façon fonctions ».
- On ne procédera qu'à une gestion/détection des erreurs très limitée, ce n'est pas l'objet de ce cours.
- On conservera les accès non sécurisées à MySQL dont on a déjà parlé.
- On ne s'intéressera ici quasiment pas au design de l'appli. On ne se concentrera pas sur le « joli » mais sur le « fonctionnel ».

## Connexion à la BDD

-Connexion à la base de données : fonction `mysqli_connect()`, qui retourne une ressource : un « pointeur » sur la BDD ouverte qu'on va stocker dans une variable pour pouvoir ensuite agir sur la BDD.

```php
// Connexion BDD
$db = mysqli_connect("localhost", "nomutilisateur", "pass", "nomdatabase");

// ... Utilisation BDD ...

// fermeture BDD
mysqli_close($db);
```

## Insertion de données

- La fonction `mysqli_query()` permet d'envoyer des requêtes SQL à la BDD.
- `sprintf()` fonctionne comme `printf()` mais retourne la string produite plutôt que de l'afficher.

```php
mysqli_query($db,
  "INSERT INTO table (col1, col2)
   VALUES ('val1', 'val2')");

// Version qui évite les attaques de type "Injection SQL"
// On prépare la requête avant en supprimant les caractères
// potentiellement dangereux
$sql = sprintf(
  "INSERT INTO table (col1, col2)
   VALUES ('%s', '%s')",
   mysqli_real_escape_string($db, 'val1'),
   mysqli_real_escape_string($db, 'val2'));
mysqli_query($db, $sql);
```

- Voici le code mis à jour pour l'insertion d'utilisateurs dans la BDD. Le code qui affichait les données entrées est remplacé par le code d'accès/insertion BDD.

```php
<!-- entree_bdd.php - Enregistrement en BDD -->
<!DOCTYPE html>
<html>
<head>
  <title>PHP</title>
</head>
<body>
  <?php
$nom = '';
$sexe = '';
$couleur = '';

if (isset($_POST['submit'])) {
  $ok = true;
  if (!isset($_POST['nom']) || $_POST['nom'] === '') {
    $ok = false;
  } else {
    $nom = $_POST['nom'];
  }
  if (!isset($_POST['sexe']) || $_POST['sexe'] === '') {
    $ok = false;
  } else {
    $sexe = $_POST['sexe'];
  }
  if (!isset($_POST['couleur']) || $_POST['couleur'] === '') {
    $ok = false;
  } else {
    $couleur = $_POST['couleur'];
  }

  if ($ok) {
    $db = mysqli_connect('localhost', 'root', '', 'sig');
    $sql = sprintf("INSERT INTO utilisateurs (nom, sexe, couleur) VALUES (
        '%s', '%s', '%s')",
      mysqli_real_escape_string($db, $nom),
      mysqli_real_escape_string($db, $sexe),
      mysqli_real_escape_string($db, $couleur));
    mysqli_query($db, $sql);
    mysqli_close($db);
    echo '<p>Utilisateur ajouté !</p>';
  }
}
?>
  <form method="post" action="">
    Nom : <input type="text" name="nom" value="<?php echo htmlspecialchars($nom); ?>"><br>
    Sexe :
    <input type="radio" name="sexe" value="f" <?php if ($sexe === 'f') {echo ' checked';}?>>femme
    <input type="radio" name="sexe" value="m" <?php if ($sexe === 'm') {echo ' checked';}?>>homme<br>
    Couleur favorite :
    <select name="couleur">
      <option value="">Veuillez choisir</option>
      <option value="#f00" <?php if ($couleur === '#f00') {echo ' selected';}?>>rouge</option>
      <option value="#0f0" <?php if ($couleur === '#0f0') {echo ' selected';}?>>vert</option>
      <option value="#00f" <?php if ($couleur === '#00f') {echo ' selected';}?>>bleu</option>
    </select><br>
    <input type="submit" name="submit" value="Envoyer">
  </form>
</body>
</html>
```

## Lire des données

- On utilise de nouveau `msqli_connect()` pour se connecter, puis `mysqli_query()` pour exécuter une requête d'interrogation :

```php
$res = mysqli_query($db, "SELECT * FROM table");
```

- Ensuite `$res` contient tous les résultats. On peut alors itérer (parcourir une à une) sur les lignes du résultat :

```php
foreach ($res as $row) {
  $val1 = $row["col1"];
  $val2 = $row["col2"];
}
```

- Voici un fichier `select.php` qui récupère touts les enregistrements de la table `utilisateurs` et les affiche sous forme de liste HTML, l'attribut couleur étant utilisé pour « styler » l'affichage de chaque enregistrement :

```php
<!-- select.php - affiche des données provenant de la BDD -->
<!DOCTYPE>
<html>
<head>
    <title>PHP</title>
</head>
<body>
<ul>
    <?php
    $db = mysqli_connect('localhost', 'root', '', 'sig');
    $sql = 'SELECT * FROM utilisateurs';
    $result = mysqli_query($db, $sql);

    foreach ($result as $ligne) {
        printf('<li><span style="color: %s;">%s (%s)</span></li>',
          htmlspecialchars($ligne['couleur']),
          htmlspecialchars($ligne['nom']),
          htmlspecialchars($ligne['sexe'])
        );
    }
    mysqli_close($db);
    ?>
</ul>
</body>
</html>
```

## Modifier des données

- On va maintenant utiliser la commande SQL `UPDATE` pour modifier des données en base.
- On utilise à nouveau `mysqli_real_escape_string` pour « filtrer » les caractères dangereux et éviter de potentielles attaques de type injection SQL.

```php
$sql = sprintf(
    "UPDATE table SET col1='%s', col2='%s'
     WHERE col3='%s'",
     mysqli_real_escape_string($db, 'value1'),
     mysqli_real_escape_string($db, 'value2'),
     mysqli_real_escape_string($db, 'value3'));
mysqli_query($db, $sql);
```

- On copie le fichier entree.php vers modifier.php. On va effectuer les changements qui vont permettre à partir de cette page de non plus créer un utilisateur, mais modifier un utilisateur existant.

```php
<!-- modifier.php - met à jour (UPDATE) les données en BDD -->
<?php
// Récupère l'id de l'utilisateur depuis l'URL.
// Si pas d'id correct, redirige vers 'select.php'
// (ce code doit être en début de fichier)
if (isset($_GET['id']) && ctype_digit($_GET['id'])) {
  $id = $_GET['id'];
} else {
  header('Location: select.php');
}
?>
<!DOCTYPE html>
<html>
<head>
  <title>PHP</title>
</head>
<body>
  <?php
$nom = '';
$sexe = '';
$couleur = '';

if (isset($_POST['submit'])) {
  $ok = true;
  if (!isset($_POST['nom']) || $_POST['nom'] === '') {
    $ok = false;
  } else {
    $nom = $_POST['nom'];
  }
  if (!isset($_POST['sexe']) || $_POST['sexe'] === '') {
    $ok = false;
  } else {
    $sexe = $_POST['sexe'];
  }
  if (!isset($_POST['couleur']) || $_POST['couleur'] === '') {
    $ok = false;
  } else {
    $couleur = $_POST['couleur'];
  }

  if ($ok) {
    $db = mysqli_connect('localhost', 'root', '', 'sig');
    $sql = sprintf("UPDATE utilisateurs
      SET nom='%s', sexe='%s', couleur='%s'
      WHERE id=%s",
      mysqli_real_escape_string($db, $nom),
      mysqli_real_escape_string($db, $sexe),
      mysqli_real_escape_string($db, $couleur),
      $id);
    mysqli_query($db, $sql);
    mysqli_close($db);
    echo '<p>Utilisateur mis à jour !</p>';
  }
} else {
  $db = mysqli_connect('localhost', 'root', '', 'sig');
  $sql = sprintf('SELECT * FROM utilisateurs WHERE id=%s', $id);
  $result = mysqli_query($db, $sql);
  foreach ($result as $ligne) {
    $nom = $ligne['nom'];
    $sexe = $ligne['sexe'];
    $couleur = $ligne['couleur'];
  }
  mysqli_close($db);
}
?>
  <form method="post" action="">
    Nom : <input type="text" name="nom" value="<?php echo htmlspecialchars($nom); ?>"><br>
    Sexe :
    <input type="radio" name="sexe" value="f" <?php if ($sexe === 'f') {echo ' checked';}?>>femme
    <input type="radio" name="sexe" value="m" <?php if ($sexe === 'm') {echo ' checked';}?>>homme<br>
    Couleur favorite :
    <select name="couleur">
      <option value="">Veuillez choisir</option>
      <option value="#f00" <?php if ($couleur === '#f00') {echo ' selected';}?>>rouge</option>
      <option value="#0f0" <?php if ($couleur === '#0f0') {echo ' selected';}?>>vert</option>
      <option value="#00f" <?php if ($couleur === '#00f') {echo ' selected';}?>>bleu</option>
    </select><br>
    <input type="submit" name="submit" value="Envoyer">
  </form>
</body>
</html>
```

- On modifie le fichier select.php (maintenant select_edit.php) pour ajouter des liens (balises `<a></a>` en HTML) permettant d'afficher la page d'édition chaque utilisateur.

```php
<!-- select_edit.php - version avec liens pour édition -->
<!DOCTYPE>
<html>
<head>
    <title>PHP</title>
</head>
<body>
<ul>
    <?php
    $db = mysqli_connect('localhost', 'root', '', 'sig');
    $sql = 'SELECT * FROM utilisateurs';
    $result = mysqli_query($db, $sql);

    foreach ($result as $ligne) {
        printf('<li><span style="color: %s;">%s (%s)</span>
        <a href="modifier.php?id=%s">modifier</a></li>',
          htmlspecialchars($ligne['couleur']),
          htmlspecialchars($ligne['nom']),
          htmlspecialchars($ligne['sexe']),
          htmlspecialchars($ligne['id'])
        );
    }
    mysqli_close($db);
    ?>
</ul>
</body>
</html>
```

### Supprimer des données

- Requête SQL `DELETE FROM`

```php
$sql = sprintf("DELETE FROM table WHERE col1='%s'",
         mysqli_real_escape_string($db, 'value1'));
mysqli_query($db, $sql);
```

- Dans une « vraie » application, l'implémentation de la suppression serait plus verbeuse :
  - il faut s'assurer que l'utilisateur veut bien supprimer (« Êtes-vous sûr ? ») ;
  - est-il autorisé à supprimer cette information ?
  - est-ce qu'un _hacker_ pourrait déclencher une suppression de l'extérieur ?
- Ici on va se contenter de lancer la requête immédiatement.

```php
<!-- supprimer.php - supprime un utilisateur dont l'id est donné par l'url -->
<?php
// Récupère l'id de l'utilisateur depuis l'URL.
// Si pas d'id correct, redirige vers 'select.php'
// (ce code doit être en début de fichier)
if (isset($_GET['id']) && ctype_digit($_GET['id'])) {
  $id = $_GET['id'];
} else {
  header('Location: select.php');
}
?>
<!DOCTYPE html>
<html>
<head>
  <title>PHP</title>
</head>
<body>
  <?php
    $db = mysqli_connect('localhost', 'root', '', 'sig');
    $sql = "DELETE FROM utilisateurs WHERE id=$id";
    msqli_query($db, $sql);
    echo '<p>Utilisateur supprimé !</p>';
    msqli_close($db);
  ?>
</body>
</html>
```

- Voici maintenant la nouvelle version de la page select qui inclut un lien pour supprimer chaque utilisateur, exactement comme pour la modification :

```php
<!-- select_edit_supp.php - version avec liens pour édition -->
<!DOCTYPE>
<html>
<head>
    <title>PHP</title>
</head>
<body>
<ul>
    <?php
    $db = mysqli_connect('localhost', 'root', '', 'sig');
    $sql = 'SELECT * FROM utilisateurs';
    $result = mysqli_query($db, $sql);

    foreach ($result as $ligne) {
        printf('<li><span style="color: %s;">%s (%s)</span>
        <a href="modifier.php?id=%s">modifier</a>
        <a href="supprimer.php?id=%s">supprimer</a></li>',
          htmlspecialchars($ligne['couleur']),
          htmlspecialchars($ligne['nom']),
          htmlspecialchars($ligne['sexe']),
          htmlspecialchars($ligne['id']),
          htmlspecialchars($ligne['id'])
        );
    }
    mysqli_close($db);
    ?>
</ul>
</body>
</html>
```

# Modularisation

- En général, c'est une bonne idée de modulariser (diviser en plusieurs parties) le code et le _markup_ (HTML) :
  - pour leur donner des noms ;
  - pour organiser ;
  - pour pouvoir réutiliser.
- En PHP, on peut « inclure » un fichier PHP dans un autre fichier.
- On utilise pour cela une des quatre instructions disponibles.
- Le contenu du fichier inclus est « collé » au moment de l'exécution à l'endroit où était l'instruction.
- Les quatre instructions sont :
  - `include` : le fichier est inclus et exécuté à chaque fois que cette instruction est rencontrée ; si le fichier n'existe pas => simple avertissement ;
  - `include_once` : le fichier ne sera exécuté qu'à la première apparition de l'instruction ; si le fichier n'existe pas => simple avertissement ;
  - `require` : pareil que `include`, mais cette fois si le fichier est introuvable => erreur, le script s'arrête ;
  - `require_once` : pareil que `include_once`, et erreur si fichier introuvable.

```php
// ce fichier a besoin de bonjour.php
<?php
  require 'bonjour.php'; // le contenu est exécuté à chaque passage
  echo ' tout le monde';
?>

// fichier bonjour.php
<?php
  echo 'Bonjour';
?>

// Voici l'inclusion que fait PHP avec ces deux fichiers
<?php
?><?php
  echo 'Bonjour';
?><?php
  echo ' tout le monde';
?>
```

- Pour charger d'autres fichiers (non PHP), il existe d'autres fonctions, notamment `readfile()`
- Par exemple, on pourrait avoir besoin d'une barre de navigation commune à toutes les pages du site.
- Ajoutons un fichier `navigation.tmpl.html` à notre projet (`tmpl` pour « _template_ », c'est juste une convention de nommage).

```html
<!-- navigation.tmpl.html -->
<nav>
  <ul>
    <li>
      <a href="select_edit_supp.php">Liste</a>
      <a href="entree_bdd.php">Ajouter</a>
    </li>
  </ul>
</nav>
```

- On peut ajouter ceci au fichier .css de styles de l'application, pour que l'affichage de la barre se face en ligne :

```css
nav > ul {
  list-style-type: none;
}
nav > ul > li {
  display: inline;
}
```

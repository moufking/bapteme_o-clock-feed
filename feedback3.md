# feedback:

> En générale le modèle mvc a été respecté (Model, Vue et controller)


### #0 Le projet ne demarre par correctement 
- Dans le dossier public, il manque le fichier `.htaccess`
    
    
       
- Le fichier `.htaccess` est un fichier de configuration utilisé par les serveurs web Apache pour permettre la personnalisation 
      des paramètres de configuration pour un répertoire particulier ou un ensemble de répertoires
       
```
RewriteCond %{REQUEST_URI}::$1 ^(/.+)/(.*)::\2$
RewriteRule ^(.*) - [E=BASE_URI:%1]

RewriteCond %{REQUEST_FILENAME} !-d

RewriteCond %{REQUEST_FILENAME} !-f

RewriteRule ^(.*)$ index.php?page=$1 [QSA,L]
```
    

### #1 Mettre en place les permissions:

- Aucune vérification n'est effectuée dans la liste des menus pour afficher uniquement le menu de connexion si l'utilisateur 
  n'est pas encore connecté, et afficher les autres menus si l'utilisateur est connecté.
  
Donc il faudrait faire une page de connexion, et récupérer le mail et le mot de passe , ensuite faire une requete comme ceci : 
  
Ex
```
$pdoStatement = $pdo->prepare('SELECT * FROM app_user WHERE email = :email AND  password = :password');
```

Cette requête permettra de vérifier si un utilisateur correspondant aux informations entrées par l'utilisateur existe dans la base de données."

### #2 La page principale (`/`) ne marche pas 
- Dans la page d'accueil du site qui  (`/home`) et non (`/`) au niveau des routes vous avez ajouté ceci

```
$router->map(
    'GET',
    '/home',
    [
        'method' => 'home',
        'controller' => MainController::class // On indique le FQCN de la classe
    ],
    'main-home'
);
```

Vous deviez ajouter ceci 

```
$router->map(
    'GET',
    '/',
    [
        'method' => 'home',
        'controller' => MainController::class // On indique le FQCN de la classe
    ],
    'main-home'
);

```

Avec ce bout de code on spécifie qu'au démarrage du site on pouvoir exécuter la fonction home qui est dans le fichier MainController .
Cette fonction permet d'affiche la page d'accueil du site.

### #3 Erreur concernant l'affichage des routes 
- Dans le fichier `home.tpl.php` , il y'a une erreur concernant les liens 'Utilisateurs', 'Se deconnecter' et 'accueil'
    
  -  Voici votre code 
    ```
                    <li class="nav-item">
                        <a class="nav-link" href="./index.html">Accueil</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="./appuser/list.html">Utilisateurs</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="/logout">Se déconnecter</a>
                    </li>
    ```
  Le code ci-dessus ne marche pas il faudrait mettre le nom exact de la route qui avait été renseigner dans le fichier de routes(index.php)
Voici un exemple : 
   ```
                    <li class="nav-item">
                        <a class="nav-link" href="<?= $router->generate('main-home') ?>">Accueil</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="<?= $router->generate('teacher_list') ?>">Etudiants</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="<?= $router->generate('user_list') ?>">Utilisateurs</a>
                    </li>
    ```
  
### #4 TP non terminé 
   - - Il faudrait établir une connexion afin de récupérer les informations de l'utilisateur dans une session,
       pour déterminer si l'utilisateur a le droit de voir certains menus en fonction de son rôle (administrateur ou utilisateur).

 - Le travail n'est pas totalement fini car il manque diverses fonctionnalités notamment :
   - La connexion
   - L'ajout, la suppression et la modification d'un professeur et d'un étudiant.
   - La déconnexion
  
### #5 Problème de menu 
 - Le menu n'est pas créer dans un fichier unique et ensuite inclure dans les autres fichiers 
   
    NB : Certain menu marche sur la page d'accueil, aucun menu ne marche lorsqu'on clique sur le menu affichant les utilisateurs 
   par exemple.
   
    -  iL faudrait créer un fichier `nav.tpl.php` et y mettre le code du menu 
     Dans le fichier  `nav.tpl.php`, vous pouvez mettre ce bout de code
       ```
       <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav mr-auto">
                    <li class="nav-item">
                        <a class="nav-link <?php if($viewData["currentPage"] == "main/home") echo "active"; ?>" href="<?= $router->generate('main-home') ?>">Accueil</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link <?php if($viewData["currentPage"] == "teachers/teachers_list") echo "active"; ?>" href="<?= $router->generate('teachers-list') ?>">Profs</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" <?php if($viewData["currentPage"] == "students/students_list") echo "active"; ?>" href="<?= $router->generate('students-list') ?>">Etudiants</a>
                    </li>
                </ul>
            </div>
       ```
       Ensuite l'importer dans tout les autres fichiers si vous en avez besoin (Home.tpl.php, user_list.tpl.php, etc...)
    
    Ex : Si vous voulez l'inclure dans le fichier permettant d'afficher la liste des utilisateurs, il faudrait dans le fichier `strudent_list.tpl.php`
       ajouter ce bout de code 
       
       ``` 
        include __DIR__ . '/../partials/nav.tpl.php';
       
       <!--code suivant -->
       ```
       
- Il y'a une erreur lors de vérification des champs du formulaire pour enregistrer un étudiant:
  
   - Les vérifications pour voir si les champs (firstName, lastName, status, job) sont vide ou pas ne sont pas exact.
     
        Au lieu de faire ceci :
        ```
      // tests sur les filtres
            if($lastname === false) {
                $errorList[] = "Le prénom est invalide";
            }
            if($firstname === false) {
                $errorList[] = "Le le nom est invalide";
            }
            if($status === false) {
                $errorList[] = "le status est vide";
            }
     ```

Étant donné que les diverses informations (firstName, lastName, status, jobs) ne sont pas de type booléen mais plutôt de type chaîne de caractères,
il faudrait faire ceci :
```
// Gestion de erreurs
        if(empty($firstname)) {
            $errorList[] = "Le prénom est vide ou invalide !";
        }
        if(empty($lastname)) {
            $errorList[] = "Le nom est vide ou invalide !";
        }
        if(empty($job)) {
            $errorList[] = "Le titre est vide ou invalide !";
        }
        if(empty($status)) {
            $errorList[] = "Le status est vide ou invalide !";
        }
        
 ```

La fonction filter_input() est une fonction PHP qui permet de valider et de filtrer les données d'entrée envoyées à un script PHP par les méthodes GET, POST ou COOKIE. La valeur de retour de cette fonction dépend du type de filtre utilisé et du succès de la validation.

Si la validation réussit, la fonction filter_input() renvoie la valeur filtrée. Si la validation échoue, elle renvoie false. Si la variable d'entrée n'est pas définie, elle renvoie null.


- Le travail n'est pas totalement fini car il manque diverses fonctionnalités, notamment :
    - La connexion
    - L'ajout, la suppression et la modification d'un professeur et d'un étudiant.
    - La déconnexion
    - La verification des droits avant de rendre disponible une action 

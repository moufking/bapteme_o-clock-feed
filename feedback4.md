# feedback:

> En générale le modèle mvc a été respecté (Model, Vue et controller)


### #0 Le projet ne demarre par correctement
- Dans le dossier public, il manque le fichier `.htaccess`

  -  - Le fichier `.htaccess` est un fichier de configuration utilisé par les serveurs web Apache pour permettre la personnalisation
       des paramètres de configuration pour un répertoire particulier ou un ensemble de répertoires

```
RewriteCond %{REQUEST_URI}::$1 ^(/.+)/(.*)::\2$
RewriteRule ^(.*) - [E=BASE_URI:%1]

RewriteCond %{REQUEST_FILENAME} !-d

RewriteCond %{REQUEST_FILENAME} !-f

RewriteRule ^(.*)$ index.php?page=$1 [QSA,L]
```

### #1 Abscence de controller 

- La page principale (`/`)  à besoin du controller `MainController` et de la fonction `home` pour fonctionner 
 - Il faudrait créer un fichier `MainController.php` dans le dossier `Controller` et y ajouter la fonction `home` . 
Voici un exemple du contenu de la foncton  `home`
   
```
public function home()
    {
        $this->show('main/home');
    }
 ```
### #2 Le menu n'existe pas 
- Dans le fichier `home.tpl.php`, il y'a pas l'inclusion du fichier de nav car on aura besoin de cela pour afficher
le menu 
    
    -  Il faut inclure alors `` include __DIR__ . '/../partials/nav.tpl.php';``

### #3 Affichage des menus en fonction du role
 - Les vérifications ne sont pas bien faites, car l'utilisateur doit être connecté pour avoir accès aux divers menus : Profs, utilisateurs, étudiants


### #4 Erreur de code 
- IL y'a une erreur dans le code affichant la liste des étudiants et la liste des professeurs 
````
     public function students() {

        $studentsModel = new Students
        students();
        $students = $studentsdModel->findAll();

        $this->show("students/students_list", [
            'students' => $students
        ]);

  }
````

  - Il faudrait créer une instance du model `strudent` ensuite utiliser le `findAll` pour récupérer la liste (Utilisateurs et 
    professeur)
    Ex :
````

    public function students() {
        $studentsModel = new Students();
        
        $students = $studentsdModel->findAll();

        $this->show("students/students_list", [
            'students' => $students
        ]);

}
````

- Il y'a une erreur lors de vérification des champs lors de la création d'un utilisateur:

    - Les vérifications pour voir si les champs (firstName, lastName, status, job) sont bien remplis ne sont pas exact ,
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
      

Étant donné que les diverses informations (firstName, lastName, status, jobs) ne sont pas de type booléen mais 
plutôt de type chaîne de caractères.
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

###  TP non terminé
- Il faudrait faire une connexion afin de récupérer les informations de l'utilisateur dans une session afin de voir si en
  fonction de son role(admin user)  l'utilisateur à droit de voir les menus.

- Le travail n'est pas totalement fini car il manque diverses fonctionnalités notamment :
    - La connexion
    - L'ajout, la suppression et la modification d'un professeur et d'un étudiant.
    - La déconnexion
    - L'ajout, la modification et la suppression d'un professeur, d'un étudiant
      
    - La verification des droits avant de rendre disponible une action



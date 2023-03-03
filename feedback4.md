# feedback:

> En générale le mode mvc a été respecté (Model, Vue et controller)


### #0 Le projet ne demarre par correctement
- Dans le dossier public, il manque le fichier `.htaccess`

  -  le fichier `.htaccess` permet de pointer vers le bon fichier principale du projet .Il faudrait créer un fichier .htaccess
     et copier ce code

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
Voici un exemple du contenu du fichier `home`
   
```
public function home()
    {
        $this->show('main/home');
    }
 ```
### #2 Le menu n'existe pas 
- Dans le fichier `home.tpl.php` il y'a pas l'inclusion du fichier de nav car on aura besoin de cela pour afficher
le menu 
    
    -  Il faut inclure alors `` include __DIR__ . '/../partials/nav.tpl.php';``

### #3 Affichage des menus en fonction du role
 - Les vérifications ne sont pas bien faites , car l'utilisateur doit etre connecté pour avoir accès au divers menu : Profs, utilisateurs, étudiants


### #4 Erreur de code 
- IL y'a une erreur dans le code affichant la liste des étudiants et la liste des professeurs 
````
public function students() {

        // VERIFICATION QUE L'UTILISATEUR AIT LES DROITS
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

NB: Il faudrait faire pareil pour la fonction affichant la liste des professeurs.


# feedback:

> En générale le mode mvc a été respecté (Model, Vue et controller)


### #0 Les routes ne fonctionne pas 

- Les routes ne marche fonction car dans le dossier public il manque le `.htaccess` qui doit contenir la bonne redirection.
  Voici le contenu du fichier `.htaccess`

```
RewriteCond %{REQUEST_URI}::$1 ^(/.+)/(.*)::\2$
RewriteRule ^(.*) - [E=BASE_URI:%1]

RewriteCond %{REQUEST_FILENAME} !-d

RewriteCond %{REQUEST_FILENAME} !-f

RewriteRule ^(.*)$ index.php?page=$1 [QSA,L]
```

### #1 Mettre en place les permissions:
- Concernant le code permettant de verifier si l'utilisateurs est présent dans la base de donner , je note qu'on pourrai faire une seul requete
  afin de voir si l'utilisateur est présent dans la base de donnée:
  En effet voici le code  :
  
``` php
<?php
  $user = AppUser::findByEmail($email);
if($user) {
                // Si le hash du mot de passe correspond au mot de passe en clair
                if(password_verify($password, $user->getPassword())) {
                    // On stocke en session l'id de l'utilisateur connecté
                    $_SESSION['userId'] = $user->getId();
                    $_SESSION['userRole'] = $user->getRole();

                    // redirection, car ma requête a fonctionnée
                    $route = $router->generate("main-home");
                    header('Location: '.$route);
                    exit;
                } else {
                    $errorList[] = "L'identifiant ou le mot de passe ne sont pas bon.";
                }
            } else {
                $errorList[] = "L'identifiant ou le mot de passe ne sont pas bon.";
            }
?>
```
On peut faire une seule  requete qui va vérifier si dans la base de donner il existe un utilisateur ayant le mail  et le mot de
passe entrer par l'utilisateur.

```
$pdoStatement = $pdo->prepare('SELECT * FROM app_user WHERE email = :email AND  password = :password');
```

- Concernant la fonction la requete affichant la liste des étudiants, vous avez mis ce bout de code 
```
  public function student()
  {
    $newstudent = new Student();

        $student = $newstudent->findAll();
        
        $this->show("student/student_list", [
            'students' => $student
        ]);
  }
 ```
Pas besoin d'initaliser une instance de l'object avant de faire la requete 
On peut faire ceci 
```
  public function list()
    {
        $studentsList = Student::findAll();

        $this->show('students/students_list', ["studentsResults" => $studentsList]);
    }
```

Pareil pour la fonction affichant la liste des professeurs 
```
 public function list()
    {
        $teachersList = Teacher::findAll();

        $this->show('teachers/teachers_list', ["teachersResults" => $teachersList]);

    }
```


Par contre en étant utilisateur, je peux voir la liste des professeurs, certes je peux pas `modifier` , `supprimer` et `ajouter`

NB: Il faudrait revoir là où la verification des permissions à été ajouter et le vérifier uniquement sur les actions `modifier` , `supprimer` et `ajouter` d'un professeur

- À part cela, l'ajout, la modification et la suppression d'un étudiant et d'un professeur fonctionnent bien."




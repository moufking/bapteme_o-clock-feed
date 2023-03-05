# feedback:
> En général, le modèle MVC a été respecté (Modèle, Vue et Contrôleur).

### #1 Mettre en place les permissions:

- Dans la liste des menus, il n'y a pas de vérification effectuée (connexion) afin de permettre 
  l'accès à l'utilisateur en fonction de son rôle (administrateur ou utilisateur).

- Au niveau de fichier `header.php` il y'a un problème concernant le bouton 'Se déconnecter'
  
   - La route de déconnexion n'existe pas, car elle n'est pas créer dans le fichier `ìndex.php`: Il faudrait créer cette route 
  afin que cela puisse fonctionner.
     
   - Il y'a une erreur concernant la route affichant la liste des utilisateurs.
  Voici votre code : 
  ```
                    <li class="nav-item">
                        <a class="nav-link" href="./appuser/list.html">Utilisateurs</a>
                    </li>
  
                     <li class="nav-item">
                        <a class="nav-link" href="/logout">Se déconnecter</a>
                    </li>
  
  ```
  
Il faudrait appeler la route avec son nom 
 Exemple : 
```
                  <li class="nav-item">
                        <a class="nav-link" <?php if($viewData["currentPage"] == "users/users_list") echo "active"; ?>" href="<?= $router->generate('users-list') ?>">Etudiants</a>
                  </li>
````

     
   - Il faudrait établir une connexion afin de récupérer 
     les informations de l'utilisateur dans une session, pour déterminer si l'utilisateur a le droit de voir certains menus en fonction de son rôle (administrateur ou utilisateur).
     
- Le travail n'est pas totalement fini car il manque diverses fonctionnalités, notamment : 
  - La connexion 
  - L'ajout, la suppression et la modification d'un professeur et d'un étudiant.
  - La déconnexion
  

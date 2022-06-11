
### Table des matières

* [Introduction](#intro)
* [Tester votre code](#test_code)
    * [Tests automatisés ou manuels](#auto_manuel)
    * [Tests unitaires vs tests d'intégration](#unit_inte)
    * [Choisir un testeur](#choix_testeur)
* [Écrire votre premier test](#premier_test)
    * [Où écrire le test](#ouecrire)
    * [Comment structurer un test simple](#structure_test)
    * [Comment écrire des assertions](#assertion)
    * [Effets secondaires](#effet_secondaire)
* [Exécution de votre premier test](#execution)
    * [Exécution des testeurs](#execution_testeur)
    * [Comprendre la sortie de test](#sortie_test)
    * [Exécuter vos tests depuis PyCharm](#test_pycharme)
    * [Exécution de vos tests à partir de Visual Studio Code](#test_vscode)
* [Ecrire des tests avec des frameworks Web comme Django et Flask](#frameworks)
    * [Pourquoi elles sont différentes des autres applications](#difference)
    * [Comment utiliser l'exécuteur de test Django](#executeur_django)
    * [Comment utiliser unittest et Flask](#unitest_flask)
* [Scénarios de test plus avancés](#scenario)
    * [Gestion des échecs attendus](#echecs)
    * [Isoler les comportements dans votre application](#isoler)
    * [Rédaction de tests d'intégration](#test_integration)
    * [Tester des applications basées sur les données](#test_donnees)
* [Tests dans plusieurs environnements](#environnement)
    * [Installation de Tox](#tox)
    * [Configuration de Tox pour vos dépendances](#config_tox)
    * [Exécuter Tox](#executer_tox)
* [Automatiser l'exécution de vos tests](#test_auto)
* [Et après](#apres)
    * [Introduire des linters dans votre application](#linter)
    * [Garder votre code de test propre](#test_propre)
    * [Test de la dégradation des performances entre les modifications](#degradation)
    * [Test des failles de sécurité dans votre application](#securite)
* [Conclusion](#conclusion)

### Introduction  <a class="encre" id="intro"></a>
Les tests en Python sont un sujet énorme et peuvent avoir l'ère d' être très complexes à mettre en oeuvre, mais cela n'est pas le cas. Le programmeur python peut commencer à créer des tests simples pour son application en quelques étapes simples, puis les développer à partir de là.

Dans ce article, nous apprendrons à créer un test de base, à l'exécuter et à trouver les bogues avant nos utilisateurs ! nous découvriront les outils disponibles pour écrire et exécuter des tests, vérifier les performances de notre application et même rechercher des problèmes de sécurité.

1. **Tester votre code**  <a class="encre" id="test_code"></a>
    
   Il existe de nombreuses façons de tester un code en python.  Nous apprendrons les  
   techniques des étapes les plus élémentaires et travaillerons vers des méthodes avancées.
    
    1. **Tests automatisés ou manuels** <a class="encre" id="auto_manuel"></a>
    
   La bonne nouvelle est que vous avez probablement déjà créé un test sans vous en rendre   
   compte. Vous souvenez-vous quand vous avez exécuté votre application et que vous l'avez    
   utilisée pour la première fois ? Avez-vous vérifié les fonctionnalités et expérimenté leur  
   utilisation ? C'est ce qu'on appelle les *tests exploratoires* et c'est une forme de test manuel.

   Les tests exploratoires sont une forme de test qui se fait sans plan. Dans un test exploratoire,  
   vous explorez simplement l'application.

   Pour disposer d'un ensemble complet de tests manuels, il vous suffit de dresser une liste de 
   toutes les fonctionnalités de votre application, des différents types d'entrées qu'elle peut 
   accepter et des résultats attendus. Désormais, chaque fois que vous modifiez votre code, 
   vous devez parcourir chaque élément de cette liste et le vérifier.

   Cela ne semble pas très amusant, n'est-ce pas?

   C'est là qu'interviennent les tests automatisés. Les tests automatisés sont l'exécution de 
   votre plan de test (les parties de votre application que vous souhaitez tester, l'ordre dans l
   equel vous souhaitez les tester et les réponses attendues) par un script au lieu d'un humain . 
   Python est déjà livré avec un ensemble d'outils et de bibliothèques pour vous aider à créer 
   des tests automatisés pour votre application. Nous allons explorer ces outils et bibliothèques 
   dans ce article.
    
   2. **Tests unitaires vs tests d'intégration** <a class="encre" id="unit_inte"></a>
    
   Le monde des tests ne manque pas de terminologie, et maintenant que vous connaissez la  
   différence entre les tests automatisés et manuels, il est temps d'aller plus loin.
    
   Pensez à la façon dont vous pourriez tester les phares d'une voiture. Vous allumez les 
   lumières (connu sous le nom d'étape de test ) et sortez de la voiture ou demandez à un ami 
   de vérifier que les lumières sont allumées (connu sous le nom d'assertion de test ). Le test de 
   plusieurs composants est appelé test d'intégration .
    
   Pensez à toutes les choses qui doivent fonctionner correctement pour qu'une tâche simple 
   donne le bon résultat. Ces composants sont comme les parties de votre application, toutes 
   ces classes, fonctions et modules que vous avez écrits.
    
   Un défi majeur avec les tests d'intégration est lorsqu'un test d'intégration ne donne pas le 
   bon résultat. Il est très difficile de diagnostiquer le problème sans pouvoir isoler quelle partie 
   du système est défaillante. Si les lumières ne s'allument pas, les ampoules sont peut-être 
   cassées. La batterie est-elle morte ? Et l'alternateur ? L'ordinateur de la voiture est-il en 
   panne ?
   
   Si vous avez une voiture moderne et élégante, elle vous dira quand vos ampoules ont disparu. 
   Il le fait en utilisant une forme de test unitaire .
   
   Un test unitaire est un test plus petit, qui vérifie qu'un seul composant fonctionne 
   correctement. Un test unitaire vous aide à isoler ce qui est cassé dans votre application et à le 
   réparer plus rapidement.
   
   Vous venez de voir deux types de tests :
   
     - Un test d'intégration vérifie que les composants de votre application fonctionnent les uns avec les autres.
     
     - Un test unitaire vérifie un petit composant de votre application.
   
   Vous pouvez écrire à la fois des tests d'intégration et des tests unitaires en Python. Pour 
   écrire un test unitaire pour la fonction intégrée sum(), vous devez vérifier la sortie de sum() 
   par rapport à une sortie connue.
  
   Par exemple, voici comment vous vérifiez que la somme des nombres (2, 2, 3) est égal à 7 :
   
   ```python
   assert sum([2,2,3]) == 7, "doit être 7"
   ```
   
   cela n'affichera rien car les valeurs sont correctes. 
   Si le résultat de sum() est incorrect, cela echouera avec un *AssertionError* et le message 
   "doit être 7"
   
  ![test0](/img/assert_sum.png)
  
  Voyons comment ecrire une fonction pour faire notre test :
  ```python
  def test_sum():
    assert sum([2,2,3]) == 7, "doit être 7"
   
   if __name__ == "main":
     test_sum()
     print("Tout est passé !")
  ```
  
  Nous avons maintenant écrit un cas de test , une assertion et un point d'entrée. Nous pouvons maintenant exécuter ceci et on verra le message "Tout est passé !"  car le résultat est exacte.
  
  En Python, sum() accepte tout itérable comme premier argument. Nous avons testé avec une liste. Maintenant, testons également avec un tuple. Créez un nouveau fichier appelé test_sum_2.py avec le code suivant :
  
  ```python
  def test_sum():
    assert sum([2, 2, 3]) == 7, "doit être 7"

def test_sum_tuple():
    assert sum((4, 2, 2)) == 7, "doit être 7"

if __name__ == "__main__":
    test_sum()
    test_sum_tuple()
    print("Tout est passé !")
    
  ```
  
  Lorsque vous exécutez test_sum_2.py, le script génère une erreur car la somme de (4, 2, 2) est 8, et non 7. Le résultat du script vous donne le message d'erreur :
  
  ![fonction_test](/img/fonction_test.png)
   
Ici, vous pouvez voir comment une erreur dans votre code génère une erreur sur la console avec des informations sur l'emplacement de l'erreur et le résultat attendu.

Écrire des tests de cette manière est acceptable pour une simple vérification, mais que se passe-t-il si plusieurs échouent ? C'est là qu'interviennent les *test runners* ou lanceurs de test. Le test runner est une application spéciale conçue pour exécuter des tests, vérifier la sortie et vous donner des outils pour déboguer et diagnostiquer les tests et les applications.

   iii.  **Choisir un testeur** <a class="encre" id="choix_testeur"></a>

Il existe de nombreux testeurs disponibles pour Python. Celui intégré à la bibliothèque standard Python s'appelle unittest. Dans ce article, nous utiliserons des scénarios de test *unittest* et *unittest test runner*. Les principes de unittest sont facilement transférables à d'autres frameworks. Les trois test runner les plus populaires sont :

  * unittest
  * nose ou nose2
  * pytest
  
Il est important de choisir le meilleur testeur pour vos besoins et votre niveau d'expérience.
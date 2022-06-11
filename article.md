
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

##### unittest
unittest est intégré à la bibliothèque standard Python depuis la version 2.1. Vous le verrez probablement dans les applications Python commerciales et les projets open source.

unittest contient à la fois un framework de test et un test runner. unittest a des exigences importantes pour l'écriture et l'exécution des tests.

unittest nécessite que :
   * Vous mettez vos tests dans des classes en tant que méthodes
   * Vous utilisez une série de méthodes d'assertion spéciales dans la classe unittest.TestCase au lieu des assertions
 
 Pour convertir l'exemple précédent en cas de test unittest, vous devez :
   * Importer unittest depuis la bibliothèque standard
   * Créer une classe appelée TestSum qui hérite de la  classe TestCase
   * Convertissez les fonctions de test en méthodes en ajoutant self comme premier argument
   * Modifier les assertions pour utiliser la méthode self.assertEqual() de la classe TestCase
   * Appeler unittest.main() dans le main
   
   Si vous exécutez ceci sur la ligne de commande, vous verrez un succès (indiqué par .) et un 
   échec (indiqué par F):
   
    .F
        ======================================================================
              FAIL: test_sum_tuple (__main__.TestSum)
    ----------------------------------------------------------------------
     Traceback (most recent call last):
    File "/Users/gredey/Documents/test_unitest0.py", line 10, in test_sum_tuple
    self.assertEqual(sum((1, 2, 2)), 7, "doit être 7")
    AssertionError: 5 != 7 : doit être 7

    ----------------------------------------------------------------------
    Ran 2 tests in 0.000s

    FAILED (failures=1)
    (base) gredey@MacBook-Pro-de-OUEDRAOGO-2 Documents % 

Nous venons d'exécuter deux tests à l'aide du test runner de unittest.

    Remarque : Soyez prudent si vous écrivez des cas de test qui doivent être exécutés à la fois en Python 2 et 3. 
    En Python 2.7 et versions antérieures, unittest s'appelle unittest2. 
     Si vous importez simplement depuis unittest,vous obtiendrez différentes versions avec différentes fonctionnalités entre Python 2 et 3.
     
 
 Pour plus d'informations sur unittest, vous pouvez explorer la [documentation de unittest](https://docs.python.org/3/library/unittest.html)
 
##### nose

Vous constaterez peut-être qu'au fil du temps, alors que vous écrivez des centaines, voire des milliers de tests pour votre application, il devient de plus en plus difficile de comprendre et d'utiliser la sortie de unittest.

nose est compatible avec tous les tests écrits à l'aide du framework  unittest et peut être utilisé en remplacement de l' unittest test runner. Le développement de nose en tant qu'application open source a pris du retard et un fork appelé nose2 a été créé. Si vous partez de zéro, il est recommandé d'utiliser à la place de nose, nose2.

Pour commencer avec nose2, installez nose2 à partir de PyPI et exécutez-le sur la ligne de commande. nose2 essaiera de découvrir tous les scripts de test nommés test*.py et les cas de test dont ils héritent de unittest.TestCase  dans votre répertoire actuel :

    (base) gredey@MacBook-Pro-de-OUEDRAOGO-2 Documents % pip3 install nose2
    Collecting nose2
    Downloading nose2-0.11.0-py2.py3-none-any.whl (144 kB)
     |████████████████████████████████| 144 kB 113 kB/s 
    Collecting coverage>=4.4.1
    Downloading coverage-6.4.1-cp39-cp39-macosx_10_9_x86_64.whl (184 kB)
     |████████████████████████████████| 184 kB 56 kB/s 
    Requirement already satisfied: six>=1.7 in /Users/gredey/opt/anaconda3/lib/python3.9/site-packages (from nose2) (1.16.0)
    Installing collected packages: coverage, nose2
    Successfully installed coverage-6.4.1 nose2-0.11.0
    (base) gredey@MacBook-Pro-de-OUEDRAOGO-2 Documents % python3 -m nose
      .F
    ======================================================================
            FAIL: test_sum_tuple (test_unitest0.TestSum)
    ----------------------------------------------------------------------
    Traceback (most recent call last):
    File "/Users/gredey/Documents/test_unitest0.py", line 10, in test_sum_tuple
    self.assertEqual(sum((1, 2, 2)), 7, "doit être 7")
    AssertionError: 5 != 7 : doit être 7

    ----------------------------------------------------------------------
    Ran 2 tests in 0.065s

    FAILED (failures=1)
    (base) gredey@MacBook-Pro-de-OUEDRAOGO-2 Documents % 

 Nous venons d'exécuter le test que nous avons créé  test_sum_unittest.py à partir du testeur nose2. nose2 propose de nombreux indicateurs de ligne de commande pour filtrer les tests que vous exécutez. Pour plus d'informations, vous pouvez explorer la [documentation de Nose 2](https://docs.nose2.io/en/latest/) .
 
##### pytest
 
 pytest prend en charge l'exécution des cas de test unittest. Le véritable avantage de pytest vient en écrivant des cas de test pytest.  Les cas de test pytest sont une série de fonctions dans un fichier Python commençant par le nom *test_.*

pytest possède d'autres fonctionnalités intéressantes :

  * Prise en charge de l'instruction intégrée  assert au lieu d'utiliser des méthodes spéciales  self.assert*()
  * Prise en charge du filtrage des cas de test
  * Possibilité de relancer à partir du dernier test ayant échoué
  * Un écosystème de centaines de plugins pour étendre les fonctionnalités

L' exemple de cas de test de TestSum avec pytest ressemblerait à ceci :

```python
def test_sum():
    assert sum([2, 2, 3]) == 7, "doit être 7"

def test_sum_tuple():
    assert sum((4, 2, 2)) == 7, "doit être 7"
```

Nous avons supprimé le TestCase, toute utilisation de classes et le point d'entrée de la ligne de commande.

Plus d'informations peuvent être trouvées sur le site Web de [documentation de Pytest](https://docs.pytest.org/en/latest/) .


2. Écrire votre premier test <a class="encre" id="premier_test"></a>

Rassemblons ce que nous avons appris jusqu'à présent et, au lieu de tester la fonction sum() intégrée, testons une implémentation simple de la même exigence.

Créez un nouveau dossier de projet et, à l'intérieur de celui-ci, créez un nouveau dossier appelé my_sum. A l' intérieur de my_sum, créez un fichier vide appelé  __init__.py. La création du fichier __init__.py  signifie que le dossier my_sum  peut être importé en tant que module à partir du répertoire parent.

Votre dossier de projet devrait ressembler à ceci :

    project/
    │
    └── my_sum/
       └── __init__.py
       
 Ouvrez my_sum/__init__.py et créez une nouvelle fonction appelée sum(), qui prend un 
 itérable (une liste, un tuple ou un ensemble) et additionne les valeurs :
 
 ```python
 def sum(arg):
   total = 0
   for val in arg:
      total += val
      return total
 ```
 
 Cet exemple de code crée une variable appelée total, parcourt toutes les valeurs de arg et les ajoute à total. Il renvoie ensuite le résultat une fois l'itérable épuisé.
 
   * Où ecrire le test <a class="encre" id="ouecrire"></a>
   
   Pour commencer à écrire des tests, vous pouvez simplement créer un fichier appelé test.py,  
   qui contiendra votre premier cas de test. Étant donné que le fichier devra pouvoir importer 
   votre application pour pouvoir la tester, vous souhaitez placer test.py au-dessus du dossier 
   du package, de sorte que votre arborescence de répertoires ressemblera à ceci :
   
     project/
     │
     ├── my_sum/
     │   └── __init__.py
     |
     └── test.py
     
   Vous constaterez que, au fur et à mesure que vous ajoutez de plus en plus de tests, votre fichier unique deviendra encombrant et difficile à maintenir, vous pouvez donc créer un dossier appelé tests et diviser les tests en plusieurs fichiers. Il est de convention de s'assurer que chaque fichier commence par test_ afin que tous les testeurs supposent que le fichier Python contient des tests à exécuter. Certains très grands projets divisent les tests en plusieurs sous-
 répertoires en fonction de leur objectif ou de leur utilisation.
 
     Remarque : Que se passe-t-il si votre application est un    
     script unique ?

    Vous pouvez importer tous les attributs du script, tels que 
    les classes, les fonctions et les variables à l'aide de la  
    fonction __import__() intégrée. Au lieu de from my_sum import 
    sum, vous pouvez écrire ce qui suit :
    
```python
          target = __import__("my_sum.py")
          sum = target.sum
```
    L'avantage de l'utilisation __import__()est que vous n'avez 
    pas à transformer votre dossier de projet en package et vous 
    pouvez spécifier le nom du fichier. Ceci est également utile 
    si votre nom de fichier entre en collision avec des 
    packages de bibliothèque standard. Par exemple, math.py 
    entrerait en collision avec le module math .
    
  
  * Comment structurer un test simple <a class="encre" id="structure"></a>

Avant de vous plonger dans l'écriture de tests, vous voudrez d'abord prendre quelques décisions :

  1. Que veux-tu tester ?
  2. Êtes-vous en train d'écrire un test unitaire ou un test d'intégration ?

Ensuite, la structure d'un test devrait suivre vaguement ce flux de travail :

  1. Créez vos entrées
  2. Exécuter le code testé, en capturant la sortie
  3. Comparer la sortie avec un résultat attendu
   
   Pour cette application, vous testez sum(). Vous pouvez vérifier de nombreux comportements de sum(), tels que :

  * Peut-il additionner une liste de nombres entiers (entiers) ?
  * Peut-il résumer un tuple ou un ensemble ?
  * Peut-il résumer une liste de flotteurs ?
  * Que se passe-t-il lorsque vous lui fournissez une mauvaise valeur, comme un seul entier ou une chaîne ?
  * Que se passe-t-il lorsque l'une des valeurs est négative ?
  
Le test le plus simple serait une liste d'entiers. Créez un fichier, test.py avec le code Python suivant :

```python
import unittest

from my_sum import sum


class TestSum(unittest.TestCase):
    def test_list_int(self):
        """
        Test that it can sum a list of integers
        """
        data = [1, 2, 3]
        result = sum(data)
        self.assertEqual(result, 6)

if __name__ == '__main__':
    unittest.main()
```
   

Cet exemple de code :

1. Importations de sum() depuis le package my_sum  que vous avez créé

2. Définit une nouvelle classe de cas de test appelée TestSum, qui hérite de unittest.TestCase

3. Définit une méthode de test, .test_list_int(), pour tester une liste d'entiers. La méthode .test_list_int() va :

   * Déclarer une variable data avec une liste de nombres(1, 2, 3)
   * Affecter le résultat de my_sum.sum(data) à une variable result 
   * Affirmer que la valeur de resulte est egal à 6 en utilisant la méthode .assertEqual() sur la classe unittest.TestCase 
4. Définit un point d'entrée de ligne de commande, qui exécute le test-runner unittest .main()

* Comment écrire des assertions

   La dernière étape de l'écriture d'un test consiste à valider la sortie par rapport à une réponse connue. C'est ce qu'on appelle une affirmation . Il existe quelques bonnes pratiques générales concernant la rédaction des assertions :
   
   * Assurez-vous que les tests sont reproductibles et exécutez votre test plusieurs fois pour vous assurer qu'il donne le même résultat à chaque fois
   * Essayez d'affirmer les résultats qui se rapportent à vos données d'entrée, par exemple en vérifiant que le résultat est la somme réelle des valeurs dans l'exemple sum() 
   
unittest est livré avec de nombreuses méthodes pour affirmer les valeurs, les types et l'existence des variables. Voici quelques-unes des méthodes les plus couramment utilisées :

| Method                              | Equivalent to         |
|-----------------------   |------------------
|   .assertEqual(a, b)           | a == b                    |
|   .assertTrue(x)                  | bool(x) is true      | 
|   .assertFalse(x)                 | bool(x) is false    | 
|   .assertIs(a, b)                   | a is b                      |
|   .assertIsNone(x)              | x is none              |
|   .assertIn(a, b)                   | a in b                      |
|   .assertIsInstance(a, b)    | isinstance(a, b)   |

.assertIs(), .assertIsNone(), .assertIn(), et .assertIsInstance() ont tous des méthodes opposées, nommées .assertIsNot(), et ainsi de suite.

* Effets secondaires <a class="encre" id="Effets secondaires"></a>

   Lorsque vous écrivez des tests, ce n'est souvent pas aussi simple que de regarder la valeur 
   de retour d'une fonction. Souvent, l'exécution d'un morceau de code modifie d'autres 
   éléments de l'environnement, tels que l'attribut d'une classe, un fichier sur le système de 
   fichiers ou une valeur dans une base de données. Ceux-ci sont connus sous le nom d' effets 
   secondaires et constituent une partie importante des tests. Décidez si l'effet secondaire est 
   testé avant de l'inclure dans votre liste d'affirmations.

  
  Si vous constatez que l'unité de code que vous souhaitez tester a de nombreux effets   
  secondaires, vous enfreignez peut-être le principe de responsabilité unique . Briser le principe 
  de responsabilité unique signifie que le morceau de code fait trop de choses et qu'il vaudrait 
  mieux qu'il soit refactorisé. Suivre le principe de responsabilité unique est un excellent moyen 
  de concevoir du code qui facilite l'écriture de tests unitaires simples et répétables pour des 
  applications fiables.
 






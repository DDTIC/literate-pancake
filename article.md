---
title: |
  "Introduction to Python modules."
date: May, 2022
lang: en-EN
urlcolor: blue
geometry: "left=2.5cm,right=2.5cm,top=3cm,bottom=3cm"
documentclass: article
fontfamily: Alegreya
header-includes: |
    \usepackage{fancyhdr}
    \pagestyle{fancy}
    \fancyhf{}
    \rhead{Dakar Institute of Technology}
    \lhead{Patrick Nsukami}
    \rfoot{Page \thepage}
    \hypersetup{pdftex,
            pdfauthor={Patrick Nsukami},
            pdftitle={Introduction to Python programming},
            pdfsubject={Introduction to Python programming},
            pdfkeywords={Python, Programming},
            pdfproducer={Emacs, Pandoc, Latex, Markdown},
            pdfcreator={Emacs, Pandoc, Latex, Markdown}}
    
---
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
    
    * **Tests automatisés ou manuels** <a class="encre" id="auto_manuel"></a>
    
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
    
    * **Tests unitaires vs tests d'intégration** <a class="encre" id="unit_inte"></a>
    
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

# Foo:

foo bar baz

```python
# module foo.py

a = 42

def bar(x):
    print(x)
```

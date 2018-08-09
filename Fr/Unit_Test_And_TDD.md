﻿# Tests unitaires & TDD

## Tour d'horizon sur les tests
  
Les tests sont importants dans la réalisation d’un logiciel car ils assurent une **qualité logicielle minimale**. 
Beaucoup de logiciels sont conçus sans test et les conséquences sont:  
- beaucoup d’**anomalies**
- des **bugs** compliqués à résoudre et parfois lancés en production.
- un logiciel **qui grossit** à cause des **patchs** et des nouvelles fonctionnalités s’appuyant sur un code bancal. 
En bref, un code **non testé** entraîne bien souvent une **dette technique**.

![Image de la pyramide des tests](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/img_pyramide_des_tests.png?token=AHjeDoKKyxTJRlb2sRXe6a4YPP5RwJQTks5bYCFzwA%3D%3D)

La **pyramide des tests** présente les différents **types** de tests :

- Les **tests IHM** (Interface Homme-Machine) coûtent cher car il faut exécuter l’appli de bout en bout. Ces tests peuvent être automatisés mais long à être exécutés. Ils ne couvrent que certains scénarios précis. Quand ces tests ne sont pas automatisés, on peut engager des testeurs.

- Les **tests d’intégration** sont assez simples. On s’affranchit des contraintes majeures (IHM, certaines dépendances). Ils sont également assez proches du code, ce qui facilite leur refactoring. Cependant, ils couvrent un spectre plus large de code et de ce fait, ont davantage de risques d’être impactés par une modification.

- Les **tests unitaires** sont simples à mettre en œuvre lorsqu’ils sont faits au fur et à mesure du code, ils permettent de couvrir une majeure partie du code à faible coût. Ils peuvent être **automatisés** et sont **rapides à exécuter**. Il est fortement recommandé de commencer par rédiger des tests unitaires, ils sont la **base** des tests et permettent de repérer rapidement les régressions ou les mauvais comportements du logiciel.

:warning: **Attention** : Il faut séparer les tests unitaires (rapide) des tests d’intégrations (plus lent).

Pour le moment nous nous intéresserons aux **tests unitaires et au TDD (test-driven development)**.

**Rédiger des tests unitaires sert à vérifier le comportement du code, tandis qu’avec l’approche TDD, qui consiste à écrire les tests en premier, rédiger des tests unitaires sert à spécifier comment le code doit fonctionner**.

Pour en savoir plus sur les tests, je vous conseille [l'article sur la pyramide des tests du blog d'OCTO Technology](https://blog.octo.com/la-pyramide-des-tests-par-la-pratique-1-5) , [ou une de leurs publications, Culture Code](https://www.octo.com/fr/publications/20-culture-code).

[Retour au menu](Summary.md)

## Tests Unitaires (TU)

Les **tests unitaires** peuvent être regroupés dans un **projet de tests**.  
Ce dernier doit avoir la même **structure** que le projet que l’on souhaite tester. Ainsi, **Une classe de l’application = Une classe de test**.

### Pourquoi écrire un test unitaire ?

- Être sûr de ne **pas** faire de **retour en arrière**, de ne pas avoir cassé un code en le modifiant avant la mise en production

-  **Réduire les bugs** dans les fonctionnalités (déjà implémentées ou nouvelles)

- **Refacto en toute quiétude** : ils réduisent la peur de faire des **modifications** et d’**ajouter de nouvelles fonctionnalités** dans le programme ou l’application.

- Traiter **en amont** des tests d'interface et des utilisateurs certains mauvais comportements et donc de les **corriger plus tôt**.

- Avoir un **feedback rapide** sur le code que les developpeurs viennent d'ajouter ou modifier.

- Permet d’avoir une application plus **robuste**

- Avoir une documentation minimale pour les développeurs

En conclusion, plus rapidement vous saurez si un test échoue, plus vite vous pourrez corriger le problème et moins cher ce sera.

### Qu'est-ce qu'un bon test unitaire ?

Un bon test unitaire :
- est **entièrement automatisé**
- retourne **toujours le même résultat** si le code n’est pas modifié
- test qu’**un seul concept** ou **une seule logique** de l’application
- test qu’**une seule méthode à la fois**
- porte un **nom clair et significatif**

### Comment écrire un test unitaire ? 

Différentes conventions de nommage existent :
- Should_ExpectedBehavior_When_StateUnderTest
- When_StateUnderTest_Expect_ExpectedBehavior

Attention à **ne pas avoir le nom de la méthode dans le nom du test** car difficile à maintenir. En cas de refactorisation du nom de la méthode, le test ne veut plus rien dire.
Un test unitaire exprime une intention, c'est à dire, qu'il test une fonctionnalité, et non son implémentation.

Un test se décompose en **3 parties** : 
- **Arrange** est l’étape d’initialisation
- **Act** correspond à l’appel de la méthode testée
- **Assert** est l’étape de vérification du comportement de la méthode testée

Lorsque l'on écrit un test unitaire, il est conseillé de commencer par rédiger l'**Assert**, qui est la réponse à la question : **Qu'est-ce qu'on veut tester?**

[Retour au menu](Summary.md)

### Comment mettre en pratique les TU avec Unity ?

#### Unity Test Runner

Pour réaliser des tests unitaires, Unity met à disposition un outil appelé le [**Unity Test Runner**](https://docs.unity3d.com/Manual/testing-editortestsrunner.html)  
Pour afficher la fenêtre *"Test Runner"*, allez dans *Windows > Test Runner*.

![Image of Test Runner](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/UnityTestsRunner/Capture1_ouvertureOnglet.PNG)

On peut voir sur la capture d'écran, qu'il existe **deux modes** :

- **PlayMode** :
  - permet d'executer des tests sur **plusieurs frames**
  - comportement Awake(), Start(), ... sont executés **automatiquement**
  - Sert davantage pour les **tests d'intégration**
  - [UnityTest] = exécuté comme une **Coroutine classique**
  - Ouvre une **scène** de test pour executé les tests (:warning: pensez à bien **enregistrer** votre scène avant de lancer les tests car votre scène sera écrasée au lancement des tests)

- **EditMode** :
  - Chaque test est executé en **une frame**
  - Il faut appeler **explicitement** les méthodes Awake() et Start(), ce qui necessite de les passer en **public**.
  - **Doit être placés dans un dossier Editor**
  - [UnityTest] est executé dans l'editor avec **"Editor.Application.Update"**

Sélectionnez le mode qui vous intéresse puis cliquez sur le bouton *“Create PlayMode/EditMode Test Assembly Folder”*. 
*Unity* va créer automatiquement un dossier *Test* avec un fichier *“Tests.asmdef”de type “assembly definition”*.  
  
Une fois le dossier créé, placez-vous à l’intérieur puis cliquez sur *“Create Test Script in current folder”*.

![Image of Test Runner create script](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/UnityTestsRunner/Capture2_btn_create_script.PNG)

Unity va alors créer un fichier *template C#* avec le code suivant :

```cs
using UnityEngine;
using UnityEngine.TestTools;
using NUnit.Framework;
using System.Collections;

public class NewTestScript {

    [Test]
    public void NewTestScriptSimplePasses() {
        // Use the Assert class to test conditions.
    }

    // A UnityTest behaves like a coroutine in PlayMode
    // and allows you to yield null to skip a frame in EditMode
    [UnityTest]
    public IEnumerator NewTestScriptWithEnumeratorPasses() {
        // Use the Assert class to test conditions.
        // yield to skip a frame
        yield return null;
    }
}
```

Ces deux tests devraient apparaître dans le Test Runner :

![Image of Test Runner tests with test](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/UnityTestsRunner/Capture3_tests.PNG)

Si vous cliquez sur **“Run All”**, les tests vont se lancer, vu que ces tests n’ont pas d’assertion, les tests devrait passer au **vert** directement. 

![Image of Test Runner tests green tests](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/UnityTestsRunner/Capture4_green_tests.PNG)

Maintenant, si nous remplaçons le premier test par celui ci :

```cs
    [Test]
    public void NewTestScriptSimplePasses() {
        // Use the Assert class to test conditions.
        Assert.AreEqual(4, 3);
    }
```

le test devrait être **rouge** car 3 et 4 ne sont pas égaux :

![Image of Test Runner tests red test](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/UnityTestsRunner/Capture5_red_tests.PNG)

Si vous cliquez sur le test en question, vous pouvez voir un message d’erreur et pourquoi le test a échoué (dans l’exemple : Expected : 4, But was : 3) :

![Image of Test Runner tests red test explanation](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/UnityTestsRunner/Capture5_red_tests_with_error_msg.PNG)

Aujourd'hui, *Unity* ne permet pas de lancer les tests en lignes de commande.
Les tests appelant des *MonoBehaviour* ne peuvent pas être éxécuté depuis [*Visual Studio*](https://visualstudio.microsoft.com/).  
Si vous utilisez [***Rider***](https://www.jetbrains.com/dotnet/promo/unity/), vous pourrer les lancer directement dans son interface.

[Retour au menu](Summary.md)

#### Les Scripts de tests

**Le nom de la classe de test doit avoir en préfixe ou en suffixe *“Test”*, selon vos préférences.**  
Veillez à ce que votre code soit harmonisé, ainsi si vous choisissez de mettre “Test” en suffixe, faites le sur toutes vos classes de tests.  

Par défaut Unity inclus le framework de test unitaire C#  **[NUnit](https://nunit.org/)** pour les tests unitaires.
Devant chaque méthode de test l’attribut **[Test]** doit apparaître :

```cs
[Test]
public void Methode_Test() {
 
    ...
 
}
```

Avec le **using UnityEngine.TestTools;**, vous pouvez créer des **"Unity Tests"** :

```cs
[UnityTest]
public IEnumerator Methode_Unity_Test() {
 
    ...
    yield return null;
    ...
 
}
```

Un *Unity Test* est une [**Coroutine**](https://docs.unity3d.com/Manual/Coroutines.html). Dans *Unity*, les *Coroutines* sont généralement utilisé pour gérer le rendu. Nous pouvons par exemple, **attendre une frame** (*yield return null*) ou un **nombre de secondes x** (*yield return new WaitForSecondes(x)*) pour effectuer une action.

Ces tests permettent donc de tester les comportements qui dépendent d'*Unity*. Ils s'apparentent le plus souvent à des **tests d'intégration**

Pour réaliser une assertion avec un test *NUnit*, il faut utiliser la classe **Assert**, quelques exemples :

```cs
    Assert.IsNotNull(object);
    Assert.IsTrue(boolean);
    Assert.AreEqual(value1, value2)
```

**Dans le cas de *Asset.AreEqual*, la value1 correspond à la valeur attendue est la value2 est la valeur testée. Cet ordre est important et permet notamment d'avoir des messages d'erreurs cohérents :  *Expected "value1" but was "value2"*.**

*Unity* ajoute également ses **propres assertions** (avec using UnityEngine.Assertions et UnityEngine.TestTools.Utils) comme :
- *"Assert.AreApproximatelyEqual"* et *"Assert.AreNotApproximatelyEqual"* qui prennent par défaut une tolérance de  0.00001f qui permettent de comparer deux floats.
- *"ColorEqualityComparer"*
- *"QuaternionEqualityComparer"*
- *"Vector2EqualityComparer"* ,*"Vector3EqualityComparer"* et *"Vector4EqualityComparer"*


:warning: Les méthode *Awake*, *Start* et *Update* doivent être passé en public pour être appelée avec les tests.

Si tous vos tests ont le même *"Arrange"* vous pouvez utilisez les attributs [SetUp] et [TearDown].
La méthode sous l'attribut [SetUp] s'executera avant toutes les méthodes [Tests] et la méthode sous l'attribut [TearDown] s'executera aprés  toutes les méthodes [Tests].

Ainsi : 
```cs
public class MaClassTest {
  [Test]
  public void Methode_Test_0() {
    //Arrange
    GameManager game = new GameManager();
    ...
    //Act
    ...
    //Assert
    ...
  }

  [Test]
  public void Methode_Test_1() {
    //Arrange
    GameManager game = new GameManager();
    ...
    //Act
    ...
    //Assert
    ...
  }
}
```
Devient : 
```cs
public class MaClassTest {
  
  GameManager game;
  
  [SetUp]
  public void Init(){
    game = new GameManager();
  }
  
  [TearDown]
  public void Dispose() {
    Destroy(game);
  }

  [Test]
  public void Methode_Test_0() {
    //Arrange
    ...
    //Act
    ...
    //Assert
    ...
  }

  [Test]
  public void Methode_Test_1() {
    //Arrange
    ...
    //Act
    ...
    //Assert
    ...
  }

```


Source : Unite Austin 2017 - Testing for Sanity: Using Unity's Integrated TestRunner, https://www.youtube.com/watch?v=MWS4aSO7HAo


[Retour au menu](Summary.md)

## Développement piloté par les tests (TDD)

Le **TDD ou Développement piloté par les tests** est une technique de développement qui consiste à écrire les tests **avant** d’écrire le code. Cela garantit davantage la structure du code comme testable et maintenable.
En utilisant le *TDD*, les tests unitaires ne servent plus à valider un code existant mais à **spécifier** le futur code implémenté.

![schema cycle TDD](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/RED-GREEN-REFACTO%20cycle.png?token=AHjeDtvHOiXwwhctewLgG-4a0KSUEjf7ks5bYXxlwA%3D%3D)

Comme nous pouvons le voir sur le schéma ci-dessus, le *TDD* possède **3** grandes étapes : 
- **RED** : On commence par écrire un test et on vérifie que ce dernier échoue (car le code n'est pas implémenté). Ce test spécifie le comportement d'une méthode (ce qu'elle doit renvoyer, ce qu'elle doit appeler, ...).
- **GREEN** : On écrit le code minimum pour que le test passe au vert.
- **REFACTOR** : On améliore le code sans changer son comportement.

Puis on passe à l'écriture d'un autre test, et la boucle recommence.

Dans le livre *Clean Code (p.80)*, Oncle Bob définit les 3 lois du *TDD* :

   1. Vous n'êtes pas autorisé à écrire du code métier tant que vous n'avez pas écrit un premier test unitaire qui échoue.
   2. Vous n'êtes pas autorisé à écrire plus qu'un test unitaire qui est suffisant pour échouer et qui ne compile pas.
   3. Vous n'êtes pas autorisé à écrire plus de code que ce qui est suffisant pour passer au vert le test unitaire.
  
:warning: *Oncle Bob*, insiste sur le fait, qu'écrire des tests unitaires ou pratiquer le *TDD* n'est pas magique, et qu'on peut très bien continuer d'écrire du mauvais code et d'écrire des mauvais tests.
Il précise également que suivre les trois lois n'est pas toujours appropprié, il faut donc trouver des solutions adaptés à notre projet.


TODO : exercice à trou (suite ou reprise du même exo) TU avec TDD. 


[Retour au menu](Summary.md)
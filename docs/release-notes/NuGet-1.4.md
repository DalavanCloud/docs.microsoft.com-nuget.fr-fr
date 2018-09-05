---
title: Notes de publication NuGet 1.4
description: Notes de publication pour NuGet 1.4, y compris les problèmes connus, les correctifs de bogues, les fonctionnalités ajoutées et les dcr.
author: karann-msft
ms.author: karann
ms.date: 11/11/2016
ms.topic: conceptual
ms.openlocfilehash: 3f4d712f83ea108a82a1bc4910c2a721a8c24d51
ms.sourcegitcommit: 1d1406764c6af5fb7801d462e0c4afc9092fa569
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43550629"
---
# <a name="nuget-14-release-notes"></a>Notes de publication NuGet 1.4

[Notes de publication de NuGet 1.3](../release-notes/nuget-1.3.md) | [Notes de publication de NuGet 1.5](../release-notes/nuget-1.5.md)

NuGet 1.4 a été publiée le 17 juin 2011.

## <a name="features"></a>Fonctionnalités

### <a name="update-package-improvements"></a>Améliorations apportées aux packages de mise à jour
NuGet 1.4 introduit de nombreuses améliorations à la commande de Package de mise à jour, ce qui la rend plus facile à maintenir des packages à la même version sur plusieurs projets dans une solution. Par exemple, lors de la mise à niveau un package vers la version la plus récente, il est très courant de tous les projets avec ce package installé pour être mis à jour vers la même verision.

Le `Update-Package` commande maintenant facilite :

#### <a name="update-all-packages-in-a-single-project"></a>Mettre à jour tous les packages dans un seul projet

    Update-Package -Project MvcApplication1

#### <a name="update-a-package-in-all-projects"></a>Mettre à jour un package dans tous les projets

    Update-Package PackageId

#### <a name="update-all-packages-in-all-projects"></a>Mettre à jour tous les packages dans tous les projets

    Update-Package

#### <a name="perform-a-safe-update-on-all-packages"></a>Effectuer une mise à jour « sécurisée » sur tous les packages
Le `-Safe` indicateur contraint les mises à niveau vers les versions uniquement avec le même composant de version majeure ou mineure. Par exemple, si la version 1.0.0 d’un package est installée, et les versions 1.0.1 1.0.2, et 1.1 sont disponibles dans le flux, le `-Safe` indicateur met à jour le package à la version 1.0.2. La mise à niveau sans le `-Safe` indicateur serait de mettre à niveau le package vers la dernière version, 1.1.

    Update-Package -Safe

### <a name="managing-packages-at-the-solution-level"></a>La gestion des Packages au niveau de la Solution
Avant NuGet 1.4, l’installation d’un package en plusieurs projets était encombrant à l’aide de la boîte de dialogue. Il nécessaire de lancer la boîte de dialogue une fois par projet.

NuGet 1.4 ajoute la prise en charge pour l’installation/désinstallation/mise à jour des packages dans plusieurs projets en même temps. Il vous suffit de lancer l’en cliquant avec le bouton avec le bouton droit sur la Solution et en sélectionnant le **gérer les Packages NuGet** option de menu.

![Boîte de dialogue Manage NuGet Packages de niveau solution](./media/manage-nuget-packages-solution-dialog.png)

Notez que, dans la barre de titre de la boîte de dialogue, le nom de la solution est affiché, pas le nom d’un projet.
Opérations de package fournissent désormais une liste de cases à cocher avec la liste de l’opération doit s’appliquer à des projets.

![Gérer la sélection de projet de Packages NuGet](./media/manage-nuget-packages-project-selection.png)

Pour plus d’informations, consultez la rubrique sur [la gestion des Packages pour la Solution](../tools/package-manager-ui.md#managing-packages-for-the-solution).

### <a name="constraining-upgrades-to-allowed-versions"></a>Restriction des mises à niveau à autorisé des Versions
Par défaut, lorsque vous exécutez le `Update-Package` commande sur un package (ou la mise à jour le package à l’aide de la boîte de dialogue), il sera mis à jour vers la dernière version dans le flux. Avec la nouvelle prise en charge pour la mise à jour tous les packages, il peut arriver dans lequel vous souhaitez verrouiller un package à une plage de versions spécifiques. Par exemple, vous savez à l’avance que votre application fonctionne uniquement avec la version 2.* d’un package, mais pas 3.0 et versions ultérieures. Afin d’empêcher par inadvertance mise à jour le package à 3, NuGet 1.4 ajoute la prise en charge pour limiter la plage des versions des packages pouvant être mis à niveau en modifiant manuellement le `packages.config` fichier à l’aide de la nouvelle `allowedVersions` attribut.

Par exemple, l’exemple suivant montre comment verrouiller le `SomePackage` 2.0-3.0 (exclusive) de plage de la version de package.
Le `allowedVersions` attribut accepte les valeurs à l’aide de la [format de plage de version](../reference/package-versioning.md#version-ranges-and-wildcards).

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
    <package id="SomePackage" version="2.1.0" allowedVersions="[2.0, 3.0)" />
</packages>
```

Notez que dans 1.4, un package à une plage de versions spécifiques de verrouillage doit être modifié manuellement. Dans NuGet 1.5 nous prévoyons d’ajouter la prise en charge pour placer cette plage via la `Install-Package` commande.

### <a name="package-visualizer"></a>Visualiseur de package
Le visualiseur de package nouvelle, lancé via le **outils** -> **Library Package Manager** -> **Package visualiseur** vous permet de l’option de menu, Visualisez facilement tous les projets et leurs dépendances de package dans une solution.

_**Remarque importante :** cette fonctionnalité tire parti de la prise en charge du langage DGML dans Visual Studio. Créer la visualisation est uniquement pris en charge dans Visual Studio Ultimate. Affichage d’un diagramme DGML est uniquement pris en charge dans Visual Studio Premium ou supérieur._

![Visualiseur de package](./media/package-visualizer.png)

### <a name="automatic-update-check-for-the-nuget-dialog"></a>Vérification de mise à jour automatique de la boîte de dialogue NuGet
Certaines versions de NuGet introduisent de nouvelles fonctionnalités exprimées par le biais du `.nuspec` fichier qui ne sont pas compris par les versions antérieures de la boîte de dialogue NuGet.
Par exemple, l’introduction dans NuGet 1.4 pour [spécifiant des assemblys de framework](../release-notes/nuget-1.2.md#framework-assembly-refs).
Pour cette raison, il est important d’utiliser la dernière version de NuGet pour vous assurer que vous pouvez en tirant parti des fonctionnalités plus récentes des packages.
Pour afficher les mises à jour à NuGet plus, la boîte de dialogue NuGet contient la logique pour mettre en surbrillance lorsqu’une version plus récente est disponible.

_**Remarque**: la vérification est effectuée uniquement si le **Online** onglet a été sélectionné dans la session active._

![Gérer la boîte de dialogue de Packages NuGet avec la nouvelle version disponible](./media/manage-nuget-packages-update-notification.png)

Pour désactiver la vérification automatique des mises à jour, accédez à la boîte de dialogue de paramètres de NuGet et décochez la case **rechercher automatiquement les mises à jour**.

![Paramètres de NuGet](./media/nuget-settings.png)

Cette fonctionnalité a été ajoutée dans NuGet 1.3, mais il ne serait pas visible, bien sûr, jusqu'à ce que d’une mise à jour 1.3, telles que NuGet 1.4, mise à disposition.

### <a name="package-manager-dialog-improvements"></a>Améliorations de la boîte de dialogue Gestionnaire de package
* **Les noms des menus améliorés**: options de Menu pour lancer la boîte de dialogue ont été renommées pour plus de clarté. L’option de menu est désormais **gérer les Packages NuGet**.
* **Volet d’informations affiche la date la plus récente mise à jour**: NuGet de la boîte de dialogue affiche la date de la dernière mise à jour dans le volet de détails pour un package lorsque le **Online** ou **met à jour** onglet est sélectionné.
* **Liste de balises affichées**: Nuget de la boîte de dialogue affiche des balises.

### <a name="powershell-improvements"></a>Améliorations de PowerShell
* **Les scripts PowerShell signés**: NuGet inclut des scripts Powershell signés l’activation de l’utilisation dans des environnements plus restrictifs.
* **Demander la prise en charge**: la Console du Gestionnaire de Package prend désormais en charge la demande le `$host.ui.Prompt` et `$host.ui.PromptForChoice` commandes.
* **Les noms de Source de package**: fourniture du nom d’une source de package est pris en charge lorsque vous spécifiez une source de package à l’aide de la `-Source` indicateur.

### <a name="nugetexe-command-line-improvements"></a>améliorations de ligne de commande NuGet.exe
* **Commandes personnalisées NuGet**: nuget.exe est extensible à l’aide des commandes personnalisées à l’aide de MEF.
* **Plus simple le flux de travail pour la création de packages de symboles**: le `-Symbols` indicateur peut être appliqué à une structure de dossier normal basé sur une convention création d’un package de symboles en incluant uniquement la source et `.pdb` fichiers dans le dossier.
* **Spécification de plusieurs Sources**: le `NuGet install` commande prend en charge la spécification de plusieurs sources à l’aide de points-virgules comme un délimiteur, ou en spécifiant `-Source` plusieurs fois.
* **Prise en charge de l’authentification proxy**: NuGet 1.4 ajoute la prise en charge de la demande d’informations d’identification utilisateur lors de l’utilisation de NuGet derrière un proxy qui requiert une authentification.
* **mettre à jour les modifications avec rupture de NuGet.exe**: le `-Self` indicateur est désormais requis pour nuget.exe mettre à jour lui-même. `nuget.exe Update` s’applique désormais dans un chemin d’accès à la `packages.config` de fichiers et tente de mettre à jour des packages. Notez que cette mise à jour est limitée dans la mesure où il ne sera pas : ** mettre à jour, ajouter, supprimer le contenu dans le fichier projet.
** D’exécuter des scripts Powershell dans le package.

### <a name="nuget-server-support-for-pushing-packages-using-nugetexe"></a>Prise en charge de NuGet Server pour envoyer des Packages à l’aide de nuget.exe
NuGet inclut une méthode simple pour héberger un [sur le web léger NuGet référentiel](../hosting-packages/nuget-server.md) via la `NuGet.Server` package NuGet. Avec NuGet 1.4, le serveur léger prend en charge l’envoi et la suppression des packages à l’aide de nuget.exe.
La dernière version de `NuGet.Server` ajoute un nouveau `appSetting`, nommé `apiKey`. Lorsque la clé est omise ou est vide, en exécutant un push de packages dans le flux est désactivé. La définition d’apiKey sur une valeur (dans l’idéal, un mot de passe fort) permet des packages push à l’aide de nuget.exe.

```xml
<appSettings>
    <!-- Set the value here to allow people to push/delete packages from the server.
            NOTE: This is a shared key (password) for all users. -->
    <add key="apiKey" value="" />
</appSettings>
```

### <a name="support-for-windows-phone-tools-mango-edition"></a>Prise en charge pour l’édition de Mango outils Windows Phone
NuGet est maintenant pris en charge dans la version release candidate de Windows Phone outils pour Mango.
Actuellement, les outils de Windows Phone n’a pas de prise en charge pour le Gestionnaire d’Extension Visual Studio. ainsi installer les outils NuGet pour Windows Phone, vous devrez peut-être télécharger et exécuter l’extension VSIX manuellement.

Pour désinstaller les outils NuGet pour Windows Phone, exécutez la commande suivante.

     vsixinstaller.exe /uninstall:NuPackToolsVsix.Microsoft.67e54e40-0ae3-42c5-a949-fddf5739e7a5

## <a name="bug-fixes"></a>Correctifs de bogues
NuGet 1.4 avait un total de 88 fixe des éléments de travail. 71 de ceux ont été marquées comme des bogues.

Pour obtenir une liste complète de travail éléments résolus dans NuGet 1.4, veuillez vue le [NuGet Issue Tracker pour cette version](http://nuget.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=NuGet%201.4&assignedTo=All&component=All&sortField=LastUpdatedDate&sortDirection=Descending&page=0).

## <a name="bug-fixes-worth-noting"></a>Correctifs de bogues à noter :

* [Problème 603](http://nuget.codeplex.com/workitem/603): dépendances de Package dans différents dépôts résout correctement lors de la spécification d’une source de package spécifique.
* [Problème 1036](http://nuget.codeplex.com/workitem/1036): ajout de `NuGet Pack SomeProject.csproj` à l’événement post-build n’est plus provoque une boucle infinie.
* [Problème 961](http://nuget.codeplex.com/workitem/961): `-Source` indicateur prend en charge les chemins d’accès relatifs.

## <a name="nuget-14-update"></a>Mise à jour de NuGet 1.4
Peu après la publication de NuGet 1.4, nous avons trouvé quelques problèmes qui ont été important de le corriger.
Le numéro de version spécifique de cette mise à jour 1.4 est 1.4.20615.9020.

### <a name="bug-fixes"></a>Correctifs de bogues
* [Problème 1220](http://nuget.codeplex.com/workitem/1220): Package de mise à jour ne s’exécute pas `install.ps1` / `uninstall.ps1` dans tous les projets lorsqu’il existe plusieurs projets
* [Problème 1156](http://nuget.codeplex.com/workitem/1156): console du Gestionnaire de Package est bloquée sur W2K3/XP (lorsque Powershell 2 n’est pas installé)

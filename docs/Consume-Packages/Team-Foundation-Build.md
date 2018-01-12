---
title: "Procédure pas à pas pour la restauration des packages NuGet avec Team Foundation Build | Microsoft Docs"
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 1/9/2017
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: 3113cccd-35f7-4980-8a6e-fc06556b5064
description: "Procédure pas à pas expliquant comment restaurer des packages avec Team Foundation Build (Visual Studio Team Services et TFS)."
keywords: "Restauration des packages NuGet, NuGet et TFS, NuGet et VSTS, systèmes de génération NuGet, Team Foundation Build, projets MSBuild personnalisés, Cloud Build, intégration continue"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 4be1bb83549958897a15d690439cac073c9683d1
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2017
---
# <a name="setting-up-package-restore-with-team-foundation-build"></a>Configuration de la restauration de packages avec Team Foundation Build

> S’applique aux éléments suivants :
>  - Projets MSBuild personnalisés exécutés sur n’importe quelle version de TFS
>  - Team Foundation Server 2012 ou version antérieure
>  - Modèles de processus Team Foundation Build personnalisés migrés vers TFS 2013 ou version ultérieure
>  - Modèles de processus de génération sans la fonctionnalité de restauration Nuget
>
> Si vous utilisez Visual Studio Team Services ou une instance locale de Team Foundation Server 2013 avec ses modèles de processus de génération, une restauration automatique des packages est effectuée dans le cadre du processus de génération.

Cette section fournit une procédure pas à pas détaillée sur la façon de restaurer les packages dans le cadre de [Team Foundation Build](http://msdn.microsoft.com/library/ms181710(v=VS.90).aspx), pour [git](http://en.wikipedia.org/wiki/Git_(software)) et pour la [gestion de versions Team Foundation](http://msdn.microsoft.com/library/ms181237(v=vs.120).aspx).

Cette procédure pas à pas concerne le scénario d’utilisation de [Team Foundation Service](http://tfs.visualstudio.com/). Toutefois, les mêmes concepts s’appliquent aux autres systèmes de gestion de versions et de build.

## <a name="the-general-approach"></a>Approche générale

L’utilisation de NuGet vous permet d’éviter l’archivage des fichiers binaires dans votre système de gestion de versions.

Ceci est particulièrement intéressant si vous utilisez un [système de gestion de versions distribué](http://en.wikipedia.org/wiki/Distributed_revision_control) tel que git, car les développeurs ont besoin de cloner l’intégralité du référentiel, y compris l’historique complet, avant de pouvoir travailler en local. L’archivage des fichiers binaires peut provoquer l’encombrement du référentiel, puisqu’ils sont généralement stockés sans compression delta.

NuGet prend en charge la [restauration des packages](../consume-packages/package-restore.md) dans le cadre de la génération depuis longtemps. L’implémentation précédente avait un problème du type « l’œuf ou la poule » pour les packages qui devaient étendre le processus de génération, car NuGet restaurait les packages lors de la génération du projet. Toutefois, MSBuild ne permet pas d’étendre la génération pendant la génération. Certains pourraient dire qu’il s’agit d’un problème MSBuild, mais je pense qu’il s’agit plutôt d’un problème inhérent. Selon ce que vous avez besoin d’étendre, vous ne pourrez plus inscrire votre package une fois la restauration effectuée.

Pour remédier à ce problème, la première étape du processus de génération doit être de vérifier que les packages sont restaurés. NuGet 2.7+ facilite cette procédure grâce à une ligne de commande simplifiée :

```
nuget restore path\to\solution.sln
```

Lorsque votre processus de génération restaure des packages avant de générer le code, vous n’avez pas à archiver les fichiers `.targets`.

> [!Note]
> Les packages doivent être associés à leur auteur afin de permettre leur chargement dans Visual Studio. Dans le cas contraire, vous pouvez toujours archiver les fichiers `.targets` pour que les autres développeurs puissent ouvrir la solution sans avoir à restaurer les packages.

Le projet de démonstration suivant montre comment configurer la génération de sorte que les dossiers `packages` et les fichiers `.targets` n’aient pas à être archivés. Il montre également comment configurer une génération automatique dans Team Foundation Service pour cet exemple de projet.

## <a name="repository-structure"></a>Structure du référentiel

Notre projet de démonstration est un outil en ligne de commande simple qui utilise l’argument de ligne de commande pour interroger Bing. Il cible .NET Framework 4 et utilise une grande partie des [packages BCL](http://www.nuget.org/profiles/dotnetframework/) ([Microsoft.Net.Http](http://www.nuget.org/packages/Microsoft.Net.Http), [Microsoft.Bcl](http://www.nuget.org/packages/Microsoft.Bcl), [Microsoft.Bcl.Async](http://www.nuget.org/packages/Microsoft.Bcl.Async) et [Microsoft.Bcl.Build](http://www.nuget.org/packages/Microsoft.Bcl.Build)).

La structure du référentiel se présente ainsi :

    <Project>
        │   .gitignore
        │   .tfignore
        │   build.proj
        │
        ├───src
        │   │   BingSearcher.sln
        │   │
        │   └───BingSearcher
        │       │   App.config
        │       │   BingSearcher.csproj
        │       │   packages.config
        │       │   Program.cs
        │       │
        │       └───Properties
        │               AssemblyInfo.cs
        │
        └───tools
            └───NuGet
                    nuget.exe

Vous pouvez voir que nous n’avons pas archivé le dossier `packages` ni les fichiers `.targets`.

Nous avons, toutefois, archivé le fichier `nuget.exe`, car il est nécessaire pour le processus de génération. En suivant les conventions couramment utilisées, nous l’avons archivé dans un dossier `tools` partagé.

Le code source se trouve dans le dossier `src`. Notre démonstration utilise une seule solution. Toutefois, ce dossier pourrait tout à fait contenir plusieurs solutions.

### <a name="ignore-files"></a>Ignorer les fichiers

> [!Note]
> Il existe actuellement un [bogue connu dans le client NuGet](https://nuget.codeplex.com/workitem/4072), à cause duquel le client continue d’ajouter le dossier `packages` à la gestion de versions. Pour contourner ce problème, désactivez l’intégration du contrôle de code source. Pour ce faire, vous avez besoin d’un fichier `Nuget.Config ` dans le dossier `.nuget` qui soit parallèle à votre solution. Si ce dossier n’existe pas encore, vous devez le créer. Dans [`Nuget.Config`](../consume-packages/configuring-nuget-behavior.md), ajoutez le contenu suivant :

```xml
<configuration>
    <solution>
        <add key="disableSourceControlIntegration" value="true" />
    </solution>
</configuration>
```


Pour informer la gestion de versions que nous n’avons pas l’intention d’archiver les dossiers des **packages**, nous avons également choisi d’ignorer les fichiers pour git (`.gitignore`) et la gestion de versions Team Foundation (`.tfignore`). Ces fichiers décrivent les modèles des fichiers que vous ne souhaitez pas archiver.

Le fichier `.gitignore` se présente ainsi :

    syntax: glob
    *.user
    *.suo
    bin
    obj
    packages

Le fichier `.gitignore` est [très puissant](https://www.kernel.org/pub/software/scm/git/docs/gitignore.html). Par exemple, si vous ne souhaitez pas archiver le contenu du dossier `packages`, mais souhaitez archiver les fichiers `.targets`, vous pouvez utiliser la règle suivante :

    packages
    !packages/**/*.targets

Cela exclut tous les dossiers `packages`, mais inclut de nouveau tous les fichiers `.targets` contenus. Pour obtenir un modèle de fichier `.gitignore` conçu spécialement pour les besoins des développeurs Visual Studio, [cliquez ici](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).

La gestion de versions Team Foundation prend en charge un mécanisme très similaire via le fichier [.tfignore](http://msdn.microsoft.com/library/ms245454.aspx). La syntaxe est pratiquement la même :

    *.user
    *.suo
    bin
    obj
    packages

## <a name="buildproj"></a>build.proj

Pour notre démonstration, nous présentons un processus de génération relativement simple. Nous allons créer un projet MSBuild qui génère toutes les solutions en veillant à ce que les packages soient restaurés avant la génération des solutions.

Ce projet aura trois cibles conventionnelles (`Clean`, `Build` et `Rebuild`), ainsi qu’une nouvelle cible (`RestorePackages`).

- Les cibles `Build` et `Rebuild` dépendent toutes les deux de `RestorePackages`. Vous avez ainsi la garantie de pouvoir exécuter `Build` et `Rebuild`, et de compter sur les packages en cours de restauration.
- `Clean`, `Build` et `Rebuild` appellent la cible MSBuild correspondante dans tous les fichiers solution.
- La cible `RestorePackages` appelle `nuget.exe` pour chaque fichier solution. Pour cela, utilisez la [fonctionnalité de traitement par lots de MSBuild](http://msdn.microsoft.com/library/ms171473.aspx).

Le résultat ressemble à ce qui suit :

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="4.0"
            DefaultTargets="Build"
            xmlns="http://schemas.microsoft.com/developer/msbuild/2003">

    <PropertyGroup>
    <OutDir Condition=" '$(OutDir)'=='' ">$(MSBuildThisFileDirectory)bin\</OutDir>
    <Configuration Condition=" '$(Configuration)'=='' ">Release</Configuration>
    <SourceHome Condition=" '$(SourceHome)'=='' ">$(MSBuildThisFileDirectory)src\</SourceHome>
    <ToolsHome Condition=" '$(ToolsHome)'=='' ">$(MSBuildThisFileDirectory)tools\</ToolsHome>
    </PropertyGroup>

    <ItemGroup>
    <Solution Include="$(SourceHome)*.sln">
        <AdditionalProperties>OutDir=$(OutDir);Configuration=$(Configuration)</AdditionalProperties>
    </Solution>
    </ItemGroup>

    <Target Name="RestorePackages">
    <Exec Command="&quot;$(ToolsHome)NuGet\nuget.exe&quot; restore &quot;%(Solution.Identity)&quot;" />
    </Target>

    <Target Name="Clean">
    <MSBuild Targets="Clean"
                Projects="@(Solution)" />
    </Target>

    <Target Name="Build" DependsOnTargets="RestorePackages">
    <MSBuild Targets="Build"
                Projects="@(Solution)" />
    </Target>

    <Target Name="Rebuild" DependsOnTargets="RestorePackages">
    <MSBuild Targets="Rebuild"
                Projects="@(Solution)" />
    </Target>
</Project>
```

## <a name="configuring-team-build"></a>Configuration de Team Build

Team Build propose divers modèles de processus. Pour cette démonstration, nous utilisons Team Foundation Service. Les installations locales de TFS sont cependant très similaires.

Les modèles Team Build sont différents pour Git et pour la gestion de versions Team Foundation. Par conséquent, les étapes suivantes vont varier selon le système de gestion de versions que vous utilisez. Dans les deux cas, vous avez seulement à sélectionner le fichier build.proj comme le projet que vous souhaitez générer.

Tout d’abord, examinons le modèle de processus pour git. Dans le modèle git, la génération est sélectionnée via la propriété `Solution to build` :

![Processus de génération pour git](media/PackageRestoreTeamBuildGit.png)

Notez que cette propriété est un emplacement de votre référentiel. Étant donné que notre `build.proj` se trouve à la racine, nous avons simplement utilisé `build.proj`. Si vous placez le fichier de build dans un dossier appelé `tools`, la valeur sera `tools\build.proj`.

Dans le modèle de gestion de versions Team Foundation, le projet est sélectionné via la propriété `Projects` :

![Processus de génération pour la gestion de versions Team Foundation](media/PackageRestoreTeamBuildTFVC.png)

Contrairement au modèle git, le modèle de gestion de versions Team Foundation prend en charge les sélecteurs (le bouton situé à droite des trois points). Par conséquent, afin d’éviter les éventuelles erreurs de frappe, nous vous suggérons de les utiliser pour sélectionner le projet.
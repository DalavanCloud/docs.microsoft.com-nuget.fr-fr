---
title: Commande pack de NuGet CLI
description: Référence de la commande pack de nuget.exe
author: karann-msft
ms.author: karann
manager: unnir
ms.date: 01/18/2018
ms.topic: reference
ms.openlocfilehash: 3140d56ac827d932c2323182ad040b8a4d14da5c
ms.sourcegitcommit: 2a6d200012cdb4cbf5ab1264f12fecf9ae12d769
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34818033"
---
# <a name="pack-command-nuget-cli"></a>pack (commande, NuGet CLI)

**S’applique à :** la création de package &bullet; **versions prises en charge :** 2.7 +

Crée un package NuGet selon le `.nuspec` ou le fichier projet. Le `dotnet pack` commande (consultez [dotnet commandes](dotnet-Commands.md)) et `msbuild /t:pack` (consultez [cibles de MSBuild](../reference/msbuild-targets.md)) peut être utilisé en tant que variantes.

> [!Important]
> Création d’un package à partir d’un fichier de projet sous Mono, n’est pas pris en charge. Vous devez également ajuster les chemins d’accès non locale dans le `.nuspec` chemins d’accès de style Unix, le fichier comme nuget.exe ne convertit pas les chemins d’accès Windows lui-même.

## <a name="usage"></a>Utilisation

```cli
nuget pack <nuspecPath | projectPath> [options] [-Properties ...]
```

où `<nuspecPath>` et `<projectPath>` spécifier le `.nuspec` ou le fichier de projet respectivement.

## <a name="options"></a>Options

| Option | Description |
| --- | --- |
| BasePath | Définit le chemin d’accès de base des fichiers définis dans le `.nuspec` fichier. |
| Build | Spécifie que le projet doit être créé avant de générer le package. |
| Exclude | Spécifie un ou plusieurs modèles de caractère générique à exclure lors de la création d’un package. Pour spécifier plusieurs modèles, répétez l’indicateur d’exclusion. Consultez l’exemple ci-dessous. |
| ExcludeEmptyDirectories | Empêche l’inclusion de répertoires vides lors de la création du package. |
| ForceEnglishOutput | *(3.5 +)*  Force nuget.exe pour exécuter à l’aide d’une culture dite indifférente, en anglais. |
| Help | Affiche l’aide de la commande. |
| IncludeReferencedProjects | Indique que le package généré doit inclure les projets référencés en tant que dépendances ou en tant que partie du package. Si un projet référencé est associée à une `.nuspec` fichier ayant le même nom que le projet, puis ce projet référencé est ajouté en tant que dépendance. Sinon, le projet référencé est ajouté en tant que partie du package. |
| MinClientVersion | Définir le *minClientVersion* attribut du package. Cette valeur remplace la valeur existants *minClientVersion* d’attribut (le cas échéant) dans le `.nuspec` fichier. |
| MSBuildPath | *(4.0 +)*  Spécifie le chemin d’accès de MSBuild à utiliser avec la commande prioritaire `-MSBuildVersion`. |
| MSBuildVersion | *(3.2 +)*  Spécifie la version de MSBuild à utiliser avec cette commande. Valeurs prises en charge sont 4, 12, 14, 15. Par défaut que le MSBuild dans votre chemin d’accès est sélectionné, sinon la valeur par défaut pour la version installée la plus élevée de MSBuild. |
| NoDefaultExcludes | Empêche l’exclusion par défaut de NuGet package des fichiers et dossiers commençant par un point, tel que `.svn` et `.gitignore`. |
| NoPackageAnalysis | Spécifie que le pack ne doit pas exécuter d’analyse du package après sa génération. |
| OutputDirectory | Spécifie le dossier dans lequel est stocké le package créé. Si aucun dossier n’est spécifié, le dossier actif est utilisé. |
| Properties | Doit apparaître en dernier dans la ligne de commande après les autres options. Spécifie une liste de propriétés qui remplacent les valeurs dans le fichier projet ; consultez [propriétés communes des projets MSBuild](/visualstudio/msbuild/common-msbuild-project-properties) des noms de propriétés. L’argument des propriétés est une liste de jeton de paires = valeur, séparés par des points-virgules, où chaque occurrence de `$token$` dans le `.nuspec` fichier est remplacé par la valeur donnée. Les valeurs peuvent être des chaînes entre guillemets. Notez que la propriété « Configuration », la valeur par défaut est « Debug ». Pour attribuer une configuration Release, utilisez `-Properties Configuration=Release`. |
| Suffix | *(3.4.4+)*  Ajoute un suffixe au numéro de version généré en interne, généralement utilisé pour l’ajout de la build ou autres identificateurs de version préliminaire. Par exemple, à l’aide de `-suffix nightly` va créer un package avec un type de numéro de version `1.2.3-nightly`. Suffixes doivent commencer par une lettre pour éviter les avertissements, les erreurs et identifier les éventuelles incompatibilités avec différentes versions de NuGet et le Gestionnaire de Package NuGet. |
| Symbols | Spécifie que le package contient des sources et des symboles. Lorsqu’il est utilisé avec un `.nuspec` fichier, cette opération crée un fichier de package NuGet normal et le package de symboles correspondants. |
| Tool | Spécifie que les fichiers de sortie du projet doivent être placés dans le `tool` dossier. |
| Verbosity | Spécifie la quantité de détails affichés dans la sortie : *normal*, *silencieux*, *détaillées*. |
| Version | Remplace le numéro de version à partir de la `.nuspec` fichier. |

Consultez également [variables d’environnement](cli-ref-environment-variables.md)

## <a name="excluding-development-dependencies"></a>À l’exclusion des dépendances de développement

Certains packages NuGet sont utiles en tant que dépendances de développement, ce qui vous aident à créer votre propre bibliothèque, mais ne sont pas forcément nécessaires en tant que dépendances de package réel.

Le `pack` commande ignorera `package` entrées dans `packages.config` qui ont le `developmentDependency` attribut la valeur `true`. Ces entrées n’inclura pas comme un dépendances dans le package créé.

Par exemple, considérez les éléments suivants `packages.config` fichier dans le projet source :

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
    <package id="jQuery" version="1.5.2" />
    <package id="netfx-Guard" version="1.3.3.2" developmentDependency="true" />
    <package id="microsoft-web-helpers" version="1.15" />
</packages>
```

Pour ce projet, le package créé par `nuget pack` a une dépendance sur `jQuery` et `microsoft-web-helpers` mais pas `netfx-Guard`.

## <a name="examples"></a>Exemples

```cli
nuget pack

nuget pack foo.nuspec

nuget pack foo.csproj

nuget pack foo.csproj -Properties Configuration=Release

nuget pack foo.csproj -Build -Symbols -Properties owners=janedoe,xiaop;version="1.0.5"

# Create a package from project foo.csproj, using MSBuild version 12 to build the project
nuget pack foo.csproj -Build -Symbols -MSBuildVersion 12 -Properties owners=janedoe,xiaop;version="1.0.5

nuget pack foo.nuspec -Version 2.1.0

nuget pack foo.nuspec -Version 1.0.0 -MinClientVersion 2.5

nuget pack Package.nuspec -exclude "*.exe" -exclude "*.bat"
```

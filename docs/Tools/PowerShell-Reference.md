---
title: "Référence de PowerShell NuGet | Documents Microsoft"
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 10/2/2017
ms.topic: reference
ms.prod: nuget
ms.technology: 
ms.assetid: cd08b869-44c6-480e-90f7-494a6d08e6d0
description: "La référence complète de commandes PowerShell disponibles dans la Console du Gestionnaire de Package NuGet dans Visual Studio."
keywords: "NuGet package manager console, commandes Powershell de NuGet, référence NuGet Powershell"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: c0da5c88447784fdd49d824bbd03b11f73c22ebc
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2017
---
# <a name="powershell-reference"></a>Référence de PowerShell

La Console du Gestionnaire de Package fournit une interface de PowerShell dans Visual Studio sous Windows pour interagir avec NuGet via les commandes spécifiques répertoriés ci-dessous. (La console n’est pas actuellement disponible dans Visual Studio pour Mac.) Pour un guide d’utilisation de la console, consultez le [Package Manager Console](../tools/package-manager-console.md) rubrique.

> [!Tip]
> Toutes les commandes PowerShell se rapportent uniquement à la consommation du package. Aucune commande de PowerShell se rapportent à la création et la publication des packages à l’exception dans la mesure où un package peut également être un consommateur d’autres packages.

> [!Important]
> Les commandes répertoriées ici sont spécifiques à la Console du Gestionnaire de Package dans Visual Studio et diffèrent le [les commandes de module de gestion des packages](https://msdn.microsoft.com/powershell/reference/6/packagemanagement/packagemanagement) qui sont disponibles dans un environnement PowerShell général. Plus précisément, chaque environnement dispose des commandes qui ne sont pas disponibles dans les autres, et les commandes portant le même nom peuvent également différer dans leurs arguments spécifiques. Lorsque vous utilisez la Console de gestion de Package dans Visual Studio, les commandes et les arguments documentées dans cette rubrique s’appliquent.

| Commandes courantes | Description | Version de NuGet |
| --- | --- | --- |
| [Package d’installation](ps-ref-install-package.md) | Installe un package et ses dépendances dans le projet. | Tout |
| [Package de mise à jour](ps-ref-update-package.md) | Met à jour un package et ses dépendances, tous les packages dans un projet. | Tout |
| [Find-Package](ps-ref-find-package.md) | Recherche dans une source de package à l’aide de mots clés ou un ID de package. | 3.0+ |
| [Get-Package](ps-ref-get-package.md) | Récupère la liste des packages installés dans le référentiel local, ou répertorie les packages disponibles à partir d’une source de package. | Tout |

| Commandes secondaires | Description | Version de NuGet |
| --- | --- | --- |
| [BindingRedirect ajouter](ps-ref-add-bindingredirect.md) | Examine tous les assemblys du chemin de sortie pour un projet et ajoute des redirections de liaison dans le `app.config` ou `web.config` lorsque cela est nécessaire. | Tout |
| [Get-projet](ps-ref-get-project.md) | Affiche des informations sur la valeur par défaut ou le projet spécifié. | 3.0+ |
| [Ouvrir-PackagePage](ps-ref-open-packagepage.md) | Lance le navigateur par défaut avec le projet, les licences ou les URL d’abus de rapport pour le package spécifié. | Déconseillé dans 3.0 + |
| [Register-TabExpansion](ps-ref-register-tabexpansion.md) | Inscrit une tabulation pour les paramètres d’une commande, ce qui vous permet de créer des expansions personnalisées pour les valeurs de paramètre de couramment utilisés. | Tout |
| [Package de synchronisation](ps-ref-sync-package.md) | Obtenir la version d’installée package à partir de spécifié du projet et synchronise la version pour les autres projets dans la solution. | 3.0+ |
| [Uninstall-Package](ps-ref-uninstall-package.md) | Supprime un package à partir d’un projet, et éventuellement de supprimer ses dépendances. | Tout |

Pour une aide complète et détaillée sur un de ces commandes dans la console, exécutez simplement la commande suivante avec le nom de la commande en question :

```ps
Get-Help <command> -full
```

Toutes les commandes de Console du Gestionnaire de Package prend en charge les éléments suivants [les paramètres PowerShell communs](http://go.microsoft.com/fwlink/?LinkID=113216):

- Déboguer
- ErrorAction
- ErrorVariable
- OutBuffer
- OutVariable
- PipelineVariable
- Verbose
- WarningAction
- WarningVariable

Pour plus d’informations, reportez-vous à [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216) dans la documentation PowerShell.
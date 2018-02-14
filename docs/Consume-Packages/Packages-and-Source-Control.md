---
title: "Packages NuGet et contrôle de code source | Microsoft Docs"
author: kraigb
ms.author: kraigb
manager: ghogen
ms.date: 07/17/2017
ms.topic: article
ms.prod: nuget
ms.technology: 
description: "Informations sur le traitement des packages NuGet dans les systèmes de contrôle de code source et de gestion de versions, et sur l’omission de packages avec git et TFVC."
keywords: "Contrôle de code source NuGet, gestion de versions NuGet, NuGet et git, NuGet et TFS, NuGet et TFVC, omission de packages, référentiels de contrôle de code source, référentiels de gestion de versions"
ms.reviewer:
- karann-msft
- unniravindranathan
ms.openlocfilehash: 6261625d5d7eaa748f9ad15510b7b2af3c814e44
ms.sourcegitcommit: 4651b16a3a08f6711669fc4577f5d63b600f8f58
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2018
---
# <a name="omitting-nuget-packages-in-source-control-systems"></a><span data-ttu-id="8511a-104">Omission de packages NuGet dans les systèmes de contrôle de code source</span><span class="sxs-lookup"><span data-stu-id="8511a-104">Omitting NuGet packages in source control systems</span></span>

<span data-ttu-id="8511a-105">En règle générale, les développeurs omettent les packages NuGet de leurs référentiels de contrôle de code source et se servent de la [restauration de package](../consume-packages/package-restore.md) pour réinstaller les dépendances d’un projet avant la génération.</span><span class="sxs-lookup"><span data-stu-id="8511a-105">Developers typically omit NuGet packages from their source control repositories and rely instead on [package restore](../consume-packages/package-restore.md) to reinstall a project's dependencies before a build.</span></span>

<span data-ttu-id="8511a-106">Les raisons pour lesquelles ils se servent de la restauration de package sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="8511a-106">The reasons for relying on package restore include the following:</span></span>

1. <span data-ttu-id="8511a-107">Les systèmes de gestion de versions distribués, tels que Git, incluent des copies complètes de chacune des versions des fichiers du référentiel.</span><span class="sxs-lookup"><span data-stu-id="8511a-107">Distributed version control systems, such as Git, include full copies of every version of every file within the repository.</span></span> <span data-ttu-id="8511a-108">Les fichiers binaires qui sont fréquemment mis à jour entraînent un encombrement important et ralentissent considérablement le clonage du référentiel.</span><span class="sxs-lookup"><span data-stu-id="8511a-108">Binary files that are frequently updated lead to significant bloat and lengthens the time it takes to clone the repository.</span></span>
1. <span data-ttu-id="8511a-109">Lorsque des packages sont inclus dans le référentiel, il peut arriver que les développeurs ajoutent des références directement au contenu de package présent sur le disque, plutôt que de référencer les packages via NuGet, ce qui peut avoir pour résultat des noms de chemins codés en dur dans le projet.</span><span class="sxs-lookup"><span data-stu-id="8511a-109">When packages are included in the repository, developers are liable to add references directly to package contents on disk rather than referencing packages through NuGet, which can lead to hard-coded path names in the project.</span></span>
1. <span data-ttu-id="8511a-110">Il est alors plus difficile de nettoyer votre solution de tous les dossiers de package inutilisés, car vous devez faire attention à ne pas supprimer les dossiers de package qui sont en cours d’utilisation.</span><span class="sxs-lookup"><span data-stu-id="8511a-110">It becomes harder to clean your solution of any unused package folders, as you need to ensure you don't delete any package folders still in use.</span></span>
1. <span data-ttu-id="8511a-111">L’omission de packages vous permet de définir des limites de propriété claires entre votre code et les packages des autres développeurs dont dépend votre code.</span><span class="sxs-lookup"><span data-stu-id="8511a-111">By omitting packages, you maintain clean boundaries of ownership between your code and the packages from others that you depend upon.</span></span> <span data-ttu-id="8511a-112">De nombreux packages NuGet sont déjà gérés dans leur propre référentiel de contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="8511a-112">Many NuGet packages are maintained in their own source control repositories already.</span></span>

<span data-ttu-id="8511a-113">Même si la restauration de packages constitue le comportement par défaut de NuGet, certaines opérations manuelles sont nécessaires pour omettre les packages (c’est-à-dire le dossier `packages` de votre projet) du contrôle de code source, comme le décrivent les sections suivantes.</span><span class="sxs-lookup"><span data-stu-id="8511a-113">Although package restore is the default behavior with NuGet, some manual work is necessary to omit packages&mdash;namely, the `packages` folder in your project&mdash;from source control, as described in the following sections.</span></span>

## <a name="omitting-packages-with-git"></a><span data-ttu-id="8511a-114">Omission de packages avec Git</span><span class="sxs-lookup"><span data-stu-id="8511a-114">Omitting packages with Git</span></span>

<span data-ttu-id="8511a-115">Utilisez le [fichier .gitignore](https://git-scm.com/docs/gitignore) pour ne pas inclure le dossier `packages` dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="8511a-115">Use the [.gitignore file](https://git-scm.com/docs/gitignore) to avoid including the `packages` folder in source control.</span></span> <span data-ttu-id="8511a-116">Pour référence, consultez [l’exemple `.gitignore` pour projets Visual Studio](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).</span><span class="sxs-lookup"><span data-stu-id="8511a-116">For reference, see the [sample `.gitignore` for Visual Studio projects](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).</span></span>

<span data-ttu-id="8511a-117">Les parties importantes du fichier `.gitignore` sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="8511a-117">The important parts of the `.gitignore` file are:</span></span>

```gitignore
# Ignore NuGet Packages
*.nupkg

# Ignore the packages folder
**/packages/*

# Include packages/build/, which is used as an MSBuild target
!**/packages/build/

# Uncomment if necessary; generally it's regenerated when needed
#!**/packages/repositories.config

# Ignore other intermediate files that NuGet might create. project.lock.json is used in conjunction
# with project.json; project.assets.json is used in conjunction with the PackageReference format.
project.lock.json
project.assets.json
*.nuget.props
```

## <a name="omitting-packages-with-team-foundation-version-control"></a><span data-ttu-id="8511a-118">Omission de packages avec Team Foundation Version Control</span><span class="sxs-lookup"><span data-stu-id="8511a-118">Omitting packages with Team Foundation Version Control</span></span>

> [!Note]
> <span data-ttu-id="8511a-119">Si possible, suivez ces instructions *avant* d’ajouter votre projet au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="8511a-119">Follow these instructions if possible *before* adding your project to source control.</span></span> <span data-ttu-id="8511a-120">Sinon, supprimez manuellement le dossier `packages` de votre référentiel, puis archivez ce changement avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="8511a-120">Otherwise, manually delete the `packages` folder from your repository and check in that change before continuing.</span></span>

<span data-ttu-id="8511a-121">Pour désactiver l’intégration du contrôle de code source à TFVC pour les fichiers sélectionnés :</span><span class="sxs-lookup"><span data-stu-id="8511a-121">To disable source control integration with TFVC for selected files:</span></span>

1. <span data-ttu-id="8511a-122">Créez un dossier appelé `.nuget` dans le dossier de votre solution (où se trouve le fichier `.sln`).</span><span class="sxs-lookup"><span data-stu-id="8511a-122">Create a folder called `.nuget` in your solution folder (where the `.sln` file is).</span></span>
    - <span data-ttu-id="8511a-123">Conseil : sur les systèmes Windows, pour créer ce dossier dans l’Explorateur Windows, utilisez le nom `.nuget.` *avec* le point de fin.</span><span class="sxs-lookup"><span data-stu-id="8511a-123">Tip: on Windows, to create this folder in Windows Explorer, use the name `.nuget.` *with* the trailing dot.</span></span>

1. <span data-ttu-id="8511a-124">Dans ce dossier, créez un fichier nommé `NuGet.Config` et ouvrez-le pour le modifier.</span><span class="sxs-lookup"><span data-stu-id="8511a-124">In that folder, create a file named `NuGet.Config` and open it for editing.</span></span>

1. <span data-ttu-id="8511a-125">Ajoutez au minimum le texte suivant, où le paramètre [disableSourceControlIntegration](../reference/nuget-config-file.md#solution-section) indique à Visual Studio d’ignorer tous les éléments du dossier `packages` :</span><span class="sxs-lookup"><span data-stu-id="8511a-125">Add the following text as a minimum, where the [disableSourceControlIntegration](../reference/nuget-config-file.md#solution-section) setting instructs Visual Studio to skip everything in the `packages` folder:</span></span>

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <configuration>
       <solution>
           <add key="disableSourceControlIntegration" value="true" />
       </solution>
   </configuration>
   ```

1. <span data-ttu-id="8511a-126">Si vous utilisez TFS 2010 ou version antérieure, masquez le dossier `packages` dans vos mappages d’espace de travail.</span><span class="sxs-lookup"><span data-stu-id="8511a-126">If you are using TFS 2010 or earlier, cloak the `packages` folder in your workspace mappings.</span></span>

1. <span data-ttu-id="8511a-127">Dans TFS 2012 ou ultérieur, ou avec Visual Studio Team Services, créez un fichier `.tfignore`, comme décrit dans [Ajouter des fichiers sur le serveur](https://www.visualstudio.com/en-us/docs/tfvc/add-files-server#tfignore).</span><span class="sxs-lookup"><span data-stu-id="8511a-127">On TFS 2012 or later, or with Visual Studio Team Services, create a `.tfignore` file as described on [AddFiles to the Server](https://www.visualstudio.com/en-us/docs/tfvc/add-files-server#tfignore).</span></span> <span data-ttu-id="8511a-128">Dans ce fichier, ajoutez le contenu ci-dessous pour ignorer explicitement les modifications apportées au dossier `\packages` au niveau du référentiel, ainsi qu’à quelques autres fichiers intermédiaires.</span><span class="sxs-lookup"><span data-stu-id="8511a-128">In that file, include the content below to explicitly ignore modifications to the `\packages` folder on the repository level and a few other intermediate files.</span></span> <span data-ttu-id="8511a-129">Vous pouvez créer le fichier dans l’Explorateur Windows en utilisant le nom `.tfignore.` avec le point de fin. Toutefois, vous devrez peut-être désactiver l’option « Cacher les extensions de fichiers » avant de procéder :</span><span class="sxs-lookup"><span data-stu-id="8511a-129">(You can create the file in Windows Explorer using the name a `.tfignore.` with the trailing dot, but you might need to disable the "Hide known file extensions" option first.):</span></span>

   ```cli
   # Ignore NuGet Packages
   *.nupkg

   # Ignore the NuGet packages folder in the root of the repository. If needed, prefix 'packages'
   # with additional folder names if it's not in the same folder as .tfignore.   
   packages

   # Include package target files which may be required for MSBuild, again prefixing the folder name as needed.
   !packages/*.targets

   # Omit temporary files
   project.lock.json
   project.assets.json
   *.nuget.props
   ```

1. <span data-ttu-id="8511a-130">Ajoutez `NuGet.Config` et `.tfignore` pour réaliser le contrôle de code source et l’archivage de vos modifications.</span><span class="sxs-lookup"><span data-stu-id="8511a-130">Add `NuGet.Config` and `.tfignore` to source control and check in your changes.</span></span>
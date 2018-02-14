---
title: Notes de publication de NuGet 4.3 RTM | Microsoft Docs
author: karann-msft
ms.author: karann-msft
manager: unniravindranathan
ms.date: 08/14/2017
ms.topic: article
ms.prod: nuget
ms.technology: 
ms.assetid: da3bf363-4d9d-446c-b91d-41c4cf6e16a1
description: "Notes de publication pour NuGet 4.3 RTM, avec notamment les problèmes connus, les correctifs de bogues, les fonctionnalités ajoutées et les DCR."
keywords: "notes de publication de NuGet 4.3 RTM, correctifs de bogues, problèmes connus, fonctionnalités ajoutées, DCR"
ms.reviewer:
- karann-msft
- unniravindranathan
- anangaur
ms.openlocfilehash: d237c4a29d8a76bba10ff9bb641f2e06329c07a6
ms.sourcegitcommit: d0ba99bfe019b779b75731bafdca8a37e35ef0d9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2017
---
# <a name="43-rtm-release-notes"></a><span data-ttu-id="80954-104">Notes de publication de la version 4.3 RTM</span><span class="sxs-lookup"><span data-stu-id="80954-104">4.3 RTM Release Notes</span></span>

<span data-ttu-id="80954-105">[Visual Studio 2017 15.3 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) est fourni avec NuGet 4.3 RTM, qui ajoute la prise en charge de nouveaux scénarios tels que .NET Standard 2.0/.NET Core 2.0, contient de nombreux correctifs de qualité et améliore les performances.</span><span class="sxs-lookup"><span data-stu-id="80954-105">[Visual Studio 2017 15.3 RTW](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes) comes with NuGet 4.3 RTM which adds support for new scenarios such as .NET Standard 2.0/.NET Core 2.0, contains many quality fixes, and improves performance.</span></span> <span data-ttu-id="80954-106">Cette version offre également plusieurs améliorations comme la prise en charge de la Gestion sémantique de version 2.0.0, l’intégration MSBuild des avertissements et erreurs NuGet, et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="80954-106">This release also brings several improvements like support for Semantic Versioning 2.0.0, MSBuild integration of NuGet warnings and errors, and more.</span></span>

## <a name="known-issues"></a><span data-ttu-id="80954-107">Problèmes connus</span><span class="sxs-lookup"><span data-stu-id="80954-107">Known issues</span></span>

### <a name="nuget-restore-may-treat-disabled-package-sources-as-enabled-in-some-cases"></a><span data-ttu-id="80954-108">La restauration NuGet peut dans certains cas traiter des sources de packages désactivées comme étant activées</span><span class="sxs-lookup"><span data-stu-id="80954-108">NuGet restore may treat disabled package sources as enabled in some cases</span></span>

#### <a name="issue"></a><span data-ttu-id="80954-109">Problème :</span><span class="sxs-lookup"><span data-stu-id="80954-109">Issue:</span></span>
<span data-ttu-id="80954-110">Les techniques des lignes de commande de restauration suivantes traitent les sources de packages désactivées comme étant activées.</span><span class="sxs-lookup"><span data-stu-id="80954-110">The following restore command-line techniques treat disabled packages sources as enabled.</span></span> [<span data-ttu-id="80954-111">NuGet#5704</span><span class="sxs-lookup"><span data-stu-id="80954-111">NuGet#5704</span></span>](https://github.com/NuGet/Home/issues/5704)
- `msbuild /t:restore`
- <span data-ttu-id="80954-112">`dotnet restore` (concerne le dotnet.exe fourni avec Visual Studio ou la version livrée avec le SDK NetCore 2.0.0)</span><span class="sxs-lookup"><span data-stu-id="80954-112">`dotnet restore` (either with dotnet.exe that ships with VS, or the one that comes with NetCore SDK 2.0.0)</span></span>

#### <a name="workaround"></a><span data-ttu-id="80954-113">Solution de contournement :</span><span class="sxs-lookup"><span data-stu-id="80954-113">Workaround:</span></span>
1. <span data-ttu-id="80954-114">Utilisez Visual Studio (2017 15.3 ou ultérieur) ou NuGet.exe (v4.3.0 ou ultérieur)</span><span class="sxs-lookup"><span data-stu-id="80954-114">Use Visual Studio (2017 15.3 or later) or NuGet.exe (v4.3.0 or later)</span></span>
1. <span data-ttu-id="80954-115">Supprimez votre source désactivée, puis continuez à utiliser msbuild ou dotnet.exe.</span><span class="sxs-lookup"><span data-stu-id="80954-115">Delete your disabled source and continue to use msbuild or dotnet.exe.</span></span>
1. <span data-ttu-id="80954-116">Pour votre solution, vous pouvez utiliser « Clear » dans NuGet.config puis définir les sources nécessaires pour cette solution.</span><span class="sxs-lookup"><span data-stu-id="80954-116">For your solution, you could use "Clear" in NuGet.config and then define the sources necessary for that solution.</span></span>

### <a name="while-using-package-manager-console-enter-key-may-not-work"></a><span data-ttu-id="80954-117">Lors de l’utilisation de la Console du Gestionnaire de Package, la touche « Entrée » peut ne pas fonctionner</span><span class="sxs-lookup"><span data-stu-id="80954-117">While using Package Manager Console, 'Enter' key may not work</span></span>

#### <a name="issue"></a><span data-ttu-id="80954-118">Problème :</span><span class="sxs-lookup"><span data-stu-id="80954-118">Issue:</span></span>
<span data-ttu-id="80954-119">Parfois, la touche Entrée ne fonctionne pas dans la Console du Gestionnaire de Package.</span><span class="sxs-lookup"><span data-stu-id="80954-119">Occasionally, the enter key does not work in the Package Manager Console.</span></span> <span data-ttu-id="80954-120">Si cela se produit, vérifiez l’évolution du correctif et spécifiez les éventuelles informations supplémentaires utiles dans les étapes de reproduction du problème.</span><span class="sxs-lookup"><span data-stu-id="80954-120">If you see this, please check out the progress on the fix, and provide any additional helpful information about your repro steps.</span></span> <span data-ttu-id="80954-121">[NuGet#4204](https://github.com/NuGet/Home/issues/4204) [NuGet#4570](https://github.com/NuGet/Home/issues/4570)</span><span class="sxs-lookup"><span data-stu-id="80954-121">[NuGet#4204](https://github.com/NuGet/Home/issues/4204) [NuGet#4570](https://github.com/NuGet/Home/issues/4570)</span></span>

#### <a name="workaround"></a><span data-ttu-id="80954-122">Solution de contournement :</span><span class="sxs-lookup"><span data-stu-id="80954-122">Workaround:</span></span>
<span data-ttu-id="80954-123">Redémarrez Visual Studio et ouvrez la console de gestion des packages avant d’ouvrir la solution.</span><span class="sxs-lookup"><span data-stu-id="80954-123">Restart Visual Studio and open the PMC before opening the solution.</span></span> <span data-ttu-id="80954-124">Vous pouvez aussi tenter de supprimer le `project.lock.json` et de réeffectuer la restauration.</span><span class="sxs-lookup"><span data-stu-id="80954-124">Alternatively, try deleting the `project.lock.json` and restoring again.</span></span>

### <a name="you-will-be-unable-to-view-add-or-update-dotnetclitools-using-nuget-package-manager"></a><span data-ttu-id="80954-125">Vous ne pouvez pas afficher, ajouter ou mettre à jour DotNetCLITools à l’aide du Gestionnaire de package Nuget</span><span class="sxs-lookup"><span data-stu-id="80954-125">You will be unable to view, add, or update DotNetCLITools, using Nuget Package Manager</span></span>

#### <a name="issue"></a><span data-ttu-id="80954-126">Problème :</span><span class="sxs-lookup"><span data-stu-id="80954-126">Issue:</span></span>
<span data-ttu-id="80954-127">Le Gestionnaire de package NuGet ne s’affiche pas et n’autorise pas l’ajout/mise à jour de DotNetCLITools.</span><span class="sxs-lookup"><span data-stu-id="80954-127">NuGet Package Manager does not display and does not allow add/update of DotNetCLITools.</span></span> [<span data-ttu-id="80954-128">NuGet#4256</span><span class="sxs-lookup"><span data-stu-id="80954-128">NuGet#4256</span></span>](https://github.com/NuGet/Home/issues/4256)

#### <a name="workaround"></a><span data-ttu-id="80954-129">Solution de contournement :</span><span class="sxs-lookup"><span data-stu-id="80954-129">Workaround:</span></span>
<span data-ttu-id="80954-130">Vous devez modifier manuellement DotNetCLIToolReferences dans votre fichier projet.</span><span class="sxs-lookup"><span data-stu-id="80954-130">DotNetCLIToolReferences must be manually edited in your project file.</span></span>

### <a name="retargeting-target-framework-version-may-lead-to-incomplete-intellisense"></a><span data-ttu-id="80954-131">Le reciblage de la version du framework cible peut générer des informations Intellisense incomplètes</span><span class="sxs-lookup"><span data-stu-id="80954-131">Retargeting target framework version may lead to incomplete Intellisense</span></span>

#### <a name="issue"></a><span data-ttu-id="80954-132">Problème :</span><span class="sxs-lookup"><span data-stu-id="80954-132">Issue:</span></span>
<span data-ttu-id="80954-133">Le reciblage de la version du framework cible peut générer des informations Intellisense incomplètes dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="80954-133">Retargeting target framework version may lead to incomplete Intellisense, in Visual Studio.</span></span> <span data-ttu-id="80954-134">Cela se produit quand vous utilisez PackageReferences comme format de gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="80954-134">This happens when you are using PackageReferences as the package manager format.</span></span> [<span data-ttu-id="80954-135">NuGet#4216</span><span class="sxs-lookup"><span data-stu-id="80954-135">NuGet#4216</span></span>](https://github.com/NuGet/Home/issues/4216)

#### <a name="workaround"></a><span data-ttu-id="80954-136">Solution de contournement :</span><span class="sxs-lookup"><span data-stu-id="80954-136">Workaround:</span></span>
<span data-ttu-id="80954-137">Effectuez une restauration manuelle.</span><span class="sxs-lookup"><span data-stu-id="80954-137">Do a manual restore.</span></span>


## <a name="issues-fixed-in-nuget-43-rtm-timeframe"></a><span data-ttu-id="80954-138">Problèmes résolus dans NuGet 4.3 RTM</span><span class="sxs-lookup"><span data-stu-id="80954-138">Issues fixed in NuGet 4.3 RTM timeframe</span></span>

<span data-ttu-id="80954-139">[Notes de publication de NuGet 4.0 RTM](../release-notes/nuget-4.0-RTM.md) : répertorie tous les problèmes résolus dans NuGet 4.0 RTM</span><span class="sxs-lookup"><span data-stu-id="80954-139">[NuGet 4.0 RTM Release Notes](../release-notes/nuget-4.0-RTM.md) - Lists all the issues fixed for NuGet 4.0 RTM</span></span>

<span data-ttu-id="80954-140">**Fonctionnalités :**</span><span class="sxs-lookup"><span data-stu-id="80954-140">**Feature:**</span></span>

* <span data-ttu-id="80954-141">Amélioration des performances de restauration NuGet - Implémentation de NoOp plus intelligent pour les restaurations sur la ligne de commande et VS - [#5080](https://github.com/NuGet/Home/issues/5080)</span><span class="sxs-lookup"><span data-stu-id="80954-141">Improve NuGet Restore Perf - Implement smarter NoOp for command line restores and VS - [#5080](https://github.com/NuGet/Home/issues/5080)</span></span>

* <span data-ttu-id="80954-142">NET Core 2.0 : l’interface de ligne de commande VS/Dotnet doit commencer à utiliser les fonctionnalités existantes de NuGet : dossiers de secours - [#4939](https://github.com/NuGet/Home/issues/4939)</span><span class="sxs-lookup"><span data-stu-id="80954-142">NET Core 2.0: VS/Dotnet CLI should start using existing NuGet functionality: FallBack folders - [#4939](https://github.com/NuGet/Home/issues/4939)</span></span>

* <span data-ttu-id="80954-143">NET Core 2.0 : Permet aux utilisateurs d’ignorer des avertissements de restauration spécifiques (ou de les promouvoir en erreur) - [#4898](https://github.com/NuGet/Home/issues/4898)</span><span class="sxs-lookup"><span data-stu-id="80954-143">NET Core 2.0: Enable users to ignore specific restore warnings (or elevate to error) - [#4898](https://github.com/NuGet/Home/issues/4898)</span></span>

* <span data-ttu-id="80954-144">NET Core 2.0 : assemblys CLI localisés - [#4896](https://github.com/NuGet/Home/issues/4896)</span><span class="sxs-lookup"><span data-stu-id="80954-144">NET Core 2.0: CLI localized assemblies - [#4896](https://github.com/NuGet/Home/issues/4896)</span></span>

* <span data-ttu-id="80954-145">NET Core 2.0 : inscription de tous les avertissements/erreurs dans le fichier de ressources (notamment PackageTargetFallback) - [#4895](https://github.com/NuGet/Home/issues/4895)</span><span class="sxs-lookup"><span data-stu-id="80954-145">NET Core 2.0: register all warnings/errors to assets file (including PackageTargetFallback) - [#4895](https://github.com/NuGet/Home/issues/4895)</span></span>

* <span data-ttu-id="80954-146">Activation de la prise en charge TFM : NetStandard2.0, Tizen - [#4892](https://github.com/NuGet/Home/issues/4892)</span><span class="sxs-lookup"><span data-stu-id="80954-146">Enable TFM support: NetStandard2.0, Tizen - [#4892](https://github.com/NuGet/Home/issues/4892)</span></span>

* <span data-ttu-id="80954-147">Réduction du nombre de projets NuGet.Core et NuGet.Client (et donc de DLL) - [#2446](https://github.com/NuGet/Home/issues/2446)</span><span class="sxs-lookup"><span data-stu-id="80954-147">Reduce the number of NuGet.Core and NuGet.Client projects (and thus DLLs) - [#2446](https://github.com/NuGet/Home/issues/2446)</span></span>

* <span data-ttu-id="80954-148">Ajout de la possibilité de marquer des avertissements nuget comme des erreurs - [#2395](https://github.com/NuGet/Home/issues/2395)</span><span class="sxs-lookup"><span data-stu-id="80954-148">Add ability to mark nuget warnings as errors - [#2395](https://github.com/NuGet/Home/issues/2395)</span></span>


<span data-ttu-id="80954-149">**Bogue :**</span><span class="sxs-lookup"><span data-stu-id="80954-149">**Bug:**</span></span>

* <span data-ttu-id="80954-150">msbuild /t:pack échoue avec l’erreur Le paramètre « DevelopmentDependency » n’est pas pris en charge par la tâche « PackTask » - [#5584](https://github.com/NuGet/Home/issues/5584)</span><span class="sxs-lookup"><span data-stu-id="80954-150">msbuild /t:pack fails with The "DevelopmentDependency" parameter is not supported by the "PackTask" task - [#5584](https://github.com/NuGet/Home/issues/5584)</span></span>

* <span data-ttu-id="80954-151">La structure de répertoires des fichiers de contenu est aplatie si vous n’ajoutez pas de séparateur de répertoire Windows à la fin de PackagePath - [#4795](https://github.com/NuGet/Home/issues/4795)</span><span class="sxs-lookup"><span data-stu-id="80954-151">Directory structure for content files flattened if not adding Windows directory separator at the end of PackagePath - [#4795](https://github.com/NuGet/Home/issues/4795)</span></span>

* <span data-ttu-id="80954-152">Les projets netcore ne prennent pas en charge le paramétrage en tant que developmentDependency - [#4694](https://github.com/NuGet/Home/issues/4694)</span><span class="sxs-lookup"><span data-stu-id="80954-152">netcore projects don't support setting as developmentDependency - [#4694](https://github.com/NuGet/Home/issues/4694)</span></span>

* <span data-ttu-id="80954-153">RestoreManagerPackage chargé de façon synchrone, ce qui bloquait le thread d’interface utilisateur et provoquait un interblocage de VS - [#4679](https://github.com/NuGet/Home/issues/4679)</span><span class="sxs-lookup"><span data-stu-id="80954-153">RestoreManagerPackage being loaded synchronously which blocked UI thread and deadlocked VS - [#4679](https://github.com/NuGet/Home/issues/4679)</span></span>

* <span data-ttu-id="80954-154">dotnet restore (et par conséquent msbuild /t:restore) ignore les projets avec une dépendance de projet de solution explicite [#4578](https://github.com/NuGet/Home/issues/4578)</span><span class="sxs-lookup"><span data-stu-id="80954-154">dotnet Restore (& therefore msbuild /t:restore) skips projects with an explicit solution project dependency [#4578](https://github.com/NuGet/Home/issues/4578)</span></span>

* <span data-ttu-id="80954-155">Si votre solution a des références de projet qui référencent le même projet avec une casse différente, la restauration risque de ne pas fonctionner.</span><span class="sxs-lookup"><span data-stu-id="80954-155">If your solution has projectreferences that refer to the same project, with different casing, restore may not work.</span></span> <span data-ttu-id="80954-156">Cela affecte également les chemins relatifs différents sans différence de casse - [#4574](https://github.com/NuGet/Home/issues/4574)</span><span class="sxs-lookup"><span data-stu-id="80954-156">This also affects different relative paths, without a difference in casing - [#4574](https://github.com/NuGet/Home/issues/4574)</span></span>

* <span data-ttu-id="80954-157">Les fichiers exécutables restaurés à partir de packages NuGet ne sont plus exécutables avec .NET Core 2.0 - [#4424](https://github.com/NuGet/Home/issues/4424)</span><span class="sxs-lookup"><span data-stu-id="80954-157">Executables restored from NuGet packages are no longer executable with .NET Core 2.0 - [#4424](https://github.com/NuGet/Home/issues/4424)</span></span>

* <span data-ttu-id="80954-158">NuGet.exe avale les détails d’exception lors de l’analyse du fichier de solution - [#4411](https://github.com/NuGet/Home/issues/4411)</span><span class="sxs-lookup"><span data-stu-id="80954-158">NuGet.exe swallows details of exception when parsing solution file - [#4411](https://github.com/NuGet/Home/issues/4411)</span></span>

* <span data-ttu-id="80954-159">Le pack place les fichiers de contenu à un emplacement incorrect si ContentTargetFolders contient un chemin qui se termine par « / » sur Windows - [#4407](https://github.com/NuGet/Home/issues/4407)</span><span class="sxs-lookup"><span data-stu-id="80954-159">Pack puts content files in wrong location if ContentTargetFolders contains a path that ends with '/' on Windows - [#4407](https://github.com/NuGet/Home/issues/4407)</span></span>

* <span data-ttu-id="80954-160">Impossible de restaurer un DotNetCliToolReference pour un package d’outils qui cible netcoreapp1.1 - [#4396](https://github.com/NuGet/Home/issues/4396)</span><span class="sxs-lookup"><span data-stu-id="80954-160">Can't restore a DotNetCliToolReference for a tools package that targets netcoreapp1.1 - [#4396](https://github.com/NuGet/Home/issues/4396)</span></span>

* <span data-ttu-id="80954-161">L’interface de ligne de commande de mise à jour de NuGet laisse l’ancienne condition de version de package dans le fichier de projet (C++) - [#2449](https://github.com/NuGet/Home/issues/2449)</span><span class="sxs-lookup"><span data-stu-id="80954-161">Nuget update CLI leaves the old package version condition in project file (C++) - [#2449](https://github.com/NuGet/Home/issues/2449)</span></span>

<span data-ttu-id="80954-162">**DCR :**</span><span class="sxs-lookup"><span data-stu-id="80954-162">**DCR:**</span></span>

* <span data-ttu-id="80954-163">Lecture de DotnetCliToolTargetFramework à partir de la nomination CPS - [#5397](https://github.com/NuGet/Home/issues/5397)</span><span class="sxs-lookup"><span data-stu-id="80954-163">Read DotnetCliToolTargetFramework from CPS nomation - [#5397](https://github.com/NuGet/Home/issues/5397)</span></span>

* <span data-ttu-id="80954-164">La vérification TPMinV doit fonctionner pour les applications UWP de style pj - [#4763](https://github.com/NuGet/Home/issues/4763)</span><span class="sxs-lookup"><span data-stu-id="80954-164">TPMinV check should work for pj style UWP - [#4763](https://github.com/NuGet/Home/issues/4763)</span></span>

* <span data-ttu-id="80954-165">Amélioration de la description de l’interface utilisateur pour les packages AutoReferenced - [#4471](https://github.com/NuGet/Home/issues/4471)</span><span class="sxs-lookup"><span data-stu-id="80954-165">Improve UI description for AutoReferenced packages - [#4471](https://github.com/NuGet/Home/issues/4471)</span></span>

* <span data-ttu-id="80954-166">NuGet restore sélectionne des ressources de compilation à partir de la section de runtime.</span><span class="sxs-lookup"><span data-stu-id="80954-166">NuGet restore is selecting compile assets from runtime section.</span></span><span data-ttu-id="80954-167"> - [#4207](https://github.com/NuGet/Home/issues/4207)</span><span class="sxs-lookup"><span data-stu-id="80954-167"> - [#4207](https://github.com/NuGet/Home/issues/4207)</span></span>

* <span data-ttu-id="80954-168">Placement des diagnostics de dépendances dans le fichier de verrouillage - [#1599](https://github.com/NuGet/Home/issues/1599)</span><span class="sxs-lookup"><span data-stu-id="80954-168">Put dependency diagnostics in the lock file - [#1599](https://github.com/NuGet/Home/issues/1599)</span></span>

## <a name="link-to-github-issues-fixed-in-43-rtm"></a><span data-ttu-id="80954-169">Lien vers les problèmes GitHub corrigés dans la version 4.3 RTM</span><span class="sxs-lookup"><span data-stu-id="80954-169">Link to GitHub issues fixed in 4.3 RTM</span></span>

[<span data-ttu-id="80954-170">Liste des problèmes</span><span class="sxs-lookup"><span data-stu-id="80954-170">Issues List</span></span>](https://github.com/NuGet/Home/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%224.3")
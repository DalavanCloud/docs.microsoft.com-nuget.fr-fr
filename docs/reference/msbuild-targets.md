---
title: Commandes pack et restore NuGet comme cibles MSBuild
description: Les commandes pack et restore NuGet peuvent être utilisées directement comme cibles MSBuild avec NuGet 4.0+.
author: karann-msft
ms.author: karann
manager: unnir
ms.date: 03/23/2018
ms.topic: conceptual
ms.openlocfilehash: 0e7e0952519afdcb4b50f31d33cce2a92e3579b4
ms.sourcegitcommit: a76ecc58f41c2c5b3536ff4a3f3fcbdf5258177c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39069698"
---
# <a name="nuget-pack-and-restore-as-msbuild-targets"></a><span data-ttu-id="fb149-103">Commandes pack et restore NuGet comme cibles MSBuild</span><span class="sxs-lookup"><span data-stu-id="fb149-103">NuGet pack and restore as MSBuild targets</span></span>

<span data-ttu-id="fb149-104">*NuGet 4.0+*</span><span class="sxs-lookup"><span data-stu-id="fb149-104">*NuGet 4.0+*</span></span>

<span data-ttu-id="fb149-105">Avec le format PackageReference, NuGet 4.0+ peut stocker toutes les métadonnées du manifeste directement dans un fichier projet, au lieu d’utiliser un fichier `.nuspec` distinct.</span><span class="sxs-lookup"><span data-stu-id="fb149-105">With the PackageReference format, NuGet 4.0+ can store all manifest metadata directly within a project file rather than using a separate `.nuspec` file.</span></span>

<span data-ttu-id="fb149-106">Avec MSBuild 15.1+, NuGet est également un citoyen MSBuild de première classe avec les cibles `pack` et `restore` comme décrit ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="fb149-106">With MSBuild 15.1+, NuGet is also a first-class MSBuild citizen with the `pack` and `restore` targets as described below.</span></span> <span data-ttu-id="fb149-107">Ces cibles vous permettent d’utiliser NuGet comme vous utiliseriez toute autre tâche ou cible MSBuild.</span><span class="sxs-lookup"><span data-stu-id="fb149-107">These targets allow you to work with NuGet as you would with any other MSBuild task or target.</span></span> <span data-ttu-id="fb149-108">(Pour NuGet 3.x et versions antérieures, vous utilisez les commandes [pack](../tools/cli-ref-pack.md) et [restore](../tools/cli-ref-restore.md) via l’interface de ligne de commande NuGet à la place.)</span><span class="sxs-lookup"><span data-stu-id="fb149-108">(For NuGet 3.x and earlier, you use the [pack](../tools/cli-ref-pack.md) and [restore](../tools/cli-ref-restore.md) commands through the NuGet CLI instead.)</span></span>

## <a name="target-build-order"></a><span data-ttu-id="fb149-109">Ordre de génération des cibles</span><span class="sxs-lookup"><span data-stu-id="fb149-109">Target build order</span></span>

<span data-ttu-id="fb149-110">Étant donné que `pack` et `restore` sont des cibles MSBuild, vous pouvez y accéder pour améliorer votre flux de travail.</span><span class="sxs-lookup"><span data-stu-id="fb149-110">Because `pack` and `restore` are  MSBuild targets, you can access them to enhance your workflow.</span></span> <span data-ttu-id="fb149-111">Par exemple, supposons que vous souhaitez copier votre package sur un partage réseau après compression.</span><span class="sxs-lookup"><span data-stu-id="fb149-111">For example, let’s say you want to copy your package to a network share after packing it.</span></span> <span data-ttu-id="fb149-112">Pour ce faire, ajoutez le code suivant dans votre fichier projet :</span><span class="sxs-lookup"><span data-stu-id="fb149-112">You can do that by adding the following in your project file:</span></span>

```xml
<Target Name="CopyPackage" AfterTargets="Pack">
  <Copy
    SourceFiles="$(OutputPath)..\$(PackageId).$(PackageVersion).nupkg"
    DestinationFolder="\\myshare\packageshare\"
    />
</Target>
```

<span data-ttu-id="fb149-113">De même, vous pouvez écrire une tâche MSBuild, écrire votre propre cible et consommer des propriétés NuGet dans la tâche MSBuild.</span><span class="sxs-lookup"><span data-stu-id="fb149-113">Similarly, you can write an MSBuild task, write your own target and consume NuGet properties in the MSBuild task.</span></span>

## <a name="pack-target"></a><span data-ttu-id="fb149-114">Cible pack</span><span class="sxs-lookup"><span data-stu-id="fb149-114">pack target</span></span>

<span data-ttu-id="fb149-115">Pour les projets .NET Standard en utilisant le format PackageReference, à l’aide de `msbuild /t:pack` dessine des entrées à partir du fichier de projet à utiliser pour créer un package NuGet.</span><span class="sxs-lookup"><span data-stu-id="fb149-115">For .NET Standard projects using the PackageReference format, using `msbuild /t:pack` draws inputs from the project file to use in creating a NuGet package.</span></span>

<span data-ttu-id="fb149-116">Le tableau ci-dessous décrit les propriétés MSBuild qui peuvent être ajoutées à un fichier projet au sein du premier nœud `<PropertyGroup>`.</span><span class="sxs-lookup"><span data-stu-id="fb149-116">The table below describes the MSBuild properties that can be added to a project file within the first `<PropertyGroup>` node.</span></span> <span data-ttu-id="fb149-117">Vous pouvez effectuer ces modifications facilement dans Visual Studio 2017 et versions ultérieures en cliquant avec le bouton droit sur le projet et en sélectionnant **Modifier {nom_projet}** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="fb149-117">You can make these edits easily in Visual Studio 2017 and later by right-clicking the project and selecting **Edit {project_name}** on the context menu.</span></span> <span data-ttu-id="fb149-118">Pour des raisons pratiques, le tableau est organisé selon la propriété équivalente dans un [fichier `.nuspec`](../reference/nuspec.md).</span><span class="sxs-lookup"><span data-stu-id="fb149-118">For convenience the table is organized by the equivalent property in a [`.nuspec` file](../reference/nuspec.md).</span></span>

<span data-ttu-id="fb149-119">Notez que les propriétés `Owners` et `Summary` de `.nuspec` ne sont pas prises en charge avec MSBuild.</span><span class="sxs-lookup"><span data-stu-id="fb149-119">Note that the `Owners` and `Summary` properties from `.nuspec` are not supported with MSBuild.</span></span>

| <span data-ttu-id="fb149-120">Valeur d’attribut/NuSpec</span><span class="sxs-lookup"><span data-stu-id="fb149-120">Attribute/NuSpec Value</span></span> | <span data-ttu-id="fb149-121">Propriété MSBuild</span><span class="sxs-lookup"><span data-stu-id="fb149-121">MSBuild Property</span></span> | <span data-ttu-id="fb149-122">Par défaut</span><span class="sxs-lookup"><span data-stu-id="fb149-122">Default</span></span> | <span data-ttu-id="fb149-123">Notes</span><span class="sxs-lookup"><span data-stu-id="fb149-123">Notes</span></span> |
|--------|--------|--------|--------|
| <span data-ttu-id="fb149-124">Id</span><span class="sxs-lookup"><span data-stu-id="fb149-124">Id</span></span> | <span data-ttu-id="fb149-125">PackageId</span><span class="sxs-lookup"><span data-stu-id="fb149-125">PackageId</span></span> | <span data-ttu-id="fb149-126">AssemblyName</span><span class="sxs-lookup"><span data-stu-id="fb149-126">AssemblyName</span></span> | <span data-ttu-id="fb149-127">$(AssemblyName) de MSBuild</span><span class="sxs-lookup"><span data-stu-id="fb149-127">$(AssemblyName) from MSBuild</span></span> |
| <span data-ttu-id="fb149-128">Version</span><span class="sxs-lookup"><span data-stu-id="fb149-128">Version</span></span> | <span data-ttu-id="fb149-129">PackageVersion</span><span class="sxs-lookup"><span data-stu-id="fb149-129">PackageVersion</span></span> | <span data-ttu-id="fb149-130">Version</span><span class="sxs-lookup"><span data-stu-id="fb149-130">Version</span></span> | <span data-ttu-id="fb149-131">Compatible avec SemVer, par exemple « 1.0.0 », « version bêta 1.0.0 » ou « version bêta-1.0.0-00345 »</span><span class="sxs-lookup"><span data-stu-id="fb149-131">This is semver compatible, for example “1.0.0”, “1.0.0-beta”, or “1.0.0-beta-00345”</span></span> |
| <span data-ttu-id="fb149-132">VersionPrefix</span><span class="sxs-lookup"><span data-stu-id="fb149-132">VersionPrefix</span></span> | <span data-ttu-id="fb149-133">PackageVersionPrefix</span><span class="sxs-lookup"><span data-stu-id="fb149-133">PackageVersionPrefix</span></span> | <span data-ttu-id="fb149-134">vide</span><span class="sxs-lookup"><span data-stu-id="fb149-134">empty</span></span> | <span data-ttu-id="fb149-135">La définition de PackageVersion remplace PackageVersionPrefix</span><span class="sxs-lookup"><span data-stu-id="fb149-135">Setting PackageVersion overwrites PackageVersionPrefix</span></span> |
| <span data-ttu-id="fb149-136">VersionSuffix</span><span class="sxs-lookup"><span data-stu-id="fb149-136">VersionSuffix</span></span> | <span data-ttu-id="fb149-137">PackageVersionSuffix</span><span class="sxs-lookup"><span data-stu-id="fb149-137">PackageVersionSuffix</span></span> | <span data-ttu-id="fb149-138">vide</span><span class="sxs-lookup"><span data-stu-id="fb149-138">empty</span></span> | <span data-ttu-id="fb149-139">$(VersionSuffix) de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="fb149-139">$(VersionSuffix) from MSBuild.</span></span> <span data-ttu-id="fb149-140">La définition de PackageVersion remplace PackageVersionSuffix</span><span class="sxs-lookup"><span data-stu-id="fb149-140">Setting PackageVersion overwrites PackageVersionSuffix</span></span> |
| <span data-ttu-id="fb149-141">Auteurs</span><span class="sxs-lookup"><span data-stu-id="fb149-141">Authors</span></span> | <span data-ttu-id="fb149-142">Auteurs</span><span class="sxs-lookup"><span data-stu-id="fb149-142">Authors</span></span> | <span data-ttu-id="fb149-143">Nom de l’utilisateur actuel</span><span class="sxs-lookup"><span data-stu-id="fb149-143">Username of the current user</span></span> | |
| <span data-ttu-id="fb149-144">Propriétaires</span><span class="sxs-lookup"><span data-stu-id="fb149-144">Owners</span></span> | <span data-ttu-id="fb149-145">N/A</span><span class="sxs-lookup"><span data-stu-id="fb149-145">N/A</span></span> | <span data-ttu-id="fb149-146">Ne figure pas dans NuSpec</span><span class="sxs-lookup"><span data-stu-id="fb149-146">Not present in NuSpec</span></span> | |
| <span data-ttu-id="fb149-147">Titre</span><span class="sxs-lookup"><span data-stu-id="fb149-147">Title</span></span> | <span data-ttu-id="fb149-148">Titre</span><span class="sxs-lookup"><span data-stu-id="fb149-148">Title</span></span> | <span data-ttu-id="fb149-149">PackageId</span><span class="sxs-lookup"><span data-stu-id="fb149-149">The PackageId</span></span>| |
| <span data-ttu-id="fb149-150">Description</span><span class="sxs-lookup"><span data-stu-id="fb149-150">Description</span></span> | <span data-ttu-id="fb149-151">Description</span><span class="sxs-lookup"><span data-stu-id="fb149-151">Description</span></span> | <span data-ttu-id="fb149-152">« Description du package »</span><span class="sxs-lookup"><span data-stu-id="fb149-152">"Package Description"</span></span> | |
| <span data-ttu-id="fb149-153">Copyright</span><span class="sxs-lookup"><span data-stu-id="fb149-153">Copyright</span></span> | <span data-ttu-id="fb149-154">Copyright</span><span class="sxs-lookup"><span data-stu-id="fb149-154">Copyright</span></span> | <span data-ttu-id="fb149-155">vide</span><span class="sxs-lookup"><span data-stu-id="fb149-155">empty</span></span> | |
| <span data-ttu-id="fb149-156">RequireLicenseAcceptance</span><span class="sxs-lookup"><span data-stu-id="fb149-156">RequireLicenseAcceptance</span></span> | <span data-ttu-id="fb149-157">PackageRequireLicenseAcceptance</span><span class="sxs-lookup"><span data-stu-id="fb149-157">PackageRequireLicenseAcceptance</span></span> | <span data-ttu-id="fb149-158">False</span><span class="sxs-lookup"><span data-stu-id="fb149-158">false</span></span> | |
| <span data-ttu-id="fb149-159">LicenseUrl</span><span class="sxs-lookup"><span data-stu-id="fb149-159">LicenseUrl</span></span> | <span data-ttu-id="fb149-160">PackageLicenseUrl</span><span class="sxs-lookup"><span data-stu-id="fb149-160">PackageLicenseUrl</span></span> | <span data-ttu-id="fb149-161">vide</span><span class="sxs-lookup"><span data-stu-id="fb149-161">empty</span></span> | |
| <span data-ttu-id="fb149-162">ProjectUrl</span><span class="sxs-lookup"><span data-stu-id="fb149-162">ProjectUrl</span></span> | <span data-ttu-id="fb149-163">PackageProjectUrl</span><span class="sxs-lookup"><span data-stu-id="fb149-163">PackageProjectUrl</span></span> | <span data-ttu-id="fb149-164">vide</span><span class="sxs-lookup"><span data-stu-id="fb149-164">empty</span></span> | |
| <span data-ttu-id="fb149-165">IconUrl</span><span class="sxs-lookup"><span data-stu-id="fb149-165">IconUrl</span></span> | <span data-ttu-id="fb149-166">PackageIconUrl</span><span class="sxs-lookup"><span data-stu-id="fb149-166">PackageIconUrl</span></span> | <span data-ttu-id="fb149-167">vide</span><span class="sxs-lookup"><span data-stu-id="fb149-167">empty</span></span> | |
| <span data-ttu-id="fb149-168">Balises</span><span class="sxs-lookup"><span data-stu-id="fb149-168">Tags</span></span> | <span data-ttu-id="fb149-169">PackageTags</span><span class="sxs-lookup"><span data-stu-id="fb149-169">PackageTags</span></span> | <span data-ttu-id="fb149-170">vide</span><span class="sxs-lookup"><span data-stu-id="fb149-170">empty</span></span> | <span data-ttu-id="fb149-171">Les balises sont séparées par un point-virgule.</span><span class="sxs-lookup"><span data-stu-id="fb149-171">Tags are semi-colon delimited.</span></span> |
| <span data-ttu-id="fb149-172">ReleaseNotes</span><span class="sxs-lookup"><span data-stu-id="fb149-172">ReleaseNotes</span></span> | <span data-ttu-id="fb149-173">PackageReleaseNotes</span><span class="sxs-lookup"><span data-stu-id="fb149-173">PackageReleaseNotes</span></span> | <span data-ttu-id="fb149-174">vide</span><span class="sxs-lookup"><span data-stu-id="fb149-174">empty</span></span> | |
| <span data-ttu-id="fb149-175">Url/du référentiel</span><span class="sxs-lookup"><span data-stu-id="fb149-175">Repository/Url</span></span> | <span data-ttu-id="fb149-176">RepositoryUrl</span><span class="sxs-lookup"><span data-stu-id="fb149-176">RepositoryUrl</span></span> | <span data-ttu-id="fb149-177">vide</span><span class="sxs-lookup"><span data-stu-id="fb149-177">empty</span></span> | <span data-ttu-id="fb149-178">URL du référentiel utilisé pour cloner ou extraire le code source.</span><span class="sxs-lookup"><span data-stu-id="fb149-178">Repository URL used to clone or retrieve source code.</span></span> <span data-ttu-id="fb149-179">Exemple : *https://github.com/NuGet/NuGet.Client.git*</span><span class="sxs-lookup"><span data-stu-id="fb149-179">Example: *https://github.com/NuGet/NuGet.Client.git*</span></span> |
| <span data-ttu-id="fb149-180">/ Type de référentiel</span><span class="sxs-lookup"><span data-stu-id="fb149-180">Repository/Type</span></span> | <span data-ttu-id="fb149-181">RepositoryType</span><span class="sxs-lookup"><span data-stu-id="fb149-181">RepositoryType</span></span> | <span data-ttu-id="fb149-182">vide</span><span class="sxs-lookup"><span data-stu-id="fb149-182">empty</span></span> | <span data-ttu-id="fb149-183">Type de référentiel.</span><span class="sxs-lookup"><span data-stu-id="fb149-183">Repository type.</span></span> <span data-ttu-id="fb149-184">Exemples : *git*, *tfs*.</span><span class="sxs-lookup"><span data-stu-id="fb149-184">Examples: *git*, *tfs*.</span></span> |
| <span data-ttu-id="fb149-185">/ Branche du référentiel</span><span class="sxs-lookup"><span data-stu-id="fb149-185">Repository/Branch</span></span> | <span data-ttu-id="fb149-186">RepositoryBranch</span><span class="sxs-lookup"><span data-stu-id="fb149-186">RepositoryBranch</span></span> | <span data-ttu-id="fb149-187">vide</span><span class="sxs-lookup"><span data-stu-id="fb149-187">empty</span></span> | <span data-ttu-id="fb149-188">Informations de branche de référentiel facultatif.</span><span class="sxs-lookup"><span data-stu-id="fb149-188">Optional repository branch information.</span></span> <span data-ttu-id="fb149-189">*RepositoryUrl* doit également être spécifié pour cette propriété à inclure.</span><span class="sxs-lookup"><span data-stu-id="fb149-189">*RepositoryUrl* must also be specified for this property to be included.</span></span> <span data-ttu-id="fb149-190">Exemple : *master* (4.7.0+ NuGet)</span><span class="sxs-lookup"><span data-stu-id="fb149-190">Example: *master* (NuGet 4.7.0+)</span></span> |
| <span data-ttu-id="fb149-191">Référentiel/validation</span><span class="sxs-lookup"><span data-stu-id="fb149-191">Repository/Commit</span></span> | <span data-ttu-id="fb149-192">RepositoryCommit</span><span class="sxs-lookup"><span data-stu-id="fb149-192">RepositoryCommit</span></span> | <span data-ttu-id="fb149-193">vide</span><span class="sxs-lookup"><span data-stu-id="fb149-193">empty</span></span> | <span data-ttu-id="fb149-194">Validation du référentiel facultatif ou l’ensemble de modifications pour indiquer à qui la source du package a été créé.</span><span class="sxs-lookup"><span data-stu-id="fb149-194">Optional repository commit or changeset to indicate which source the package was built against.</span></span> <span data-ttu-id="fb149-195">*RepositoryUrl* doit également être spécifié pour cette propriété à inclure.</span><span class="sxs-lookup"><span data-stu-id="fb149-195">*RepositoryUrl* must also be specified for this property to be included.</span></span> <span data-ttu-id="fb149-196">Exemple : *0e4d1b598f350b3dc675018d539114d1328189ef* (4.7.0+ NuGet)</span><span class="sxs-lookup"><span data-stu-id="fb149-196">Example: *0e4d1b598f350b3dc675018d539114d1328189ef* (NuGet 4.7.0+)</span></span> |
| <span data-ttu-id="fb149-197">PackageType</span><span class="sxs-lookup"><span data-stu-id="fb149-197">PackageType</span></span> | `<PackageType>DotNetCliTool, 1.0.0.0;Dependency, 2.0.0.0</PackageType>` | | |
| <span data-ttu-id="fb149-198">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="fb149-198">Summary</span></span> | <span data-ttu-id="fb149-199">Non pris en charge</span><span class="sxs-lookup"><span data-stu-id="fb149-199">Not supported</span></span> | | |

### <a name="pack-target-inputs"></a><span data-ttu-id="fb149-200">entrées de cible pack</span><span class="sxs-lookup"><span data-stu-id="fb149-200">pack target inputs</span></span>

- <span data-ttu-id="fb149-201">IsPackable</span><span class="sxs-lookup"><span data-stu-id="fb149-201">IsPackable</span></span>
- <span data-ttu-id="fb149-202">PackageVersion</span><span class="sxs-lookup"><span data-stu-id="fb149-202">PackageVersion</span></span>
- <span data-ttu-id="fb149-203">PackageId</span><span class="sxs-lookup"><span data-stu-id="fb149-203">PackageId</span></span>
- <span data-ttu-id="fb149-204">Auteurs</span><span class="sxs-lookup"><span data-stu-id="fb149-204">Authors</span></span>
- <span data-ttu-id="fb149-205">Description</span><span class="sxs-lookup"><span data-stu-id="fb149-205">Description</span></span>
- <span data-ttu-id="fb149-206">Copyright</span><span class="sxs-lookup"><span data-stu-id="fb149-206">Copyright</span></span>
- <span data-ttu-id="fb149-207">PackageRequireLicenseAcceptance</span><span class="sxs-lookup"><span data-stu-id="fb149-207">PackageRequireLicenseAcceptance</span></span>
- <span data-ttu-id="fb149-208">DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="fb149-208">DevelopmentDependency</span></span>
- <span data-ttu-id="fb149-209">PackageLicenseUrl</span><span class="sxs-lookup"><span data-stu-id="fb149-209">PackageLicenseUrl</span></span>
- <span data-ttu-id="fb149-210">PackageProjectUrl</span><span class="sxs-lookup"><span data-stu-id="fb149-210">PackageProjectUrl</span></span>
- <span data-ttu-id="fb149-211">PackageIconUrl</span><span class="sxs-lookup"><span data-stu-id="fb149-211">PackageIconUrl</span></span>
- <span data-ttu-id="fb149-212">PackageReleaseNotes</span><span class="sxs-lookup"><span data-stu-id="fb149-212">PackageReleaseNotes</span></span>
- <span data-ttu-id="fb149-213">PackageTags</span><span class="sxs-lookup"><span data-stu-id="fb149-213">PackageTags</span></span>
- <span data-ttu-id="fb149-214">PackageOutputPath</span><span class="sxs-lookup"><span data-stu-id="fb149-214">PackageOutputPath</span></span>
- <span data-ttu-id="fb149-215">IncludeSymbols</span><span class="sxs-lookup"><span data-stu-id="fb149-215">IncludeSymbols</span></span>
- <span data-ttu-id="fb149-216">IncludeSource</span><span class="sxs-lookup"><span data-stu-id="fb149-216">IncludeSource</span></span>
- <span data-ttu-id="fb149-217">PackageTypes</span><span class="sxs-lookup"><span data-stu-id="fb149-217">PackageTypes</span></span>
- <span data-ttu-id="fb149-218">IsTool</span><span class="sxs-lookup"><span data-stu-id="fb149-218">IsTool</span></span>
- <span data-ttu-id="fb149-219">RepositoryUrl</span><span class="sxs-lookup"><span data-stu-id="fb149-219">RepositoryUrl</span></span>
- <span data-ttu-id="fb149-220">RepositoryType</span><span class="sxs-lookup"><span data-stu-id="fb149-220">RepositoryType</span></span>
- <span data-ttu-id="fb149-221">RepositoryBranch</span><span class="sxs-lookup"><span data-stu-id="fb149-221">RepositoryBranch</span></span>
- <span data-ttu-id="fb149-222">RepositoryCommit</span><span class="sxs-lookup"><span data-stu-id="fb149-222">RepositoryCommit</span></span>
- <span data-ttu-id="fb149-223">NoPackageAnalysis</span><span class="sxs-lookup"><span data-stu-id="fb149-223">NoPackageAnalysis</span></span>
- <span data-ttu-id="fb149-224">MinClientVersion</span><span class="sxs-lookup"><span data-stu-id="fb149-224">MinClientVersion</span></span>
- <span data-ttu-id="fb149-225">IncludeBuildOutput</span><span class="sxs-lookup"><span data-stu-id="fb149-225">IncludeBuildOutput</span></span>
- <span data-ttu-id="fb149-226">IncludeContentInPack</span><span class="sxs-lookup"><span data-stu-id="fb149-226">IncludeContentInPack</span></span>
- <span data-ttu-id="fb149-227">BuildOutputTargetFolder</span><span class="sxs-lookup"><span data-stu-id="fb149-227">BuildOutputTargetFolder</span></span>
- <span data-ttu-id="fb149-228">ContentTargetFolders</span><span class="sxs-lookup"><span data-stu-id="fb149-228">ContentTargetFolders</span></span>
- <span data-ttu-id="fb149-229">NuspecFile</span><span class="sxs-lookup"><span data-stu-id="fb149-229">NuspecFile</span></span>
- <span data-ttu-id="fb149-230">NuspecBasePath</span><span class="sxs-lookup"><span data-stu-id="fb149-230">NuspecBasePath</span></span>
- <span data-ttu-id="fb149-231">NuspecProperties</span><span class="sxs-lookup"><span data-stu-id="fb149-231">NuspecProperties</span></span>

## <a name="pack-scenarios"></a><span data-ttu-id="fb149-232">Scénarios avec pack</span><span class="sxs-lookup"><span data-stu-id="fb149-232">pack scenarios</span></span>

### <a name="packageiconurl"></a><span data-ttu-id="fb149-233">PackageIconUrl</span><span class="sxs-lookup"><span data-stu-id="fb149-233">PackageIconUrl</span></span>

<span data-ttu-id="fb149-234">Dans le cadre de la modification pour [NuGet problème 352](https://github.com/NuGet/Home/issues/352), `PackageIconUrl` sera finalement remplacée par `PackageIconUri` et peut être un chemin relatif vers un fichier d’icône qui sera inclus à la racine du package obtenu.</span><span class="sxs-lookup"><span data-stu-id="fb149-234">As part of the change for [NuGet Issue 352](https://github.com/NuGet/Home/issues/352), `PackageIconUrl` will eventually be changed to `PackageIconUri` and can be relative path to a icon file which will included at the root of the resulting package.</span></span>

### <a name="output-assemblies"></a><span data-ttu-id="fb149-235">Assemblys de sortie</span><span class="sxs-lookup"><span data-stu-id="fb149-235">Output assemblies</span></span>

<span data-ttu-id="fb149-236">`nuget pack` copie les fichiers de sortie avec les extensions `.exe`, `.dll`, `.xml`, `.winmd`, `.json` et `.pri`.</span><span class="sxs-lookup"><span data-stu-id="fb149-236">`nuget pack` copies output files with extensions `.exe`, `.dll`, `.xml`, `.winmd`, `.json`, and `.pri`.</span></span> <span data-ttu-id="fb149-237">Les fichiers de sortie qui sont copiés dépendent de ce que fournit MSBuild à partir de la cible `BuiltOutputProjectGroup`.</span><span class="sxs-lookup"><span data-stu-id="fb149-237">The output files that are copied depend on what MSBuild provides from the `BuiltOutputProjectGroup` target.</span></span>

<span data-ttu-id="fb149-238">Il existe deux propriétés MSBuild que vous pouvez utiliser dans votre fichier projet ou ligne de commande pour contrôler la destination des assemblys de sortie :</span><span class="sxs-lookup"><span data-stu-id="fb149-238">There are two MSBuild  properties that you can use in your project file or command line to control where output assemblies go:</span></span>

- <span data-ttu-id="fb149-239">`IncludeBuildOutput` : valeur booléenne qui détermine si les assemblys de sortie de génération doivent être inclus dans le package.</span><span class="sxs-lookup"><span data-stu-id="fb149-239">`IncludeBuildOutput`: A boolean that determines whether the build output assemblies should be included in the package.</span></span>
- <span data-ttu-id="fb149-240">`BuildOutputTargetFolder` : spécifie le dossier dans lequel les assemblys de sortie doivent être placés.</span><span class="sxs-lookup"><span data-stu-id="fb149-240">`BuildOutputTargetFolder`: Specifies the folder in which the output assemblies should be placed.</span></span> <span data-ttu-id="fb149-241">Les assemblys de sortie (et les autres fichiers de sortie) sont copiés dans les dossiers de leur framework respectif.</span><span class="sxs-lookup"><span data-stu-id="fb149-241">The output assemblies (and other output files) are copied into their respective framework folders.</span></span>

### <a name="package-references"></a><span data-ttu-id="fb149-242">Références de package</span><span class="sxs-lookup"><span data-stu-id="fb149-242">Package references</span></span>

<span data-ttu-id="fb149-243">Consultez [Références de package dans les fichiers projet](../consume-packages/package-references-in-project-files.md).</span><span class="sxs-lookup"><span data-stu-id="fb149-243">See [Package References in Project Files](../consume-packages/package-references-in-project-files.md).</span></span>

### <a name="project-to-project-references"></a><span data-ttu-id="fb149-244">Références entre projets</span><span class="sxs-lookup"><span data-stu-id="fb149-244">Project to project references</span></span>

<span data-ttu-id="fb149-245">Les références entre projets sont considérées par défaut comme des références de package nuget, par exemple :</span><span class="sxs-lookup"><span data-stu-id="fb149-245">Project to project references are considered by default as nuget package references, for example:</span></span>

```xml
<ProjectReference Include="..\UwpLibrary2\UwpLibrary2.csproj"/>
```

<span data-ttu-id="fb149-246">Vous pouvez également ajouter les métadonnées suivantes à votre référence de projet :</span><span class="sxs-lookup"><span data-stu-id="fb149-246">You can also add the following metadata to your project reference:</span></span>

```xml
<IncludeAssets>
<ExcludeAssets>
<PrivateAssets>
```

### <a name="including-content-in-a-package"></a><span data-ttu-id="fb149-247">Ajout de contenu dans un package</span><span class="sxs-lookup"><span data-stu-id="fb149-247">Including content in a package</span></span>

<span data-ttu-id="fb149-248">Pour inclure du contenu, ajoutez des métadonnées supplémentaires à l’élément `<Content>`.</span><span class="sxs-lookup"><span data-stu-id="fb149-248">To include content, add extra metadata to the existing `<Content>` item.</span></span> <span data-ttu-id="fb149-249">Par défaut, tous les éléments de type « Contenu » sont inclus dans le package, sauf si vous procédez à un remplacement avec des entrées telles que les suivantes :</span><span class="sxs-lookup"><span data-stu-id="fb149-249">By default everything of type "Content" gets included in the package unless you override with entries like the following:</span></span>

 ```xml
<Content Include="..\win7-x64\libuv.txt">
  <Pack>false</Pack>
</Content>
 ```

<span data-ttu-id="fb149-250">Par défaut, tous les éléments sont ajoutés à la racine de `content` et du dossier `contentFiles\any\<target_framework>` au sein d’un package et la structure de dossier relatif est conservée, sauf si vous spécifiez un chemin de package :</span><span class="sxs-lookup"><span data-stu-id="fb149-250">By default, everything gets added to the root of the `content` and `contentFiles\any\<target_framework>` folder within a package and preserves the relative folder structure, unless you specify a package path:</span></span>

```xml
<Content Include="..\win7-x64\libuv.txt">
  <Pack>true</Pack>
  <PackagePath>content\myfiles\</PackagePath>
</Content>
```

<span data-ttu-id="fb149-251">Si vous souhaitez copier tout le contenu uniquement vers des dossiers racines spécifiques (et non vers `content` et `contentFiles`), vous pouvez utiliser la propriété MSBuild `ContentTargetFolders` qui a pour valeur par défaut « content;contentFiles », mais peut être définie sur tout autre nom de dossier.</span><span class="sxs-lookup"><span data-stu-id="fb149-251">If you want to copy all your content to only a specific root folder(s) (instead of `content` and `contentFiles` both), you can use the MSBuild property `ContentTargetFolders`, which defaults to "content;contentFiles" but can be set to any other folder names.</span></span> <span data-ttu-id="fb149-252">Notez que le fait de spécifier uniquement « contentFiles » dans `ContentTargetFolders` place les fichiers sous `contentFiles\any\<target_framework>` ou `contentFiles\<language>\<target_framework>` selon `buildAction`.</span><span class="sxs-lookup"><span data-stu-id="fb149-252">Note that just specifying "contentFiles" in `ContentTargetFolders` puts files under `contentFiles\any\<target_framework>` or `contentFiles\<language>\<target_framework>` based on `buildAction`.</span></span>

<span data-ttu-id="fb149-253">`PackagePath` peut être un ensemble de chemins cibles séparés par un point-virgule.</span><span class="sxs-lookup"><span data-stu-id="fb149-253">`PackagePath` can be a semicolon-delimited set of target paths.</span></span> <span data-ttu-id="fb149-254">La spécification d’un chemin de package vide permet d’ajouter le fichier à la racine du package.</span><span class="sxs-lookup"><span data-stu-id="fb149-254">Specifying an empty package path would add the file to the root of the package.</span></span> <span data-ttu-id="fb149-255">Par exemple, le code suivant ajoute `libuv.txt` à `content\myfiles`, `content\samples` et la racine du package :</span><span class="sxs-lookup"><span data-stu-id="fb149-255">For example, the following adds `libuv.txt` to `content\myfiles`, `content\samples`, and the package root:</span></span>

```xml
<Content Include="..\win7-x64\libuv.txt">
  <Pack>true</Pack>
  <PackagePath>content\myfiles;content\sample;;</PackagePath>
</Content>
```

<span data-ttu-id="fb149-256">Il existe également une propriété MSBuild `$(IncludeContentInPack)` qui a pour valeur par défaut `true`.</span><span class="sxs-lookup"><span data-stu-id="fb149-256">There is also an MSBuild property `$(IncludeContentInPack)`, which defaults to `true`.</span></span> <span data-ttu-id="fb149-257">Si sa valeur est `false` sur un projet, le contenu de ce projet ne figure pas dans le package nuget.</span><span class="sxs-lookup"><span data-stu-id="fb149-257">If this is set to `false` on any project, then the content from that project are not included in the nuget package.</span></span>

<span data-ttu-id="fb149-258">D’autres métadonnées spécifiques à pack que vous pouvez définir sur l’un des éléments ci-dessus incluent ```<PackageCopyToOutput>``` et ```<PackageFlatten>``` qui définissent les valeurs ```CopyToOutput``` et ```Flatten``` sur l’entrée ```contentFiles``` dans le fichier nuspec de sortie.</span><span class="sxs-lookup"><span data-stu-id="fb149-258">Other pack specific metadata that you can set on any of the above items includes ```<PackageCopyToOutput>``` and ```<PackageFlatten>``` which sets ```CopyToOutput``` and ```Flatten``` values on the ```contentFiles``` entry in the output nuspec.</span></span>

> [!Note]
> <span data-ttu-id="fb149-259">Outre les éléments de contenu, les métadonnées `<Pack>` et `<PackagePath>` peuvent aussi être définies sur des fichiers avec l’action de génération Compile, EmbeddedResource, ApplicationDefinition, Page, Resource, SplashScreen, DesignData, DesignDataWithDesignTimeCreateableTypes, CodeAnalysisDictionary, AndroidAsset, AndroidResource, BundleResource ou None.</span><span class="sxs-lookup"><span data-stu-id="fb149-259">Apart from Content items, the `<Pack>` and `<PackagePath>` metadata can also be set on files with a build action of Compile, EmbeddedResource, ApplicationDefinition, Page, Resource, SplashScreen, DesignData, DesignDataWithDesignTimeCreateableTypes, CodeAnalysisDictionary, AndroidAsset, AndroidResource, BundleResource or None.</span></span>
>
> <span data-ttu-id="fb149-260">Pour que la commande pack ajoute le nom de fichier à votre chemin de package lors de l’utilisation de modèles de globbing, votre chemin doit se terminer par le caractère de séparation de dossier, sinon il est traité comme le chemin complet avec le nom de fichier.</span><span class="sxs-lookup"><span data-stu-id="fb149-260">For pack to append the filename to your package path when using globbing patterns, your package path must end with the folder separator character, otherwise the package path is treated as the full path including the file name.</span></span>

### <a name="includesymbols"></a><span data-ttu-id="fb149-261">IncludeSymbols</span><span class="sxs-lookup"><span data-stu-id="fb149-261">IncludeSymbols</span></span>

<span data-ttu-id="fb149-262">Quand vous utilisez `MSBuild /t:pack /p:IncludeSymbols=true`, les fichiers `.pdb` correspondants sont copiés avec d’autres fichiers de sortie (`.dll`, `.exe`, `.winmd`, `.xml`, `.json`, `.pri`).</span><span class="sxs-lookup"><span data-stu-id="fb149-262">When using `MSBuild /t:pack /p:IncludeSymbols=true`, the corresponding `.pdb` files are copied along with other output files (`.dll`, `.exe`, `.winmd`, `.xml`, `.json`, `.pri`).</span></span> <span data-ttu-id="fb149-263">Notez que la définition `IncludeSymbols=true` crée un package standard *et* un package de symboles.</span><span class="sxs-lookup"><span data-stu-id="fb149-263">Note that setting `IncludeSymbols=true` creates a regular package *and* a symbols package.</span></span>

### <a name="includesource"></a><span data-ttu-id="fb149-264">IncludeSource</span><span class="sxs-lookup"><span data-stu-id="fb149-264">IncludeSource</span></span>

<span data-ttu-id="fb149-265">Propriété identique à `IncludeSymbols`, sauf qu’elle copie également les fichiers sources avec les fichiers `.pdb`.</span><span class="sxs-lookup"><span data-stu-id="fb149-265">This is the same as `IncludeSymbols`, except that it copies source files along with `.pdb` files as well.</span></span> <span data-ttu-id="fb149-266">Tous les fichiers de type `Compile` sont copiés vers `src\<ProjectName>\` en conservant la structure de dossiers de chemin relatif dans le package obtenu.</span><span class="sxs-lookup"><span data-stu-id="fb149-266">All files of type `Compile` are copied over to `src\<ProjectName>\` preserving the relative path folder structure in the resulting package.</span></span> <span data-ttu-id="fb149-267">La même situation se produit également pour les fichiers sources de n’importe quel `ProjectReference` dont `TreatAsPackageReference` a la valeur `false`.</span><span class="sxs-lookup"><span data-stu-id="fb149-267">The same also happens for source files of any `ProjectReference` which has `TreatAsPackageReference` set to `false`.</span></span>

<span data-ttu-id="fb149-268">Si un fichier de type Compile est en dehors du dossier de projet, il est simplement ajouté à `src\<ProjectName>\`.</span><span class="sxs-lookup"><span data-stu-id="fb149-268">If a file of type Compile, is outside the project folder, then it's just added to `src\<ProjectName>\`.</span></span>

### <a name="istool"></a><span data-ttu-id="fb149-269">IsTool</span><span class="sxs-lookup"><span data-stu-id="fb149-269">IsTool</span></span>

<span data-ttu-id="fb149-270">Lors de l’utilisation de `MSBuild /t:pack /p:IsTool=true`, tous les fichiers de sortie, comme spécifié dans le scénario [Assemblys de sortie](#output-assemblies), sont copiés dans le dossier `tools` au lieu du dossier `lib`.</span><span class="sxs-lookup"><span data-stu-id="fb149-270">When using `MSBuild /t:pack /p:IsTool=true`, all output files, as specified in the [Output Assemblies](#output-assemblies) scenario, are copied to the `tools` folder instead of the `lib` folder.</span></span> <span data-ttu-id="fb149-271">Notez que cela est différent d’un `DotNetCliTool` qui est spécifié en définissant `PackageType` dans le fichier `.csproj`.</span><span class="sxs-lookup"><span data-stu-id="fb149-271">Note that this is different from a `DotNetCliTool` which is specified by setting the `PackageType` in `.csproj` file.</span></span>

### <a name="packing-using-a-nuspec"></a><span data-ttu-id="fb149-272">Compression à l’aide d’un fichier .nuspec</span><span class="sxs-lookup"><span data-stu-id="fb149-272">Packing using a .nuspec</span></span>

<span data-ttu-id="fb149-273">Vous pouvez utiliser un `.nuspec` fichier à compresser votre projet, sous réserve que vous avez un fichier de projet SDK à importer `NuGet.Build.Tasks.Pack.targets` afin que la tâche pack peut être exécutée.</span><span class="sxs-lookup"><span data-stu-id="fb149-273">You can use a `.nuspec` file to pack your project provided that you have a SDK project file to import `NuGet.Build.Tasks.Pack.targets` so that the pack task can be executed.</span></span> <span data-ttu-id="fb149-274">Vous devez toujours restaurer le projet avant que vous pouvez choisir un fichier nuspec.</span><span class="sxs-lookup"><span data-stu-id="fb149-274">You still need to restore the project before you can pack a nuspec file.</span></span> <span data-ttu-id="fb149-275">Le framework cible du fichier projet n’est pas pertinent et pas utilisé lors de la compression d’un fichier nuspec.</span><span class="sxs-lookup"><span data-stu-id="fb149-275">The target framework of the project file is irrelevant and not used when packing a nuspec.</span></span> <span data-ttu-id="fb149-276">Les trois propriétés MSBuild suivantes sont pertinentes lors de la compression à l’aide d’un fichier `.nuspec` :</span><span class="sxs-lookup"><span data-stu-id="fb149-276">The following three MSBuild properties are relevant to packing using a `.nuspec`:</span></span>

1. <span data-ttu-id="fb149-277">`NuspecFile` : chemin relatif ou absolu du fichier `.nuspec` utilisé pour la compression.</span><span class="sxs-lookup"><span data-stu-id="fb149-277">`NuspecFile`: relative or absolute path to the `.nuspec` file being used for packing.</span></span>
1. <span data-ttu-id="fb149-278">`NuspecProperties` : liste de paires clé=valeur séparées par un point-virgule.</span><span class="sxs-lookup"><span data-stu-id="fb149-278">`NuspecProperties`: a semicolon-separated list of key=value pairs.</span></span> <span data-ttu-id="fb149-279">En raison du mode de fonctionnement de l’analyse de ligne de commande MSBuild, plusieurs propriétés doivent être spécifiées comme suit : `/p:NuspecProperties=\"key1=value1;key2=value2\"`.</span><span class="sxs-lookup"><span data-stu-id="fb149-279">Due to the way MSBuild command-line parsing works, multiple properties must be specified as follows: `/p:NuspecProperties=\"key1=value1;key2=value2\"`.</span></span>  
1. <span data-ttu-id="fb149-280">`NuspecBasePath` : chemin de base pour le fichier `.nuspec`.</span><span class="sxs-lookup"><span data-stu-id="fb149-280">`NuspecBasePath`: Base path for the `.nuspec` file.</span></span>

<span data-ttu-id="fb149-281">Si vous utilisez `dotnet.exe` pour compresser votre projet, utilisez une commande semblable à la suivante :</span><span class="sxs-lookup"><span data-stu-id="fb149-281">If using `dotnet.exe` to pack your project, use a command like the following:</span></span>

```cli
dotnet pack <path to .csproj file> /p:NuspecFile=<path to nuspec file> /p:NuspecProperties=<> /p:NuspecBasePath=<Base path> 
```

<span data-ttu-id="fb149-282">Si vous utilisez MSBuild pour compresser votre projet, utilisez une commande semblable à la suivante :</span><span class="sxs-lookup"><span data-stu-id="fb149-282">If using MSBuild to pack your project, use a command like the following:</span></span>

```cli
msbuild /t:pack <path to .csproj file> /p:NuspecFile=<path to nuspec file> /p:NuspecProperties=<> /p:NuspecBasePath=<Base path> 
```

<span data-ttu-id="fb149-283">Veuillez noter qu’un nuspec de livraison à l’aide de dotnet.exe ou msbuild permet d’accéder à la génération du projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="fb149-283">Please note that packing a nuspec using dotnet.exe or msbuild also leads to building the project by default.</span></span> <span data-ttu-id="fb149-284">Cela peut être évité en passant ```--no-build``` dotnet.exe, qui est l’équivalent du paramètre de propriété ```<NoBuild>true</NoBuild> ``` dans votre fichier projet, ainsi que de paramètre ```<IncludeBuildOutput>false</IncludeBuildOutput> ``` dans le fichier projet</span><span class="sxs-lookup"><span data-stu-id="fb149-284">This can be avoided by passing ```--no-build``` property to dotnet.exe, which is the equivalent of setting ```<NoBuild>true</NoBuild> ``` in your project file, along with setting ```<IncludeBuildOutput>false</IncludeBuildOutput> ``` in the project file</span></span>

<span data-ttu-id="fb149-285">Un exemple d’un fichier csproj pour compresser un fichier nuspec est :</span><span class="sxs-lookup"><span data-stu-id="fb149-285">An example of a csproj file to pack a nuspec file is:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <NoBuild>true</NoBuild>
    <IncludeBuildOutput>false</IncludeBuildOutput>
    <NuspecFile>PATH_TO_NUSPEC_FILE</NuspecFile>
    <NuspecProperties>add nuspec properties here</NuspecProperties>
    <NuspecBasePath>optional to provide</NuspecBasePath>
  </PropertyGroup>
</Project>
```

### <a name="advanced-extension-points-to-create-customized-package"></a><span data-ttu-id="fb149-286">Avancée des points d’extension pour créer le package personnalisé</span><span class="sxs-lookup"><span data-stu-id="fb149-286">Advanced extension points to create customized package</span></span>

<span data-ttu-id="fb149-287">Le `pack` cible fournit deux points d’extension qui s’exécutent dans la build spécifique du framework cible interne,.</span><span class="sxs-lookup"><span data-stu-id="fb149-287">The `pack` target provides two extension points that run in the inner, target framework specific build.</span></span> <span data-ttu-id="fb149-288">Les points d’extension prise en charge inclusion de contenu spécifique du framework cible et les assemblys dans un package :</span><span class="sxs-lookup"><span data-stu-id="fb149-288">The extension points support including target framework specific content and assemblies into a package:</span></span>

- <span data-ttu-id="fb149-289">`TargetsForTfmSpecificBuildOutput` cible : utilisation de fichiers à l’intérieur de la `lib` dossier ou un dossier spécifié à l’aide `BuildOutputTargetFolder`.</span><span class="sxs-lookup"><span data-stu-id="fb149-289">`TargetsForTfmSpecificBuildOutput` target: Use for files inside the `lib` folder or a folder specified using `BuildOutputTargetFolder`.</span></span>
- <span data-ttu-id="fb149-290">`TargetsForTfmSpecificContentInPackage` cible : utilisation de fichiers en dehors de la `BuildOutputTargetFolder`.</span><span class="sxs-lookup"><span data-stu-id="fb149-290">`TargetsForTfmSpecificContentInPackage` target: Use for files outside the `BuildOutputTargetFolder`.</span></span>

#### <a name="targetsfortfmspecificbuildoutput"></a><span data-ttu-id="fb149-291">TargetsForTfmSpecificBuildOutput</span><span class="sxs-lookup"><span data-stu-id="fb149-291">TargetsForTfmSpecificBuildOutput</span></span>

<span data-ttu-id="fb149-292">Écrire une cible personnalisée et spécifiez-le comme valeur de la `$(TargetsForTfmSpecificBuildOutput)` propriété.</span><span class="sxs-lookup"><span data-stu-id="fb149-292">Write a custom target and specify it as the value of the `$(TargetsForTfmSpecificBuildOutput)` property.</span></span> <span data-ttu-id="fb149-293">Pour tous les fichiers qui doivent passer dans le `BuildOutputTargetFolder` (lib par défaut), la cible doit écrire ces fichiers dans un ItemGroup `BuildOutputInPackage` et définissez les deux valeurs de métadonnées suivantes :</span><span class="sxs-lookup"><span data-stu-id="fb149-293">For any files that need to go into the `BuildOutputTargetFolder` (lib by default), the target should write those files into the ItemGroup `BuildOutputInPackage` and set the following two metadata values:</span></span>

- <span data-ttu-id="fb149-294">`FinalOutputPath`: Le chemin d’accès absolu du fichier ; Si n’est fourni, l’identité est utilisée pour évaluer le chemin d’accès source.</span><span class="sxs-lookup"><span data-stu-id="fb149-294">`FinalOutputPath`: The absolute path of the file; if not provided, the Identity is used to evaluate source path.</span></span>
- <span data-ttu-id="fb149-295">`TargetPath`: (Facultatif) défini quand le fichier doit aller dans un sous-dossier au sein de `lib\<TargetFramework>` , tels que les assemblys satellites disponibles vont dans leurs dossiers de culture respectifs.</span><span class="sxs-lookup"><span data-stu-id="fb149-295">`TargetPath`:  (Optional) Set when the file needs to go into a subfolder within `lib\<TargetFramework>` , like satellite assemblies that go under their respective culture folders.</span></span> <span data-ttu-id="fb149-296">La valeur par défaut est le nom du fichier.</span><span class="sxs-lookup"><span data-stu-id="fb149-296">Defaults to the name of the file.</span></span>

<span data-ttu-id="fb149-297">Exemple :</span><span class="sxs-lookup"><span data-stu-id="fb149-297">Example:</span></span>

```xml
<PropertyGroup>
  <TargetsForTfmSpecificBuildOutput>$(TargetsForTfmSpecificBuildOutput);GetMyPackageFiles</TargetsForTfmSpecificBuildOutput>
</PropertyGroup>

<Target Name="GetMyPackageFiles">
  <ItemGroup>
    <BuildOutputInPackage Include="$(OutputPath)cs\$(AssemblyName).resources.dll">
        <TargetPath>cs</TargetPath>
    </BuildOutputInPackage>
  </ItemGroup>
</Target>
```

#### <a name="targetsfortfmspecificcontentinpackage"></a><span data-ttu-id="fb149-298">TargetsForTfmSpecificContentInPackage</span><span class="sxs-lookup"><span data-stu-id="fb149-298">TargetsForTfmSpecificContentInPackage</span></span>

<span data-ttu-id="fb149-299">Écrire une cible personnalisée et spécifiez-le comme valeur de la `$(TargetsForTfmSpecificContentInPackage)` propriété.</span><span class="sxs-lookup"><span data-stu-id="fb149-299">Write a custom target and specify it as the value of the `$(TargetsForTfmSpecificContentInPackage)` property.</span></span> <span data-ttu-id="fb149-300">Pour tous les fichiers à inclure dans le package, la cible doit écrire ces fichiers dans un ItemGroup `TfmSpecificPackageFile` et définir les métadonnées facultatives suivantes :</span><span class="sxs-lookup"><span data-stu-id="fb149-300">For any files to include in the package, the target should write those files into the ItemGroup `TfmSpecificPackageFile` and set the following optional metadata:</span></span>

- <span data-ttu-id="fb149-301">`PackagePath`: Chemin d’accès où le fichier doit être sortie dans le package.</span><span class="sxs-lookup"><span data-stu-id="fb149-301">`PackagePath`: Path where the file should be output in the package.</span></span> <span data-ttu-id="fb149-302">NuGet émet un avertissement si plus d’un fichier est ajouté à la même chemin d’accès du package.</span><span class="sxs-lookup"><span data-stu-id="fb149-302">NuGet issues a warning if more than one file is added to the same package path.</span></span>
- <span data-ttu-id="fb149-303">`BuildAction`: L’action de génération à assigner au fichier, requis uniquement si le chemin d’accès du package est dans le `contentFiles` dossier.</span><span class="sxs-lookup"><span data-stu-id="fb149-303">`BuildAction`: The build action to assign to the file, required only if the package path is in the `contentFiles` folder.</span></span> <span data-ttu-id="fb149-304">Valeur par défaut est « None ».</span><span class="sxs-lookup"><span data-stu-id="fb149-304">Defaults to "None".</span></span>

<span data-ttu-id="fb149-305">Voici un exemple :</span><span class="sxs-lookup"><span data-stu-id="fb149-305">An example:</span></span>
```xml
<PropertyGroup>
  <TargetsForTfmSpecificContentInPackage>$(TargetsForTfmSpecificContentInPackage);CustomContentTarget</TargetsForTfmSpecificContentInPackage>
</PropertyGroup>

<Target Name="CustomContentTarget">
  <ItemGroup>
    <TfmSpecificPackageFile Include="abc.txt">
      <PackagePath>mycontent/$(TargetFramework)</PackagePath>
    </TfmSpecificPackageFile>
    <TfmSpecificPackageFile Include="Extensions/ext.txt" Condition="'$(TargetFramework)' == 'net46'">
      <PackagePath>net46content</PackagePath>
    </TfmSpecificPackageFile>  
  </ItemGroup>
</Target>  
```

## <a name="restore-target"></a><span data-ttu-id="fb149-306">Cible restore</span><span class="sxs-lookup"><span data-stu-id="fb149-306">restore target</span></span>

<span data-ttu-id="fb149-307">`MSBuild /t:restore` (que `nuget restore` et `dotnet restore` utilisent avec les projets .NET Core) restaure les packages référencés dans le fichier projet en effectuant les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="fb149-307">`MSBuild /t:restore` (which `nuget restore` and `dotnet restore` use with .NET Core projects), restores packages referenced in the project file as follows:</span></span>

1. <span data-ttu-id="fb149-308">Lire toutes les références entre projets</span><span class="sxs-lookup"><span data-stu-id="fb149-308">Read all project to project references</span></span>
1. <span data-ttu-id="fb149-309">Lire les propriétés du projet pour trouver le dossier intermédiaire et les versions cibles de .NET Framework</span><span class="sxs-lookup"><span data-stu-id="fb149-309">Read the project properties to find the intermediate folder and target frameworks</span></span>
1. <span data-ttu-id="fb149-310">Passer des données msbuild à NuGet.Build.Tasks.dll</span><span class="sxs-lookup"><span data-stu-id="fb149-310">Pass msbuild data to NuGet.Build.Tasks.dll</span></span>
1. <span data-ttu-id="fb149-311">Exécuter la restauration</span><span class="sxs-lookup"><span data-stu-id="fb149-311">Run restore</span></span>
1. <span data-ttu-id="fb149-312">Télécharger les packages</span><span class="sxs-lookup"><span data-stu-id="fb149-312">Download packages</span></span>
1. <span data-ttu-id="fb149-313">Écrire le fichier de ressources, les cibles et les propriétés</span><span class="sxs-lookup"><span data-stu-id="fb149-313">Write assets file, targets, and props</span></span>

<span data-ttu-id="fb149-314">Le `restore` cibler works **uniquement** pour les projets utilisant le format PackageReference.</span><span class="sxs-lookup"><span data-stu-id="fb149-314">The `restore` target works **only** for projects using the PackageReference format.</span></span> <span data-ttu-id="fb149-315">C’est le cas **pas** Professionnel pour les projets utilisant le `packages.config` format ; utiliser [restauration nuget](../tools/cli-ref-restore.md) à la place.</span><span class="sxs-lookup"><span data-stu-id="fb149-315">It does **not** work for projects using the `packages.config` format; use [nuget restore](../tools/cli-ref-restore.md) instead.</span></span>

### <a name="restore-properties"></a><span data-ttu-id="fb149-316">Propriétés de restauration</span><span class="sxs-lookup"><span data-stu-id="fb149-316">Restore properties</span></span>

<span data-ttu-id="fb149-317">Des paramètres de restauration supplémentaires peuvent provenir de propriétés MSBuild dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="fb149-317">Additional restore settings may come from MSBuild properties in the project file.</span></span> <span data-ttu-id="fb149-318">Des valeurs peuvent également être définies à partir de la ligne de commande à l’aide du commutateur `/p:` (consultez Exemples ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="fb149-318">Values can also be set from the command line using the `/p:` switch (see Examples below).</span></span>

| <span data-ttu-id="fb149-319">Propriété</span><span class="sxs-lookup"><span data-stu-id="fb149-319">Property</span></span> | <span data-ttu-id="fb149-320">Description</span><span class="sxs-lookup"><span data-stu-id="fb149-320">Description</span></span> |
|--------|--------|
| <span data-ttu-id="fb149-321">RestoreSources</span><span class="sxs-lookup"><span data-stu-id="fb149-321">RestoreSources</span></span> | <span data-ttu-id="fb149-322">Liste de sources de packages séparées par un point-virgule.</span><span class="sxs-lookup"><span data-stu-id="fb149-322">Semicolon-delimited list of package sources.</span></span> |
| <span data-ttu-id="fb149-323">RestorePackagesPath</span><span class="sxs-lookup"><span data-stu-id="fb149-323">RestorePackagesPath</span></span> | <span data-ttu-id="fb149-324">Chemin du dossier de packages de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fb149-324">User packages folder path.</span></span> |
| <span data-ttu-id="fb149-325">RestoreDisableParallel</span><span class="sxs-lookup"><span data-stu-id="fb149-325">RestoreDisableParallel</span></span> | <span data-ttu-id="fb149-326">Limite les téléchargements à un à la fois.</span><span class="sxs-lookup"><span data-stu-id="fb149-326">Limit downloads to one at a time.</span></span> |
| <span data-ttu-id="fb149-327">RestoreConfigFile</span><span class="sxs-lookup"><span data-stu-id="fb149-327">RestoreConfigFile</span></span> | <span data-ttu-id="fb149-328">Chemin à un fichier `Nuget.Config` à appliquer.</span><span class="sxs-lookup"><span data-stu-id="fb149-328">Path to a `Nuget.Config` file to apply.</span></span> |
| <span data-ttu-id="fb149-329">RestoreNoCache</span><span class="sxs-lookup"><span data-stu-id="fb149-329">RestoreNoCache</span></span> | <span data-ttu-id="fb149-330">Si la valeur est true, permet d’éviter l’utilisation de packages de mise en cache.</span><span class="sxs-lookup"><span data-stu-id="fb149-330">If true, avoids using cached packages.</span></span> <span data-ttu-id="fb149-331">Consultez [gérer les packages globaux et les dossiers de cache](../consume-packages/managing-the-global-packages-and-cache-folders.md).</span><span class="sxs-lookup"><span data-stu-id="fb149-331">See [Managing the global packages and cache folders](../consume-packages/managing-the-global-packages-and-cache-folders.md).</span></span> |
| <span data-ttu-id="fb149-332">RestoreIgnoreFailedSources</span><span class="sxs-lookup"><span data-stu-id="fb149-332">RestoreIgnoreFailedSources</span></span> | <span data-ttu-id="fb149-333">Si la valeur est true, ignore les sources de packages défectueuses ou manquantes.</span><span class="sxs-lookup"><span data-stu-id="fb149-333">If true, ignores failing or missing package sources.</span></span> |
| <span data-ttu-id="fb149-334">RestoreTaskAssemblyFile</span><span class="sxs-lookup"><span data-stu-id="fb149-334">RestoreTaskAssemblyFile</span></span> | <span data-ttu-id="fb149-335">Chemin d’accès à `NuGet.Build.Tasks.dll`.</span><span class="sxs-lookup"><span data-stu-id="fb149-335">Path to `NuGet.Build.Tasks.dll`.</span></span> |
| <span data-ttu-id="fb149-336">RestoreGraphProjectInput</span><span class="sxs-lookup"><span data-stu-id="fb149-336">RestoreGraphProjectInput</span></span> | <span data-ttu-id="fb149-337">Liste de projets à restaurer séparés par un point-virgule, qui doit contenir des chemins absolus.</span><span class="sxs-lookup"><span data-stu-id="fb149-337">Semicolon-delimited list of projects to restore, which should contain absolute paths.</span></span> |
| <span data-ttu-id="fb149-338">RestoreOutputPath</span><span class="sxs-lookup"><span data-stu-id="fb149-338">RestoreOutputPath</span></span> | <span data-ttu-id="fb149-339">Dossier de sortie qui est par défaut le dossier `obj`.</span><span class="sxs-lookup"><span data-stu-id="fb149-339">Output folder, defaulting to the `obj` folder.</span></span> |

#### <a name="examples"></a><span data-ttu-id="fb149-340">Exemples</span><span class="sxs-lookup"><span data-stu-id="fb149-340">Examples</span></span>

<span data-ttu-id="fb149-341">Ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="fb149-341">Command line:</span></span>

```cli
msbuild /t:restore /p:RestoreConfigFile=<path>
```

<span data-ttu-id="fb149-342">Fichier projet :</span><span class="sxs-lookup"><span data-stu-id="fb149-342">Project file:</span></span>

```xml
<PropertyGroup>
    <RestoreIgnoreFailedSource>true</RestoreIgnoreFailedSource>
</PropertyGroup>
```

### <a name="restore-outputs"></a><span data-ttu-id="fb149-343">Sorties de restauration</span><span class="sxs-lookup"><span data-stu-id="fb149-343">Restore outputs</span></span>

<span data-ttu-id="fb149-344">La restauration crée les fichiers suivants dans le dossier `obj` de build :</span><span class="sxs-lookup"><span data-stu-id="fb149-344">Restore creates the following files in the build `obj` folder:</span></span>

| <span data-ttu-id="fb149-345">Fichier</span><span class="sxs-lookup"><span data-stu-id="fb149-345">File</span></span> | <span data-ttu-id="fb149-346">Description</span><span class="sxs-lookup"><span data-stu-id="fb149-346">Description</span></span> |
|--------|--------|
| `project.assets.json` | <span data-ttu-id="fb149-347">Contient le graphique de dépendance de toutes les références de package.</span><span class="sxs-lookup"><span data-stu-id="fb149-347">Contains the dependency graph of all package references.</span></span> |
| `{projectName}.projectFileExtension.nuget.g.props` | <span data-ttu-id="fb149-348">Références à des propriétés MSBuild contenues dans des packages</span><span class="sxs-lookup"><span data-stu-id="fb149-348">References to MSBuild props contained in packages</span></span> |
| `{projectName}.projectFileExtension.nuget.g.targets` | <span data-ttu-id="fb149-349">Références à des cibles MSBuild contenues dans des packages</span><span class="sxs-lookup"><span data-stu-id="fb149-349">References to MSBuild targets contained in packages</span></span> |

### <a name="packagetargetfallback"></a><span data-ttu-id="fb149-350">PackageTargetFallback</span><span class="sxs-lookup"><span data-stu-id="fb149-350">PackageTargetFallback</span></span>

<span data-ttu-id="fb149-351">L’élément `PackageTargetFallback` vous permet de spécifier un jeu de cibles compatibles à utiliser lors de la restauration des packages.</span><span class="sxs-lookup"><span data-stu-id="fb149-351">The `PackageTargetFallback` element allows you to specify a set of compatible targets to be used when restoring packages.</span></span> <span data-ttu-id="fb149-352">Il est conçu pour permettre aux packages qui utilisent un [moniker du Framework cible](../reference/target-frameworks.md) dotnet de fonctionner avec des packages compatibles qui ne déclarent pas de moniker du Framework cible dotnet.</span><span class="sxs-lookup"><span data-stu-id="fb149-352">It's designed to allow packages that use a dotnet [TxM](../reference/target-frameworks.md) to work with compatible packages that don't declare a dotnet TxM.</span></span> <span data-ttu-id="fb149-353">Autrement dit, si votre projet utilise le moniker du Framework cible dotnet, tous les packages dont il dépend doivent également avoir un moniker du Framework cible dotnet, sauf si vous ajoutez `<PackageTargetFallback>` à votre projet pour permettre aux plateformes autres que dotnet d’être compatibles avec dotnet.</span><span class="sxs-lookup"><span data-stu-id="fb149-353">That is, if your project uses the dotnet TxM, then all the packages it depends on must also have a dotnet TxM, unless you add the `<PackageTargetFallback>` to your project in order to allow non-dotnet platforms to be compatible with dotnet.</span></span>

<span data-ttu-id="fb149-354">Par exemple, si le projet utilise le moniker du Framework cible `netstandard1.6` et qu’un package dépendant ne contient que `lib/net45/a.dll` et `lib/portable-net45+win81/a.dll`, la génération du projet échoue.</span><span class="sxs-lookup"><span data-stu-id="fb149-354">For example, if the project is using the `netstandard1.6` TxM, and a dependent package contains only `lib/net45/a.dll` and `lib/portable-net45+win81/a.dll`, then the project will fail to build.</span></span> <span data-ttu-id="fb149-355">Si vous souhaitez présenter la dernière DLL, vous pouvez ajouter un `PackageTargetFallback` comme suit pour dire que la DLL `portable-net45+win81` est compatible :</span><span class="sxs-lookup"><span data-stu-id="fb149-355">If what you want to bring in is the latter DLL, then you can add a `PackageTargetFallback` as follows to say that the `portable-net45+win81` DLL is compatible:</span></span>

```xml
<PackageTargetFallback Condition="'$(TargetFramework)'=='netstandard1.6'">
    portable-net45+win81
</PackageTargetFallback>
```

<span data-ttu-id="fb149-356">Pour déclarer une solution de secours pour toutes les cibles de votre projet, omettez l’attribut `Condition`.</span><span class="sxs-lookup"><span data-stu-id="fb149-356">To declare a fallback for all targets in your project, leave off the `Condition` attribute.</span></span> <span data-ttu-id="fb149-357">Vous pouvez également étendre tout `PackageTargetFallback` en incluant `$(PackageTargetFallback)` comme indiqué ici :</span><span class="sxs-lookup"><span data-stu-id="fb149-357">You can also extend any existing `PackageTargetFallback` by including `$(PackageTargetFallback)` as shown here:</span></span>

```xml
<PackageTargetFallback>
    $(PackageTargetFallback);portable-net45+win81
</PackageTargetFallback >
```

### <a name="replacing-one-library-from-a-restore-graph"></a><span data-ttu-id="fb149-358">Remplacement d’une bibliothèque à partir d’un graphique de restauration</span><span class="sxs-lookup"><span data-stu-id="fb149-358">Replacing one library from a restore graph</span></span>

<span data-ttu-id="fb149-359">Si une restauration présente un assembly incorrect, il est possible d’exclure cette option par défaut de package et de la remplacer par votre propre choix.</span><span class="sxs-lookup"><span data-stu-id="fb149-359">If a restore is bringing the wrong assembly, it's possible to exclude that packages default choice, and replace it with your own choice.</span></span> <span data-ttu-id="fb149-360">Commencez par exclure toutes les ressources avec un `PackageReference` de niveau supérieur :</span><span class="sxs-lookup"><span data-stu-id="fb149-360">First with a top level `PackageReference`, exclude all assets:</span></span>

```xml
<PackageReference Include="Newtonsoft.Json" Version="9.0.1">
  <ExcludeAssets>All</ExcludeAssets>
</PackageReference>
```

<span data-ttu-id="fb149-361">Ajoutez ensuite votre propre référence à la copie locale appropriée de la DLL :</span><span class="sxs-lookup"><span data-stu-id="fb149-361">Next, add your own reference to the appropriate local copy of the DLL:</span></span>

```xml
<Reference Include="Newtonsoft.Json.dll" />
```

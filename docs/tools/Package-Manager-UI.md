---
title: Référence de l’interface utilisateur du Gestionnaire de Package NuGet
description: Instructions sur l’utilisation de l’UI Gestionnaire de Package NuGet dans Visual Studio pour travailler avec les packages NuGet.
author: karann-msft
ms.author: karann
ms.date: 12/08/2017
ms.topic: conceptual
f1_keywords:
- vs.toolsoptionspages.nuget_package_manager
- vs.toolsoptionspages.nuget_package_manager.general
- vs.toolsoptionspages.nuget_package_manager.package_sources
- vs.nuget.packagemanager.ui
ms.openlocfilehash: 651bbe63ec95fcedb8e9504022d08d6ba7f9219e
ms.sourcegitcommit: 1d1406764c6af5fb7801d462e0c4afc9092fa569
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43551755"
---
# <a name="nuget-package-manager-ui"></a><span data-ttu-id="04764-103">Interface utilisateur du Gestionnaire de Package NuGet</span><span class="sxs-lookup"><span data-stu-id="04764-103">NuGet Package Manager UI</span></span>

<span data-ttu-id="04764-104">Le Gestionnaire de Package NuGet UI dans Visual Studio sur Windows vous permet facilement installer, désinstaller et mettre à jour les packages NuGet dans les projets et solutions.</span><span class="sxs-lookup"><span data-stu-id="04764-104">The NuGet Package Manager UI in Visual Studio on Windows allows you to easily install, uninstall, and update NuGet packages in projects and solutions.</span></span> <span data-ttu-id="04764-105">Pour une expérience dans Visual Studio pour Mac, consultez [, y compris un package NuGet dans votre projet](/visualstudio/mac/nuget-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="04764-105">For the experience in Visual Studio for Mac, see [Including a NuGet package in your project](/visualstudio/mac/nuget-walkthrough).</span></span> <span data-ttu-id="04764-106">Le Gestionnaire de Package UI n’est pas inclus avec Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="04764-106">The Package Manager UI is not included with Visual Studio Code.</span></span>

<span data-ttu-id="04764-107">Dans cette rubrique :</span><span class="sxs-lookup"><span data-stu-id="04764-107">In this topic:</span></span>

- [<span data-ttu-id="04764-108">Recherche et installation d’un package (onglet Parcourir)</span><span class="sxs-lookup"><span data-stu-id="04764-108">Finding and installing a package (Browse tab)</span></span>](#finding-and-installing-a-package)
- [<span data-ttu-id="04764-109">Désinstallation d’un package (onglet installé)</span><span class="sxs-lookup"><span data-stu-id="04764-109">Uninstalling a package (Installed tab)</span></span>](#uninstalling-a-package)
- <span data-ttu-id="04764-110">[La mise à jour d’un package (onglets installé et les mises à jour)](#updating-a-package) (inclut le [« Référencée implicitement par un SDK » ou « AutoReferenced » message](#implicit_reference))</span><span class="sxs-lookup"><span data-stu-id="04764-110">[Updating a package (Installed and Updates tabs)](#updating-a-package) (includes the ["Implicitly referenced by an SDK" or "AutoReferenced" message](#implicit_reference))</span></span>
- <span data-ttu-id="04764-111">[La gestion des packages pour la solution](#managing-packages-for-the-solution) (travailler avec plusieurs projets en même temps).</span><span class="sxs-lookup"><span data-stu-id="04764-111">[Managing packages for the solution](#managing-packages-for-the-solution) (working with multiple projects at the same time).</span></span>
- [<span data-ttu-id="04764-112">Sources de package</span><span class="sxs-lookup"><span data-stu-id="04764-112">Package sources</span></span>](#package-sources)
- [<span data-ttu-id="04764-113">Contrôlent les Options du Gestionnaire de package</span><span class="sxs-lookup"><span data-stu-id="04764-113">Package manager Options control</span></span>](#package-manager-options-control)

> [!Note]
> <span data-ttu-id="04764-114">Si vous êtes pas le Gestionnaire de Package NuGet dans Visual Studio 2015, consultez **Outils > Extensions et mises à jour...**  et recherchez le *Gestionnaire de Package NuGet* extension.</span><span class="sxs-lookup"><span data-stu-id="04764-114">If you're missing the NuGet Package Manager in Visual Studio 2015, check **Tools > Extensions and Updates...** and search for the *NuGet Package Manager* extension.</span></span> <span data-ttu-id="04764-115">Si vous ne parvenez pas à utiliser le programme d’installation des extensions dans Visual Studio, téléchargez l’extension directement à partir de [ https://dist.nuget.org/index.html ](https://dist.nuget.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="04764-115">If you're unable to use the extensions installer in Visual Studio, download the extension directly from [https://dist.nuget.org/index.html](https://dist.nuget.org/index.html).</span></span>
>
> <span data-ttu-id="04764-116">Visual Studio 2017, NuGet et le Gestionnaire de Package NuGet sont installées automatiquement avec n’importe quelle. Charges de travail liées NET.</span><span class="sxs-lookup"><span data-stu-id="04764-116">In Visual Studio 2017, NuGet and the NuGet Package Manager are automatically installed with any .NET-related workloads.</span></span> <span data-ttu-id="04764-117">Installer individuellement en sélectionnant le **composants individuels > Outils de Code > Gestionnaire de package NuGet** option dans le programme d’installation de Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="04764-117">Install it individually by selecting the **Individual components > Code tools > NuGet package manager** option in the Visual Studio 2017 installer.</span></span>

## <a name="finding-and-installing-a-package"></a><span data-ttu-id="04764-118">Recherche et installation d’un package</span><span class="sxs-lookup"><span data-stu-id="04764-118">Finding and installing a package</span></span>

1. <span data-ttu-id="04764-119">Dans **l’Explorateur de solutions**, avec le bouton droit soit **références** ou un projet, puis sélectionnez **gérer les Packages NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="04764-119">In **Solution Explorer**, right-click either **References**  or a project and select **Manage NuGet Packages...**.</span></span>

    ![Gérer l’option de menu de Packages NuGet](media/ManagePackagesUICommand.png)

1. <span data-ttu-id="04764-121">Le **Parcourir** onglet affiche les packages par popularité à partir de la source actuellement sélectionnée (consultez [sources de packages](#package-sources)).</span><span class="sxs-lookup"><span data-stu-id="04764-121">The **Browse** tab displays packages by popularity from the currently selected source (see [package sources](#package-sources)).</span></span> <span data-ttu-id="04764-122">Recherchez un package spécifique à l’aide de la zone de recherche dans la coin supérieur gauche.</span><span class="sxs-lookup"><span data-stu-id="04764-122">Search for a specific package using the search box on the upper left.</span></span> <span data-ttu-id="04764-123">Sélectionner un package à partir de la liste pour afficher ses informations, ce qui permet également la **installer** bouton avec une liste déroulante Sélection de la version.</span><span class="sxs-lookup"><span data-stu-id="04764-123">Select a package from the list to display its information, which also enables the **Install** button along with a version-selection drop-down.</span></span>

    ![Gérer l’onglet Parcourir de boîte de dialogue de Packages NuGet](media/Search.png)

1. <span data-ttu-id="04764-125">Sélectionnez la version souhaitée dans la liste déroulante et sélectionnez **installer**.</span><span class="sxs-lookup"><span data-stu-id="04764-125">Select the desired version from the drop-down and select **Install**.</span></span> <span data-ttu-id="04764-126">Visual Studio installe le package et ses dépendances dans le projet.</span><span class="sxs-lookup"><span data-stu-id="04764-126">Visual Studio installs the package and its dependencies into the project.</span></span> <span data-ttu-id="04764-127">Vous pouvez être invité à accepter les termes du contrat de licence.</span><span class="sxs-lookup"><span data-stu-id="04764-127">You may be asked to accept license terms.</span></span> <span data-ttu-id="04764-128">Lors de l’installation est terminée, les packages ajoutés s’affichent sur le **installé** onglet. Les packages sont également répertoriés dans le **références** nœud de l’Explorateur de solutions, indiquant que vous puissiez y faire référence dans le projet avec `using` instructions.</span><span class="sxs-lookup"><span data-stu-id="04764-128">When installation is complete, the added packages appear on the **Installed** tab. Packages are also listed in the **References** node of Solution Explorer, indicating that you can refer to them in the project with `using` statements.</span></span>

    ![Références dans l’Explorateur de solutions](media/References.png)

> [!Tip]
> <span data-ttu-id="04764-130">Pour inclure les versions préliminaires dans la recherche et de proposer des versions préliminaires dans la version de liste déroulante, sélectionnez le **inclure la version préliminaire** option.</span><span class="sxs-lookup"><span data-stu-id="04764-130">To include prerelease versions in the search, and to make prerelease versions available in the version drop-down, select the **Include prerelease** option.</span></span>

## <a name="uninstalling-a-package"></a><span data-ttu-id="04764-131">Désinstaller un package</span><span class="sxs-lookup"><span data-stu-id="04764-131">Uninstalling a package</span></span>

1. <span data-ttu-id="04764-132">Dans **l’Explorateur de solutions**, avec le bouton droit soit **références** ou le projet de votre choix, puis sélectionnez **gérer les Packages NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="04764-132">In **Solution Explorer**, right-click either **References** or the desired project, and select **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="04764-133">Sélectionnez le **installé** onglet.</span><span class="sxs-lookup"><span data-stu-id="04764-133">Select the **Installed** tab.</span></span>
1. <span data-ttu-id="04764-134">Sélectionnez le package à désinstaller (à l’aide de la recherche pour filtrer la liste si nécessaire), puis sélectionnez **désinstallation**.</span><span class="sxs-lookup"><span data-stu-id="04764-134">Select the package to uninstall (using search to filter the list if necessary) and select **Uninstall**.</span></span>

    ![Désinstaller un package](media/UninstallPackage.png)

1. <span data-ttu-id="04764-136">Notez que le **inclure la version préliminaire** et **source du Package** contrôles n’ont aucun effet lors de la désinstallation des packages.</span><span class="sxs-lookup"><span data-stu-id="04764-136">Note that the **Include prerelease** and **Package source** controls have no effect when uninstalling packages.</span></span>

## <a name="updating-a-package"></a><span data-ttu-id="04764-137">Mettre à jour un package</span><span class="sxs-lookup"><span data-stu-id="04764-137">Updating a package</span></span>

1. <span data-ttu-id="04764-138">Dans **l’Explorateur de solutions**, avec le bouton droit soit **références** ou le projet de votre choix, puis sélectionnez **gérer les Packages NuGet...** . (Dans les projets de site web, cliquez sur le **Bin** dossier.)</span><span class="sxs-lookup"><span data-stu-id="04764-138">In **Solution Explorer**, right-click either **References** or the desired project, and select **Manage NuGet Packages...**. (In web site projects, right-click the **Bin** folder.)</span></span>
1. <span data-ttu-id="04764-139">Sélectionnez le **mises à jour** onglet pour afficher les packages qui ont des mises à jour disponibles à partir des sources de package sélectionné.</span><span class="sxs-lookup"><span data-stu-id="04764-139">Select the **Updates** tab to see packages that have available updates from the selected package sources.</span></span> <span data-ttu-id="04764-140">Sélectionnez **inclure la version préliminaire** à inclure des packages de version préliminaire dans la liste mise à jour.</span><span class="sxs-lookup"><span data-stu-id="04764-140">Select **Include prerelease** to include prerelease packages in the update list.</span></span>
1. <span data-ttu-id="04764-141">Sélectionnez le package à mettre à jour, sélectionnez la version souhaitée dans la liste déroulante à droite, puis **mettre à jour**.</span><span class="sxs-lookup"><span data-stu-id="04764-141">Select the package to update, select the desired version from the drop-down on the right, and select **Update**.</span></span>

    ![Mettre à jour un package](media/UpdatePackages.png)

1. <a name="implicit_reference"></a><span data-ttu-id="04764-143">Pour certains packages, les **mise à jour** bouton est désactivé et un message s’affiche indiquant qu’il est « implicitement référencé par un SDK » (ou « AutoReferenced »).</span><span class="sxs-lookup"><span data-stu-id="04764-143">For some packages, the **Update** button is disabled and a message appears saying that it's "Implicitly referenced by an SDK" (or "AutoReferenced").</span></span> <span data-ttu-id="04764-144">Le message indique que le package, telles que Microsoft.NETCore.App ou Microsoft.NETStandard.Library, fait partie d’un framework plus grand ou le Kit de développement logiciel et ne doit-elle pas être mis à jour indépendamment.</span><span class="sxs-lookup"><span data-stu-id="04764-144">The message indicates that the package, such as Microsoft.NETCore.App or Microsoft.NETStandard.Library, is part of a larger framework or SDK and should not be updated independently.</span></span> <span data-ttu-id="04764-145">(Ces packages sont signalés en interne par `<IsImplicitlyDefined>True</IsImplicitlyDefined>`.) Pour mettre à jour le package, mettez à jour le Kit de développement logiciel auquel il appartient, déduire le Kit de développement de conteneur à partir du nom de package.</span><span class="sxs-lookup"><span data-stu-id="04764-145">(Such packages are internally marked with `<IsImplicitlyDefined>True</IsImplicitlyDefined>`.) To update the package, update the SDK to which it belongs, inferring the containing SDK from the package name.</span></span> <span data-ttu-id="04764-146">Par exemple, un package comme Microsoft.NETCore.App fait partie du SDK .NET Core, par conséquent, vous devez mettre à jour votre installation du SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="04764-146">For example, a package like Microsoft.NETCore.App is part of the .NET Core SDK, therefore you need to update your  .NET Core SDK installation.</span></span>

    ![Exemple de package la mention implicitement références ou AutoReferenced](media/PackageManagerUIAutoReferenced.png)

1. <span data-ttu-id="04764-148">Pour mettre à jour plusieurs packages vers leurs versions les plus récentes, sélectionnez-les dans la liste, puis sélectionnez le **mettre à jour** bouton au-dessus de la liste.</span><span class="sxs-lookup"><span data-stu-id="04764-148">To update multiple packages to their newest versions, select  them in the list and select the **Update** button above the list.</span></span>
1. <span data-ttu-id="04764-149">Vous pouvez également mettre à jour un package individuel à partir de la **installé** onglet. Dans ce cas, les détails pour le package incluent un sélecteur de version (soumis à la **inclure la version préliminaire** option) et un **mise à jour** bouton.</span><span class="sxs-lookup"><span data-stu-id="04764-149">You can also update an individual package from the **Installed** tab. In this case, the details for the package include a version selector (subject to the **Include prerelease** option) and an **Update** button.</span></span>

## <a name="managing-packages-for-the-solution"></a><span data-ttu-id="04764-150">La gestion des packages pour la solution</span><span class="sxs-lookup"><span data-stu-id="04764-150">Managing packages for the solution</span></span>

<span data-ttu-id="04764-151">La gestion des packages pour une solution sont un moyen pratique de travailler avec plusieurs projets simultanément.</span><span class="sxs-lookup"><span data-stu-id="04764-151">Managing packages for a solution is a convenient means to work with multiple projects simultaneously.</span></span>

1. <span data-ttu-id="04764-152">Sélectionnez le **Outils > Gestionnaire de Package NuGet > Gérer les Packages NuGet pour la Solution...**  menu de commande, ou avec le bouton droit de la solution et sélectionnez **gérer les Packages NuGet...** :</span><span class="sxs-lookup"><span data-stu-id="04764-152">Select the **Tools > NuGet Package Manager > Manage NuGet Packages for Solution...** menu command, or right-click the solution and select **Manage NuGet Packages...**:</span></span>

    ![Gérer les packages NuGet pour la solution](media/ManagePackagesSolutionUICommand.png)

1. <span data-ttu-id="04764-154">Lors de la gestion des packages pour la solution, l’interface utilisateur vous permet de sélectionner les projets qui sont affectés par les opérations :</span><span class="sxs-lookup"><span data-stu-id="04764-154">When managing packages for the solution, the UI lets you select the projects that are affected by the operations:</span></span>

    ![Sélecteur de projet lors de la gestion des packages pour la solution](media/SolutionPackagesUI.png)

### <a name="consolidate-tab"></a><span data-ttu-id="04764-156">Consolider onglet</span><span class="sxs-lookup"><span data-stu-id="04764-156">Consolidate tab</span></span>

<span data-ttu-id="04764-157">Les développeurs estiment généralement mauvaise pratique pour utiliser les différentes versions du même package NuGet sur différents projets dans la même solution.</span><span class="sxs-lookup"><span data-stu-id="04764-157">Developers typically consider it bad practice to use different versions of the same NuGet package across different projects in the same solution.</span></span> <span data-ttu-id="04764-158">Lorsque vous choisissez de gérer les packages pour une solution, le Gestionnaire de Package UI fournit un **consolider** onglet sur lequel vous pouvez facilement voir où les packages avec des numéros de version distincts sont utilisés par différents projets dans la solution :</span><span class="sxs-lookup"><span data-stu-id="04764-158">When you choose to manage packages for a solution, the Package Manager UI provides a **Consolidate** tab on which you can easily see where packages with distinct version numbers are used by different projects in the solution:</span></span>

![Onglet consolider de l’interface utilisateur de gestionnaire de package](media/ConsolidateTab.png)

<span data-ttu-id="04764-160">Dans cet exemple, le projet ClassLibrary1 est à l’aide de EntityFramework 6.2.0, tandis que ConsoleApp1 est à l’aide de EntityFramework 6.1.0.</span><span class="sxs-lookup"><span data-stu-id="04764-160">In this example, the ClassLibrary1 project is using EntityFramework 6.2.0, whereas ConsoleApp1 is using EntityFramework 6.1.0.</span></span> <span data-ttu-id="04764-161">Pour consolider les versions de package, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="04764-161">To consolidate package versions, do the following:</span></span>

- <span data-ttu-id="04764-162">Sélectionnez les projets à mettre à jour dans la liste des projets.</span><span class="sxs-lookup"><span data-stu-id="04764-162">Select the projects to update in the project list.</span></span>
- <span data-ttu-id="04764-163">Sélectionnez la version à utiliser dans tous ces projets dans le **Version** contrôle, tel qu’Entity Framework 6.2.0.</span><span class="sxs-lookup"><span data-stu-id="04764-163">Select the version to use in all those projects in the **Version** control, such as EntityFramework 6.2.0.</span></span>
- <span data-ttu-id="04764-164">Sélectionnez le **installer** bouton.</span><span class="sxs-lookup"><span data-stu-id="04764-164">Select the **Install** button.</span></span>

<span data-ttu-id="04764-165">Le Gestionnaire de Package installe la version du package sélectionné dans tous les projets sélectionnés, après laquelle le package n’apparaît plus dans le **consolider** onglet.</span><span class="sxs-lookup"><span data-stu-id="04764-165">The Package Manager installs the selected package version into all selected projects, after which the package no longer appears on the **Consolidate** tab.</span></span>

## <a name="package-sources"></a><span data-ttu-id="04764-166">Sources de package</span><span class="sxs-lookup"><span data-stu-id="04764-166">Package sources</span></span>

<span data-ttu-id="04764-167">Pour modifier la source à partir de laquelle Visual Studio obtient les packages, sélectionnez un dans le sélecteur de source :</span><span class="sxs-lookup"><span data-stu-id="04764-167">To change the source from which Visual Studio obtains packages, select one from the source selector:</span></span>

![Sélecteur de source de package dans l’interface utilisateur du Gestionnaire de package](media/PackageSourceDropDown.png)

<span data-ttu-id="04764-169">Pour gérer les sources de package :</span><span class="sxs-lookup"><span data-stu-id="04764-169">To manage package sources:</span></span>

1. <span data-ttu-id="04764-170">Sélectionnez le **paramètres** icône dans le Gestionnaire de Package UI décrites ci-dessous ou utilisez le **Outils > Options** de commandes et accédez à **Gestionnaire de Package NuGet**:</span><span class="sxs-lookup"><span data-stu-id="04764-170">Select the **Settings** icon in the Package Manager UI outlined below or use the **Tools > Options** command and scroll to **NuGet Package Manager**:</span></span>

    ![Icône des paramètres de l’interface utilisateur package manager](media/PackageSourceSettings.png)

1. <span data-ttu-id="04764-172">Sélectionnez le **Sources de Package** nœud :</span><span class="sxs-lookup"><span data-stu-id="04764-172">Select the **Package Sources** node:</span></span>

    ![Options de Sources de package](media/options.png)

1. <span data-ttu-id="04764-174">Pour ajouter une source, sélectionnez **+**, modifiez le nom, entrez l’URL ou le chemin d’accès dans le **Source** contrôler, puis sélectionnez **mise à jour**.</span><span class="sxs-lookup"><span data-stu-id="04764-174">To add a source, select **+**, edit the name, enter the URL or path in the **Source** control, and select  **Update**.</span></span> <span data-ttu-id="04764-175">La source apparaît maintenant dans le sélecteur de liste déroulante.</span><span class="sxs-lookup"><span data-stu-id="04764-175">The source now appears in the selector drop-down.</span></span>
1. <span data-ttu-id="04764-176">Pour modifier une source de package, sélectionnez-le, apporter des modifications dans le **nom** et **Source** cases, puis sélectionnez **mise à jour**.</span><span class="sxs-lookup"><span data-stu-id="04764-176">To change a package source, select it, make edits in the **Name** and **Source** boxes, and select **Update**.</span></span>
1. <span data-ttu-id="04764-177">Pour désactiver une source de package, désactivez la case à gauche du nom dans la liste.</span><span class="sxs-lookup"><span data-stu-id="04764-177">To disable a package source, clear the box to the left of the name in the list.</span></span>
1. <span data-ttu-id="04764-178">Pour supprimer une source de package, sélectionnez-le, puis le **X** bouton.</span><span class="sxs-lookup"><span data-stu-id="04764-178">To remove a package source, select it and then select the **X** button.</span></span>
1. <span data-ttu-id="04764-179">Utilisez le haut et flèche bas pour modifier l’ordre de priorité des sources de package.</span><span class="sxs-lookup"><span data-stu-id="04764-179">Use the up and down arrow buttons to change the priority order of the package sources.</span></span> <span data-ttu-id="04764-180">Visual Studio recherche ces sources dans l’ordre de priorité lors de la restauration des packages pour un projet.</span><span class="sxs-lookup"><span data-stu-id="04764-180">Visual Studio searches these sources in the priority order when restoring packages for a project.</span></span> <span data-ttu-id="04764-181">Pour plus d’informations, consultez [restauration des packages](../consume-packages/package-restore.md).</span><span class="sxs-lookup"><span data-stu-id="04764-181">For more information, see [Package restore](../consume-packages/package-restore.md).</span></span>

> [!Tip]
> <span data-ttu-id="04764-182">Si une source de package s’affiche de nouveau après l’avoir supprimée, devrait être répertoriée dans au niveau de l’ordinateur ou le niveau de l’utilisateur `NuGet.Config` fichiers.</span><span class="sxs-lookup"><span data-stu-id="04764-182">If a package source reappears after deleting it, it may be listed in a computer-level or user-level `NuGet.Config` files.</span></span> <span data-ttu-id="04764-183">Consultez [du comportement de NuGet configuration](../consume-packages/configuring-nuget-behavior.md) pour l’emplacement de ces fichiers, puis supprimer la source en modifiant les fichiers manuellement ou en utilisant le [nuget sources commande](../tools/nuget-exe-CLI-reference.md).</span><span class="sxs-lookup"><span data-stu-id="04764-183">See [Configuring NuGet behavior](../consume-packages/configuring-nuget-behavior.md) for the location of these files, then remove the source by editing the files manually or using the [nuget sources command](../tools/nuget-exe-CLI-reference.md).</span></span>

## <a name="package-manager-options-control"></a><span data-ttu-id="04764-184">Contrôlent les Options du Gestionnaire de package</span><span class="sxs-lookup"><span data-stu-id="04764-184">Package manager Options control</span></span>

<span data-ttu-id="04764-185">Lorsqu’un package est sélectionné, le Gestionnaire de Package UI affiche un petit, extensible **Options** contrôle sous le sélecteur de version (illustré ici à la fois réduit et développé).</span><span class="sxs-lookup"><span data-stu-id="04764-185">When a package is selected, the Package Manager UI displays a small, expandable **Options** control below the version selector (shown here both collapsed and expanded).</span></span> <span data-ttu-id="04764-186">Notez que, pour un projet, les types, uniquement le **afficher la fenêtre d’aperçu** option est fournie.</span><span class="sxs-lookup"><span data-stu-id="04764-186">Note that for some project types, only the **Show preview window** option is provided.</span></span>

![Options du Gestionnaire de package](media/PackageManagerUIOptions.png)

<span data-ttu-id="04764-188">Les sections suivantes décrivent ces options.</span><span class="sxs-lookup"><span data-stu-id="04764-188">The following sections explain these options.</span></span>

### <a name="show-preview-window"></a><span data-ttu-id="04764-189">Afficher la fenêtre d’aperçu</span><span class="sxs-lookup"><span data-stu-id="04764-189">Show preview window</span></span>

<span data-ttu-id="04764-190">Sélectionné, une fenêtre modale affiche ce qui les dépendances d’un package choisi avant que le package est installé :</span><span class="sxs-lookup"><span data-stu-id="04764-190">When selected, a modal window displays which the dependencies of a chosen package before the package is installed:</span></span>

![Boîte de dialogue Aperçu exemple](media/InstallPreviewDialog.png)

<!-- This is here because the link in the UI needs this anchor. See https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Clients/PackageManagement.UI/Xamls/OptionsControl.xaml -->
<a name="install-options"></a>

### <a name="install-and-update-options"></a><span data-ttu-id="04764-192">Installer et mettre à jour des Options</span><span class="sxs-lookup"><span data-stu-id="04764-192">Install and Update Options</span></span>

<span data-ttu-id="04764-193">(Non disponible pour tous les types de projet).</span><span class="sxs-lookup"><span data-stu-id="04764-193">(Not available for all project types.)</span></span>

<span data-ttu-id="04764-194">**Comportement de dépendance** configure la façon dont NuGet décide quelles versions des packages dépendants à installer :</span><span class="sxs-lookup"><span data-stu-id="04764-194">**Dependency behavior** configures how NuGet decides which versions of dependent packages to install:</span></span>

- <span data-ttu-id="04764-195">*Ignorer les dépendances* ignore l’installation de toutes les dépendances, ce qui interrompt généralement le package en cours d’installation.</span><span class="sxs-lookup"><span data-stu-id="04764-195">*Ignore dependencies* skips installing any dependencies, which typically breaks the package being installed.</span></span>
- <span data-ttu-id="04764-196">*La plus basse* [Default] installe la dépendance avec le numéro de version minimal qui répond aux exigences du package principal choisi.</span><span class="sxs-lookup"><span data-stu-id="04764-196">*Lowest* [Default] installs the dependency with the minimal version number that meets the requirements of the primary chosen package.</span></span>
- <span data-ttu-id="04764-197">*Correctif logiciel le plus élevé* installe la version avec les mêmes numéros de version principale et secondaire, mais le plus grand nombre de correctifs.</span><span class="sxs-lookup"><span data-stu-id="04764-197">*Highest Patch* installs the version with the same major and minor version numbers, but the highest patch number.</span></span> <span data-ttu-id="04764-198">Par exemple, si version 1.2.2 est spécifiée, la version la plus élevée qui commence par 1.2 sera être installée</span><span class="sxs-lookup"><span data-stu-id="04764-198">For example, if version 1.2.2 is specified then the highest version that starts with 1.2 will be installed</span></span>
- <span data-ttu-id="04764-199">*Mineur le plus élevé* installe la version avec le même numéro de version principale, mais le numéro mineur le plus élevé et le numéro du correctif.</span><span class="sxs-lookup"><span data-stu-id="04764-199">*Highest Minor* installs the version with the same major version number but the highest minor number and patch number.</span></span> <span data-ttu-id="04764-200">Si la version 1.2.2 est spécifiée, la version la plus élevée qui commence par 1 sera installée</span><span class="sxs-lookup"><span data-stu-id="04764-200">If version 1.2.2 is specified, then the highest version that starts with 1 will be installed</span></span>
- <span data-ttu-id="04764-201">*La plus élevée* installe la version disponible la plus récente du package.</span><span class="sxs-lookup"><span data-stu-id="04764-201">*Highest* installs the highest available version of the package.</span></span>

<span data-ttu-id="04764-202">**Action de conflit de fichiers** spécifie la façon dont NuGet doit gérer les packages qui existent déjà dans le projet ou l’ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="04764-202">**File conflict action** specifies how NuGet should handle packages that already exist in the project or local machine:</span></span>

- <span data-ttu-id="04764-203">*Invite* indique à NuGet demander s’il faut conserver ou remplacer les packages existants.</span><span class="sxs-lookup"><span data-stu-id="04764-203">*Prompt* instructs NuGet to ask whether to keep or overwrite existing packages.</span></span>
- <span data-ttu-id="04764-204">*Ignorer tout* indique à NuGet à ignorer le remplacement des packages existants.</span><span class="sxs-lookup"><span data-stu-id="04764-204">*Ignore All* instructs NuGet to skip overwriting any existing packages.</span></span>
- <span data-ttu-id="04764-205">*Remplacer tous les* indique à remplacer tous les packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="04764-205">*Overwrite All* instructs NuGet to overwrite any existing packages.</span></span>

<!-- This is here because the link in the UI needs this anchor. See https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Clients/PackageManagement.UI/Xamls/OptionsControl.xaml -->
<a name="uninstall-options"></a>

### <a name="uninstall-options"></a><span data-ttu-id="04764-206">Options de désinstallation</span><span class="sxs-lookup"><span data-stu-id="04764-206">Uninstall Options</span></span>

<span data-ttu-id="04764-207">(Non disponible pour tous les types de projet).</span><span class="sxs-lookup"><span data-stu-id="04764-207">(Not available for all project types.)</span></span>

<span data-ttu-id="04764-208">**Supprimer les dépendances**: lorsque sélectionnée, supprime tous les packages dépendants si elles ne sont pas référencées ailleurs dans le projet.</span><span class="sxs-lookup"><span data-stu-id="04764-208">**Remove dependencies**: when selected, removes any dependent packages if they're not referenced elsewhere in the project.</span></span>

<span data-ttu-id="04764-209">**Forcer la désinstallation même s’il existe des dépendances sur ce dernier**: lorsque sélectionnée, désinstalle un package même s’il est toujours référencé dans le projet.</span><span class="sxs-lookup"><span data-stu-id="04764-209">**Force uninstall even if there are dependencies on it**: when selected, uninstalls a package even if it's still being referenced in the project.</span></span> <span data-ttu-id="04764-210">Cela est généralement utilisé en association avec **supprimer les dépendances** pour supprimer un package et ce que les dépendances qu’il est installé.</span><span class="sxs-lookup"><span data-stu-id="04764-210">This is typically used in combination with **Remove dependencies** to remove a package and whatever dependencies it installed.</span></span> <span data-ttu-id="04764-211">À l’aide de cette option peut, toutefois, entraîner des références rompues dans le projet.</span><span class="sxs-lookup"><span data-stu-id="04764-211">Using this option may, however, lead to broken references in the project.</span></span> <span data-ttu-id="04764-212">Dans ce cas, vous devrez peut-être [réinstaller ces autres packages](../consume-packages/reinstalling-and-updating-packages.md).</span><span class="sxs-lookup"><span data-stu-id="04764-212">In such cases, you may need to [reinstall those other packages](../consume-packages/reinstalling-and-updating-packages.md).</span></span>

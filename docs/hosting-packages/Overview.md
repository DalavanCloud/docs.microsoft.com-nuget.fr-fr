---
title: Vue d’ensemble de l’hébergement de vos propres flux NuGet
description: Vue d’ensemble des possibilités d’hébergement de vos propres galeries ou flux de packages NuGet localement ou à distance.
author: karann-msft
ms.author: karann
ms.date: 08/25/2017
ms.topic: conceptual
ms.reviewer: anangaur
ms.openlocfilehash: 4741d780afa4fbe11001aed49a9f72bf608d96d9
ms.sourcegitcommit: a1846edf70ddb2505d58e536e08e952d870931b0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52303561"
---
# <a name="hosting-your-own-nuget-feeds"></a>Hébergement de vos propres flux NuGet

Plutôt que de mettre les packages à la disposition de tous, vous pouvez les réserver à un public limité, tel que votre organisation ou groupe de travail. De plus, certaines sociétés peuvent souhaiter restreindre les bibliothèques tierces que sont susceptibles d’utiliser leurs développeurs en leur demandant de puiser dans une source de packages limitée plutôt que dans nuget.org.

À ces fins, NuGet prend en charge la configuration de sources de packages privées des façons suivantes :

- Flux local : les packages sont simplement placés sur un partage de fichiers réseau approprié, dans l’idéal, en utilisant `nuget init` et `nuget add` pour créer une structure de dossiers hiérarchique (NuGet 3.3+). Pour plus d’informations, consultez [Flux locaux](../hosting-packages/local-feeds.md).
- NuGet.Server : les packages sont accessibles via un serveur HTTP local. Pour plus d’informations, consultez [NuGet.Server](../hosting-packages/nuget-server.md).
- Galerie NuGet : les packages sont hébergés sur un serveur Internet à l’aide du [projet de Galerie NuGet](https://github.com/NuGet/NuGetGallery#build-and-run-the-gallery-in-arbitrary-number-easy-steps) (github.com). Avec la Galerie NuGet, gérez les utilisateurs et profitez de fonctionnalités telles qu’une interface utilisateur web complète qui permet de rechercher et d’explorer les packages à partir du navigateur, comme nuget.org.

Il existe également plusieurs autres produits d’hébergement NuGet qui prennent en charge les flux privés distants, notamment les suivants :

- [Visual Studio Team Services Package Management](https://www.visualstudio.com/docs/package/nuget/publish), qui est également disponible sur Team Foundation Server 2017 et ultérieur.
- [MyGet](http://myget.org)
- [ProGet](http://inedo.com/proget) d’Inedo
- [NuGet Server](http://nugetserver.net/) : projet communautaire d’Inedo
- [NuGet Server (Open Source)](http://nuget-server.net) : implémentation open source similaire à NuGet Server d’Inedo
- [LiGet](https://github.com/ai-traders/liget), une implémentation open source du serveur NuGet V2 qui s’exécute sur Kestrel dans Docker
- [BaGet](https://github.com/loic-sharma/BaGet), implémentation open source du serveur NuGet V3 reposant sur ASP.NET Core
- [Sleet](https://github.com/emgarten/sleet), un générateur de flux statique NuGet V3 open source
- [Artifactory](https://www.jfrog.com/artifactory/) de JFrog
- [Nexus](http://www.sonatype.org/nexus/) de Sonatype
- [TeamCity](https://www.jetbrains.com/teamcity/) de JetBrains

Quelle que soit la façon dont les packages sont hébergés, vous y accédez en les ajoutant à la liste des sources disponibles dans `NuGet.Config`. Vous pouvez effectuer cette opération dans Visual Studio, comme décrit dans [Sources de packages](../tools/package-manager-ui.md#package-sources), ou à partir de la ligne de commande à l’aide de [`nuget sources`](../tools/cli-ref-sources.md). Le chemin d’une source peut être le chemin d’un dossier local, le nom d’un réseau ou une URL.

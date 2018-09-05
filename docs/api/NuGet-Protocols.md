---
title: Protocoles NuGet.org
description: Les protocoles nuget.org en constante évolution pour interagir avec les clients NuGet.
author: anangaur
ms.author: anangaur
ms.date: 10/30/2017
ms.topic: conceptual
ms.reviewer: kraigb
ms.openlocfilehash: d0add777040dbb8bcde6d8e385a4feab568e5cdd
ms.sourcegitcommit: 1d1406764c6af5fb7801d462e0c4afc9092fa569
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43547271"
---
# <a name="nugetorg-protocols"></a><span data-ttu-id="7bdcb-103">protocoles NuGet.org</span><span class="sxs-lookup"><span data-stu-id="7bdcb-103">nuget.org protocols</span></span>

<span data-ttu-id="7bdcb-104">Pour interagir avec nuget.org, les clients doivent suivre certains protocoles.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-104">To interact with nuget.org, clients need to follow certain protocols.</span></span> <span data-ttu-id="7bdcb-105">Étant donné que ces protocoles conservent en constante évolution, les clients doivent identifier la version du protocole qu’ils utilisent lors de l’appel d’API de nuget.org spécifique.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-105">Because these protocols keep evolving, clients must identify the protocol version they use when calling specific nuget.org APIs.</span></span> <span data-ttu-id="7bdcb-106">Cela permet de nuget.org d’introduire des modifications d’une manière sans rupture pour les anciens clients.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-106">This allows nuget.org to introduce changes in a non-breaking way for the old clients.</span></span>

> [!Note]
> <span data-ttu-id="7bdcb-107">Les API documentées dans cette page sont spécifiques à nuget.org et n’est pas garantie pour d’autres implémentations de serveur NuGet d’introduire ces API.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-107">The APIs documented on this page are specific to nuget.org and there is no expectation for other NuGet server implementations to introduce these APIs.</span></span> 

<span data-ttu-id="7bdcb-108">Pour plus d’informations sur l’API NuGet implémenté largement au sein de l’écosystème NuGet, consultez le [vue d’ensemble de l’API](overview.md).</span><span class="sxs-lookup"><span data-stu-id="7bdcb-108">For information about the NuGet API implemented broadly across the NuGet ecosystem, see the [API overview](overview.md).</span></span>

<span data-ttu-id="7bdcb-109">Cette rubrique répertorie les différents protocoles en tant qu’et quand ils proviennent de l’existence.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-109">This topic lists various protocols as and when they come to existence.</span></span>

## <a name="nuget-protocol-version-410"></a><span data-ttu-id="7bdcb-110">Version du protocole NuGet 4.1.0</span><span class="sxs-lookup"><span data-stu-id="7bdcb-110">NuGet protocol version 4.1.0</span></span>

<span data-ttu-id="7bdcb-111">Le 4.1.0 protocole spécifie l’utilisation de clés de portée vérifier pour interagir avec les services autres que nuget.org, pour valider un package par rapport à un compte nuget.org.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-111">The 4.1.0 protocol specifies usage of verify-scope keys to interact with services other than nuget.org, to validate a package against a nuget.org account.</span></span> <span data-ttu-id="7bdcb-112">Notez que le `4.1.0` version nombre est une chaîne opaque mais qu’il coïncide avec la première version du client NuGet officiel pris en charge ce protocole.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-112">Note that the `4.1.0` version number is an opaque string but happens to coincide with the first version of the official NuGet client that supported this protocol.</span></span>

<span data-ttu-id="7bdcb-113">La validation garantit que les clés API créés par l’utilisateur sont utilisées uniquement auprès de nuget.org, et cette vérification ou la validation à partir d’un service tiers est gérée via un usage des clés de vérifier de portée.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-113">Validation ensures that the user-created API keys are used only with nuget.org, and that other verification or validation from a third-party service is handled through a one-time use verify-scope keys.</span></span> <span data-ttu-id="7bdcb-114">Ces clés de vérifier de portée peuvent être utilisées pour valider que le package appartient à un utilisateur particulier (compte) sur nuget.org.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-114">These verify-scope keys can be used to validate that the package belongs to a particular user (account) on nuget.org.</span></span>

### <a name="client-requirement"></a><span data-ttu-id="7bdcb-115">Exigence du client</span><span class="sxs-lookup"><span data-stu-id="7bdcb-115">Client requirement</span></span>

<span data-ttu-id="7bdcb-116">Les clients sont requises pour transmettre l’en-tête suivant lorsqu’ils effectuent des appels d’API vers **push** packages sur nuget.org :</span><span class="sxs-lookup"><span data-stu-id="7bdcb-116">Clients are required to pass the following header when they make API calls to **push** packages to nuget.org:</span></span>

    X-NuGet-Protocol-Version: 4.1.0

<span data-ttu-id="7bdcb-117">Notez que le `X-NuGet-Client-Version` en-tête a une sémantique similaire, mais elle est réservé uniquement à être utilisé par le client NuGet officiel.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-117">Note that the `X-NuGet-Client-Version` header has similar semantics but is reserved to only be used by the official NuGet client.</span></span> <span data-ttu-id="7bdcb-118">Les clients tiers doivent utiliser le `X-NuGet-Protocol-Version` en-tête et valeur.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-118">Third party clients should use the `X-NuGet-Protocol-Version` header and value.</span></span>

<span data-ttu-id="7bdcb-119">Le **push** protocole lui-même est décrit dans la documentation pour le [ `PackagePublish` ressources](package-publish-resource.md).</span><span class="sxs-lookup"><span data-stu-id="7bdcb-119">The **push** protocol itself is described in the documentation for the [`PackagePublish` resource](package-publish-resource.md).</span></span>

<span data-ttu-id="7bdcb-120">Si un client interagit avec les services externes et des besoins pour vérifier si un package appartient à un utilisateur particulier (compte), il doit utiliser le protocole suivant et utiliser les clés de l’étendue de vérifier et pas les clés de l’API à partir de nuget.org.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-120">If a client interacts with external services and needs to validate whether a package belongs to a particular user (account), it should use the following protocol and use the verify-scope keys and not the API keys from nuget.org.</span></span>

### <a name="api-to-request-a-verify-scope-key"></a><span data-ttu-id="7bdcb-121">API pour demander une clé de l’étendue de la vérification</span><span class="sxs-lookup"><span data-stu-id="7bdcb-121">API to request a verify-scope key</span></span>

<span data-ttu-id="7bdcb-122">Cette API est utilisée pour obtenir une clé de l’étendue de la vérification d’un auteur de nuget.org valider un package détenu par ce dernier.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-122">This API is used to get a verify-scope key for a nuget.org author to validate a package owned by him/her.</span></span>

    POST api/v2/package/create-verification-key/{ID}/{VERSION}

#### <a name="request-parameters"></a><span data-ttu-id="7bdcb-123">Paramètres de la demande</span><span class="sxs-lookup"><span data-stu-id="7bdcb-123">Request parameters</span></span>

<span data-ttu-id="7bdcb-124">Name</span><span class="sxs-lookup"><span data-stu-id="7bdcb-124">Name</span></span>           | <span data-ttu-id="7bdcb-125">Vers l'avant</span><span class="sxs-lookup"><span data-stu-id="7bdcb-125">In</span></span>     | <span data-ttu-id="7bdcb-126">Type</span><span class="sxs-lookup"><span data-stu-id="7bdcb-126">Type</span></span>   | <span data-ttu-id="7bdcb-127">Obligatoire</span><span class="sxs-lookup"><span data-stu-id="7bdcb-127">Required</span></span> | <span data-ttu-id="7bdcb-128">Notes</span><span class="sxs-lookup"><span data-stu-id="7bdcb-128">Notes</span></span>
-------------- | ------ | ------ | -------- | -----
<span data-ttu-id="7bdcb-129">Id</span><span class="sxs-lookup"><span data-stu-id="7bdcb-129">ID</span></span>             | <span data-ttu-id="7bdcb-130">URL</span><span class="sxs-lookup"><span data-stu-id="7bdcb-130">URL</span></span>    | <span data-ttu-id="7bdcb-131">chaîne</span><span class="sxs-lookup"><span data-stu-id="7bdcb-131">string</span></span> | <span data-ttu-id="7bdcb-132">oui</span><span class="sxs-lookup"><span data-stu-id="7bdcb-132">yes</span></span>      | <span data-ttu-id="7bdcb-133">L’identidier de package pour lequel la touche d’étendue de vérification est demandée</span><span class="sxs-lookup"><span data-stu-id="7bdcb-133">The package identidier for which the verify scope key is requested</span></span>
<span data-ttu-id="7bdcb-134">VERSION</span><span class="sxs-lookup"><span data-stu-id="7bdcb-134">VERSION</span></span>        | <span data-ttu-id="7bdcb-135">URL</span><span class="sxs-lookup"><span data-stu-id="7bdcb-135">URL</span></span>    | <span data-ttu-id="7bdcb-136">chaîne</span><span class="sxs-lookup"><span data-stu-id="7bdcb-136">string</span></span> | <span data-ttu-id="7bdcb-137">Non</span><span class="sxs-lookup"><span data-stu-id="7bdcb-137">no</span></span>       | <span data-ttu-id="7bdcb-138">La version du package</span><span class="sxs-lookup"><span data-stu-id="7bdcb-138">The package version</span></span>
<span data-ttu-id="7bdcb-139">X-NuGet-ApiKey</span><span class="sxs-lookup"><span data-stu-id="7bdcb-139">X-NuGet-ApiKey</span></span> | <span data-ttu-id="7bdcb-140">Header</span><span class="sxs-lookup"><span data-stu-id="7bdcb-140">Header</span></span> | <span data-ttu-id="7bdcb-141">chaîne</span><span class="sxs-lookup"><span data-stu-id="7bdcb-141">string</span></span> | <span data-ttu-id="7bdcb-142">oui</span><span class="sxs-lookup"><span data-stu-id="7bdcb-142">yes</span></span>      | <span data-ttu-id="7bdcb-143">Par exemple, `X-NuGet-ApiKey: {USER_API_KEY}`.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-143">For example, `X-NuGet-ApiKey: {USER_API_KEY}`</span></span>

#### <a name="response"></a><span data-ttu-id="7bdcb-144">Réponse</span><span class="sxs-lookup"><span data-stu-id="7bdcb-144">Response</span></span>

```json
{
    "Key": "{Verify scope key from nuget.org}",
    "Expires": "{Date}"
}
```

### <a name="api-to-verify-the-verify-scope-key"></a><span data-ttu-id="7bdcb-145">API pour vérifier la touche d’étendue de vérification</span><span class="sxs-lookup"><span data-stu-id="7bdcb-145">API to verify the verify scope key</span></span>

<span data-ttu-id="7bdcb-146">Cette API est utilisée pour valider une clé de vérifier de portée pour le package détenu par l’auteur de nuget.org.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-146">This API is used to validate a verify-scope key for package owned by the nuget.org author.</span></span>

    GET api/v2/verifykey/{ID}/{VERSION}

#### <a name="request-parameters"></a><span data-ttu-id="7bdcb-147">Paramètres de la demande</span><span class="sxs-lookup"><span data-stu-id="7bdcb-147">Request parameters</span></span>

<span data-ttu-id="7bdcb-148">Name</span><span class="sxs-lookup"><span data-stu-id="7bdcb-148">Name</span></span>           | <span data-ttu-id="7bdcb-149">Vers l'avant</span><span class="sxs-lookup"><span data-stu-id="7bdcb-149">In</span></span>     | <span data-ttu-id="7bdcb-150">Type</span><span class="sxs-lookup"><span data-stu-id="7bdcb-150">Type</span></span>   | <span data-ttu-id="7bdcb-151">Obligatoire</span><span class="sxs-lookup"><span data-stu-id="7bdcb-151">Required</span></span> | <span data-ttu-id="7bdcb-152">Notes</span><span class="sxs-lookup"><span data-stu-id="7bdcb-152">Notes</span></span>
-------------  | ------ | ------ | -------- | -----
<span data-ttu-id="7bdcb-153">Id</span><span class="sxs-lookup"><span data-stu-id="7bdcb-153">ID</span></span>             | <span data-ttu-id="7bdcb-154">URL</span><span class="sxs-lookup"><span data-stu-id="7bdcb-154">URL</span></span>    | <span data-ttu-id="7bdcb-155">chaîne</span><span class="sxs-lookup"><span data-stu-id="7bdcb-155">string</span></span> | <span data-ttu-id="7bdcb-156">oui</span><span class="sxs-lookup"><span data-stu-id="7bdcb-156">yes</span></span>      | <span data-ttu-id="7bdcb-157">L’identificateur de package pour lequel la touche d’étendue de vérification est demandée</span><span class="sxs-lookup"><span data-stu-id="7bdcb-157">The package identifier for which the verify scope key is requested</span></span>
<span data-ttu-id="7bdcb-158">VERSION</span><span class="sxs-lookup"><span data-stu-id="7bdcb-158">VERSION</span></span>        | <span data-ttu-id="7bdcb-159">URL</span><span class="sxs-lookup"><span data-stu-id="7bdcb-159">URL</span></span>    | <span data-ttu-id="7bdcb-160">chaîne</span><span class="sxs-lookup"><span data-stu-id="7bdcb-160">string</span></span> | <span data-ttu-id="7bdcb-161">Non</span><span class="sxs-lookup"><span data-stu-id="7bdcb-161">no</span></span>       | <span data-ttu-id="7bdcb-162">La version du package</span><span class="sxs-lookup"><span data-stu-id="7bdcb-162">The package version</span></span>
<span data-ttu-id="7bdcb-163">X-NuGet-ApiKey</span><span class="sxs-lookup"><span data-stu-id="7bdcb-163">X-NuGet-ApiKey</span></span> | <span data-ttu-id="7bdcb-164">Header</span><span class="sxs-lookup"><span data-stu-id="7bdcb-164">Header</span></span> | <span data-ttu-id="7bdcb-165">chaîne</span><span class="sxs-lookup"><span data-stu-id="7bdcb-165">string</span></span> | <span data-ttu-id="7bdcb-166">oui</span><span class="sxs-lookup"><span data-stu-id="7bdcb-166">yes</span></span>      | <span data-ttu-id="7bdcb-167">Par exemple, `X-NuGet-ApiKey: {VERIFY_SCOPE_KEY}`.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-167">For example, `X-NuGet-ApiKey: {VERIFY_SCOPE_KEY}`</span></span>

> [!Note]
> <span data-ttu-id="7bdcb-168">Cette touche étendue API de vérification expire dans une journée ou à la première utilisation, selon ce qui se produit en premier.</span><span class="sxs-lookup"><span data-stu-id="7bdcb-168">This verify scope API key expires in a day's time or on first use, whichever occurs first.</span></span>

#### <a name="response"></a><span data-ttu-id="7bdcb-169">Réponse</span><span class="sxs-lookup"><span data-stu-id="7bdcb-169">Response</span></span>

<span data-ttu-id="7bdcb-170">Code d’état</span><span class="sxs-lookup"><span data-stu-id="7bdcb-170">Status Code</span></span> | <span data-ttu-id="7bdcb-171">Signification</span><span class="sxs-lookup"><span data-stu-id="7bdcb-171">Meaning</span></span>
----------- | -------
<span data-ttu-id="7bdcb-172">200</span><span class="sxs-lookup"><span data-stu-id="7bdcb-172">200</span></span>         | <span data-ttu-id="7bdcb-173">La clé API est valide</span><span class="sxs-lookup"><span data-stu-id="7bdcb-173">The API key is valid</span></span>
<span data-ttu-id="7bdcb-174">403</span><span class="sxs-lookup"><span data-stu-id="7bdcb-174">403</span></span>         | <span data-ttu-id="7bdcb-175">La clé API est non valide ou non autorisé à envoyer sur le package</span><span class="sxs-lookup"><span data-stu-id="7bdcb-175">The API key is invalid or not authorized to push against the package</span></span>
<span data-ttu-id="7bdcb-176">404</span><span class="sxs-lookup"><span data-stu-id="7bdcb-176">404</span></span>         | <span data-ttu-id="7bdcb-177">Le package référencé par `ID` et `VERSION` (facultatif) n’existe pas</span><span class="sxs-lookup"><span data-stu-id="7bdcb-177">The package referred to by `ID` and `VERSION` (optional) does not exist</span></span>

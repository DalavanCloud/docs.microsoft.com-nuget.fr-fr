---
title: Installer un package NuGet signé
description: Décrit le processus d’installation de packages NuGet signés et de configuration des paramètres d’approbation des signatures de package.
author: karann-msft
ms.author: karann
ms.date: 11/29/2018
ms.topic: conceptual
ms.openlocfilehash: 11ffaee96b6f6a9260f38c534328b6631cd96abf
ms.sourcegitcommit: 673e580ae749544a4a071b4efe7d42fd2bb6d209
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52977833"
---
# <a name="install-a-signed-package"></a>Installer un package signé

L’installation de packages signés ne nécessite aucune action spécifique. Cependant, si le contenu a été modifié après sa signature, l’installation est bloquée avec une erreur [NU3008](../reference/errors-and-warnings/NU3008.md).

> [!Warning]
> Les packages signés avec des certificats non approuvés sont considérés comme non signés et installés sans avertissements ni erreurs, comme n’importe quel package non signé.

## <a name="configure-package-signature-requirements"></a>Configurer les exigences de signature de package

> [!Note]
> Nécessite NuGet 4.9.0+ et Visual Studio version 15.9 et ultérieure sur Windows

Vous pouvez configurer la façon dont les clients NuGet valident les signatures de package en définissant `signatureValidationMode` sur `require` dans le fichier [nuget.config](../reference/nuget-config-file) avec la commande [`nuget config`](../tools/cli-ref-config).

```cmd
nuget.exe config -set signatureValidationMode=require
```

```xml
  <config>
    <add key="signatureValidationMode" value="require" />
  </config>
```

Ce mode vérifie que tous les packages sont signés par un des certificats approuvés dans le fichier `nuget.config`. Ce fichier vous permet de spécifier les auteurs et/ou les dépôts qui sont approuvés en fonction de l’empreinte du certificat.

### <a name="trust-package-author"></a>Approuver un auteur de package

Pour approuver des packages en fonction de la signature de l’auteur, utilisez la commande [`trusted-signers`](..tools/cli-ref-trusted-signers) pour définir la propriété `author` dans le fichier nuget.config.

```cmd
nuget.exe  trusted-signers Add -Name MyCompanyCert -CertificateFingerprint CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039 -FingerprintAlgorithm SHA256
```

```xml
<trustedSigners>
  <author name="MyCompanyCert">
    <certificate fingerprint="CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039" hashAlgorithm="SHA256" allowUntrustedRoot="false" />
  </author>
</trustedSigners>
```

>[!TIP]
>Utilisez la [commande verify](https://docs.microsoft.com/en-us/nuget/tools/cli-ref-verify) de `nuget.exe` pour obtenir la valeur `SHA256` de l’empreinte du certificat.


### <a name="trust-all-packages-from-a-repository"></a>Approuver tous les packages d’un dépôt

Pour approuver des packages en fonction de la signature du dépôt, utilisez l’élément `repository` :

```xml
<trustedSigners>  
  <repository name="nuget.org" serviceIndex="https://api.nuget.org/v3/index.json">
    <certificate fingerprint="0E5F38F57DC1BCC806D8494F4F90FBCEDD988B4676070...." 
                  hashAlgorithm="SHA256" 
                allowUntrustedRoot="false" />
  </repository>
</trustedSigners>
```

### <a name="trust-package-owners"></a>Approuver des propriétaires de package

Les signatures de dépôt incluent des métadonnées supplémentaires pour déterminer les propriétaires du package au moment de l’envoi. Vous pouvez restreindre les packages d’un dépôt en fonction d’une liste de propriétaires :

```xml
<trustedSigners>  
  <repository name="nuget.org" serviceIndex="https://api.nuget.org/v3/index.json">
    <certificate fingerprint="0E5F38F57DC1BCC806D8494F4F90FBCEDD988B4676070...." 
                  hashAlgorithm="SHA256" 
                allowUntrustedRoot="false" />
      <owners>microsoft;nuget</owners>
  </repository>
</trustedSigners>
```

Si un package a plusieurs propriétaires et que l’un de ces propriétaires est dans la liste approuvée, l’installation du package réussit.

### <a name="untrusted-root-certificates"></a>Certificats racines non approuvés

Dans certaines situations, vous souhaitez activer la vérification avec des certificats qui ne sont pas chaînés à une racine approuvée dans la machine locale. Vous pouvez utiliser l’attribut `allowUntrustedRoot` pour personnaliser ce comportement.

### <a name="sync-repository-certificates"></a>Synchroniser des certificats de dépôt

Les dépôts de packages doivent annoncer les certificats qu’ils utilisent dans leur [index des services](https://docs.microsoft.com/en-us/nuget/api/service-index). Au final, le dépôt met à jour ces certificats, par exemple quand un certificat expire. Quand cela se produit, les clients avec des stratégies spécifiques demandent une mise à jour de la configuration pour inclure le certificat nouvellement ajouté. Vous pouvez facilement mettre à niveau les signataires approuvés associés à un dépôt avec la [commande trusted-signers sync](/nuget/tools/cli-ref-trusted-signers.md#nuget-trusted-signers-sync--name-) de `nuget.exe`.

### <a name="schema-reference"></a>Informations de référence sur le schéma

Vous trouverez les informations de référence complètes sur le schéma pour les stratégies clientes dans les [informations de référence sur nuget.config](/nuget/reference/nuget-config-file#trustedsigners-section)

## <a name="related-articles"></a>Articles connexes

- [Les différentes façons d’installer un package NuGet](ways-to-install-a-package.md)
- [Signature de packages NuGet](../create-packages/Sign-a-Package.md)
- [Informations de référence sur les packages signés](../reference/Signed-Packages-Reference.md)

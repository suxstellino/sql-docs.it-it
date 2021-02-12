---
title: Asserzione di autorizzazioni negli assembly personalizzati | Microsoft Docs
description: Informazioni su come usare l'asserzione di autorizzazioni in modo che sia possibile implementare un assembly personalizzato che effettua chiamate protette a risorse protette all'interno del sistema di sicurezza.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-assemblies
ms.topic: reference
helpviewer_keywords:
- secure calls [Reporting Services]
- custom assemblies [Reporting Services], permissions
- permission sets [Reporting Services]
- asserting permissions
- permissions [Reporting Services], custom assemblies
- limited permission sets
- security configuration files [Reporting Services]
ms.assetid: 3afb9631-f15e-405e-990b-ee102828f298
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e63239176b15eb1b1044c6b281cdc8014c26d973
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064616"
---
# <a name="asserting-permissions-in-custom-assemblies"></a>Asserzione di autorizzazioni negli assembly personalizzati
  Per impostazione predefinita, il codice degli assembly personalizzati viene eseguito con il set di autorizzazioni **Execution** limitato. In alcuni casi, potrebbe essere necessario implementare un assembly personalizzato per l'esecuzione di chiamate protette alle risorse protette all'interno del sistema di sicurezza (ad esempio un file o il Registro di sistema). A tale scopo, è necessario effettuare le operazioni seguenti:  
  
1.  Identificare le autorizzazioni esatte necessarie per il codice per effettuare la chiamata protetta. Se questo metodo è parte di una libreria [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], queste informazioni devono essere incluse nella documentazione del metodo.  
  
2.  Modificare i file di configurazione dei criteri del server di report per concedere le autorizzazioni necessarie all'assembly personalizzato. Per altre informazioni sui file di configurazione dei criteri di sicurezza, vedere [Uso di file di criteri di sicurezza di Reporting Services](../../reporting-services/extensions/secure-development/using-reporting-services-security-policy-files.md).  
  
3.  Eseguire l'asserzione delle autorizzazioni necessarie come parte del metodo nel quale viene effettuata la chiamata protetta. Questa operazione è necessaria in quanto il codice dell'assembly personalizzato chiamato dal server di report fa parte dell'assembly host delle espressioni di report che, per impostazione predefinita, viene eseguito con l'autorizzazione di **esecuzione**. Il set di autorizzazioni **Execution** consente al codice di essere eseguito, ma non di usare risorse protette.  
  
4.  Contrassegnare l'assembly personalizzato con **AllowPartiallyTrustedCallersAttribute** se è firmato con un nome sicuro. Questa operazione è necessaria in quanto gli assembly personalizzati vengono chiamati da un'espressione di report che fa parte dell'assembly host delle espressioni di report a cui, per impostazione predefinita, non viene concesso il set di autorizzazioni **FullTrust**. Si tratta quindi di un chiamante "parzialmente attendibile". Per altre informazioni, vedere [Uso di assembly personalizzati con nome sicuro](../../reporting-services/custom-assemblies/using-strong-named-custom-assemblies.md).  
  
## <a name="implementing-a-secure-call"></a>Implementazione di una chiamata protetta  
 È possibile modificare i file di configurazione dei criteri per concedere autorizzazioni specifiche all'assembly. Se, ad esempio, si scrive un assembly personalizzato per gestire la conversione delle valute, potrebbe essere necessario leggere i tassi di cambio della valuta correnti da un file. Per recuperare le informazioni sui tassi, aggiungere un'altra autorizzazione di sicurezza **FileIOPermission** al set di autorizzazioni per l'assembly. Nel file di configurazione dei criteri è possibile inserire le seguenti voci aggiuntive:  
  
```  
<PermissionSet class="NamedPermissionSet"  
   version="1"  
   Name="CurrencyRatesFilePermissionSet"  
   Description="A special permission set that grants read access to my currency rates file.">  
      <IPermission class="FileIOPermission"  
         version="1"  
         Read="C:\CurrencyRates.xml"/>  
      <IPermission class="SecurityPermission"  
         version="1"  
         Flags="Execution, Assertion"/>  
</PermissionSet>  
```  
  
 Viene quindi aggiunto un gruppo di codice che fa riferimento a tale set di autorizzazioni:  
  
```  
<CodeGroup class="UnionCodeGroup"  
   version="1"  
   PermissionSetName="CurrencyRatesFilePermissionSet"  
   Name="MyNewCodeGroup"  
   Description="A special code group for my custom assembly.">  
   <IMembershipCondition class="UrlMembershipCondition"  
      version="1"  
      Url="C:\Program Files\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\MSSQL\Reporting Services\ReportServer\bin\CurrencyConversion.dll"/>  
</CodeGroup>  
```  
  
 Per consentire al codice di acquisire l'autorizzazione appropriata, è necessaria l'asserzione dell'autorizzazione nel codice dell'assembly personalizzato. Se, ad esempio, si desidera aggiungere l'accesso in sola lettura a un file XML, C:\CurrencyRates.xml, è necessario aggiungere al metodo il codice seguente:  
  
```  
// C#  
FileIOPermission permission = new FileIOPermission(FileIOPermissionAccess.Read, @"C:\CurrencyRates.xml");  
try  
{  
   permission.Assert();  
   // Load the XML currency rates file  
   XmlDocument doc = new XmlDocument();  
   doc.Load(@"C:\CurrencyRates.xml");  
...  
```  
  
 È inoltre possibile aggiungere l'asserzione come attributo del metodo:  
  
```  
[FileIOPermissionAttribute(SecurityAction.Assert, Read=@"C:\CurrencyRates.xml")]  
```  
  
 Per ulteriori informazioni, vedere l'argomento relativo alla sicurezza di .NET Framework nella Guida per gli sviluppatori di .NET Framework.  
  
## <a name="see-also"></a>Vedere anche  
 [Uso di assembly personalizzati con i report](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)  
  
  

---
description: Operazioni del servizio Web per categoria (Master Data Services)
title: Operazioni del servizio Web per categoria
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: e3f346b5-7e26-481d-9821-1846e2e91289
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 65656aa902b5500d1dcc57dde914b3a0b2265008
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350135"
---
# <a name="categorized-web-service-operations-master-data-services"></a>Operazioni del servizio Web per categoria (Master Data Services)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Il servizio Web [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] contiene un set completo di operazioni che consentono di scrivere il codice per controllare tutte le caratteristiche implementate da [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] tramite l'interfaccia utente. Le operazioni del servizio Web sono definite dall'interfaccia <xref:Microsoft.MasterDataServices.IService> e vengono implementate come metodi nella classe <xref:Microsoft.MasterDataServices.ServiceClient>. In questo argomento le operazioni del servizio Web vengono raggruppate in categorie concettuali per consentire all'utente di capire come utilizzare l'API del servizio Web.  
  
## <a name="model-operations"></a>Operazioni di modelli  
 Queste operazioni vengono utilizzate per creare, aggiornare ed eliminare i modelli, nonché per gestire tutto il contenuto di un modello, ad esempio entità, gerarchie e versioni. Per altre informazioni, vedere [Modelli &#40;Master Data Services&#41;](../../master-data-services/models-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.MetadataUpdate%2A>
  
## <a name="entity-operations"></a>Operazioni di entità  
 Queste operazioni vengono utilizzate per creare, aggiornare ed eliminare i membri di una singola entità. Per altre informazioni, vedere [Entità &#40;Master Data Services&#41;](../../master-data-services/entities-master-data-services.md) e [Membri &#40;Master Data Services&#41;](../../master-data-services/members-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberKeyLookup%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersCopy%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersMerge%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMembersUpdate%2A>
  
## <a name="member-operations"></a>Operazioni di membri  
 Queste operazioni vengono utilizzate per ottenere, aggiornare ed eliminare i membri. Il set di membri utilizzato può contenere membri di più entità. Per altre informazioni, vedere [Membri &#40;Master Data Services&#41;](../../master-data-services/members-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkMerge%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersBulkUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ModelMembersGet%2A>
  
## <a name="attribute-and-hierarchy-operations"></a>Operazioni di attributi e gerarchie  
 Queste operazioni vengono utilizzate per ottenere informazioni su attributi e gerarchie. Gli attributi e le gerarchie possono essere modificati anche mediante le operazioni di modelli, ad esempio <xref:Microsoft.MasterDataServices.ServiceClient.MetadataUpdate%2A>. Per altre informazioni, vedere [Attributi &#40;Master Data Services&#41;](../../master-data-services/attributes-master-data-services.md) e [Gerarchie &#40;Master Data Services&#41;](../../master-data-services/hierarchies-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAttributesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.HierarchyMembersGet%2A>
  
## <a name="business-rule-operations"></a>Operazioni di regole business  
 Queste operazioni vengono utilizzate per creare, aggiornare, eliminare e pubblicare le regole business. Per altre informazioni, vedere [Regole di business &#40;Master Data Services&#41;](../../master-data-services/business-rules-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesPaletteGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesPublish%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.BusinessRulesUpdate%2A>
  
## <a name="annotation-operations"></a>Operazioni di annotazioni  
 Queste operazioni vengono utilizzate per creare, aggiornare ed eliminare le annotazioni. Per altre informazioni, vedere [Annotazioni &#40;Master Data Services&#41;](../../master-data-services/annotations-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.AnnotationsDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.AnnotationsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAnnotationsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityMemberAnnotationsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionAnnotationsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionAnnotationsGet%2A>
  
## <a name="transaction-operations"></a>Operazioni di transazioni  
 Queste operazioni vengono utilizzate per ottenere e invertire le transazioni. Per altre informazioni, vedere [Transazioni &#40;Master Data Services&#41;](../../master-data-services/transactions-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.TransactionsReverse%2A>
  
## <a name="version-and-validation-operations"></a>Operazioni di versioni e convalida  
 Queste operazioni vengono utilizzate per copiare e convalidare le versioni. Per altre informazioni, vedere [Versioni &#40;Master Data Services&#41;](../../master-data-services/versions-master-data-services.md) e [Convalida &#40;Master Data Services&#41;](../../master-data-services/validation-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.VersionCopy%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ValidationGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ValidationProcess%2A>
  
## <a name="data-quality-operations"></a>Operazioni di qualità dei dati  
 Queste operazioni vengono utilizzate per eseguire le attività relative alla qualità dei dati e per esaminarne i risultati.  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityCleansingOperationCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityMatchingOperationCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityInstalledState%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityKnowledgeBasesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationStart%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationResultsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.DataQualityOperationStatus%2A>
  
## <a name="data-import-operations"></a>Operazioni di importazione dei dati  
 Queste operazioni vengono utilizzate per importare i dati in un database di [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]. Per altre informazioni, vedere [Panoramica: importazione di dati da tabelle &#40;Master Data Services&#41;](../../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingClear%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingLoad%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.EntityStagingProcess%2A>
  
 Le operazioni seguenti vengono utilizzate per importare i dati mediante il processo di gestione temporanea incluso in [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Queste operazioni devono essere utilizzate solo per supportare i database esistenti. Per un nuovo progetto di sviluppo utilizzare le operazioni elencate in precedenza.  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingClear%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingNameCheck%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.StagingProcess%2A>
  
## <a name="data-export-operations"></a>Operazioni di esportazione dei dati  
 Queste operazioni vengono utilizzate per esportare i dati tramite l'utilizzo di viste sottoscrizioni. Per altre informazioni, vedere [Panoramica: Esportazione di dati &#40;Master Data Services&#41;](../../master-data-services/overview-exporting-data-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewListGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ExportViewUpdate%2A>
  
## <a name="security-operations"></a>Operazioni per la sicurezza  
 Queste operazioni vengono utilizzate per modificare le impostazioni di sicurezza che controllano l'accesso al database di [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]. Per altre informazioni, vedere [Sicurezza &#40;Master Data Services&#41;](../../master-data-services/security-master-data-services.md).  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrincipalsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesClone%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesCreate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SecurityPrivilegesUpdate%2A>
  
## <a name="system-operations"></a>Operazioni di sistema  
 Queste operazioni vengono utilizzate per ottenere e aggiornare le impostazioni di sistema e le preferenze dell'utente.  
  
- <xref:Microsoft.MasterDataServices.ServiceClient.ServiceCheck%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.ServiceVersionGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemDomainListGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemPropertiesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemSettingsGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.SystemSettingsUpdate%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesDelete%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesGet%2A>
- <xref:Microsoft.MasterDataServices.ServiceClient.UserPreferencesUpdate%2A>
  
  

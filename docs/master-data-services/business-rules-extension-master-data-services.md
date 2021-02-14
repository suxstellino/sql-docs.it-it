---
title: Business Rules Extension
description: È possibile applicare gli script SQL definiti dall'utente come estensione di condizioni e azioni predefinite della regola business in Master Data Services.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 4c18be5f-a3fa-45a8-9be6-0f45f58bbc9e
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 0f99b4496f8abaae384c67d2c7849687f7045744
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341499"
---
# <a name="business-rules-extension-master-data-services"></a>Estensione delle regole business (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]è possibile applicare gli script SQL definiti dall'utente come estensione di condizioni e azioni predefinite.  
  
> [!NOTE]  
>  Tutti gli script devono essere definiti nello schema [usr].  
  
 Le funzioni SQL che soddisfano i criteri seguenti possono essere usate come una condizione della regola business.  
  
-   Il valore restituito deve essere di tipo BIT.  
  
-   Per i tipi di parametro sono supportati solo i tipi seguenti.  
  
    -   NVARCHAR  
  
    -   DATETIME2  
  
    -   DECIMAL (precisione, scala)  
  
         la precisione deve essere 38  
  
         la scala deve essere un valore compreso tra 0 e 7  
  
 le stored procedure SQL che usano la sintassi seguente possono essere usate come azione delle regole business  
  
```  
CREATE PROCEDURE [usr].[YourAction]  
       (         
            @MemberIdList mdm.[MemberId] READONLY,  
            @ModelName NVARCHAR(MAX),  
            @VersionName NVARCHAR(MAX),  
            @EntityName NVARCHAR(MAX),  
            @BusinessRuleName NVARCHAR(MAX)  
        )    
      AS BEGIN    
       ...     
       END  
  
```  
  
 Gli script definiti dall'utente non verranno aggiunti ai pacchetti di distribuzione. Assicurarsi che il database Master Data Services di destinazione contenga tutti gli script usati nelle regole business prima di distribuire un pacchetto.  
  
 Le azioni di script verranno eseguite come mds_br_user con queste autorizzazioni  
  
|SCHEMA|Autorizzazioni|  
|-|-|  
|mdm|SELECT|  
|stg|SELECT, UPDATE, DELETE, EXECUTE, INSERT|  
|usr|FULL|  
  
## <a name="prerequisites"></a>Prerequisiti  
 Per eseguire questa procedura:  
  
-   È necessaria l'autorizzazione per accedere all'area funzionale Amministrazione sistema.  
  
-   È necessario essere un amministratore del modello. Per altre informazioni, vedere [Amministratori &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)  
  
-   Gli script definiti dall'utente sono stati aggiunti al database di [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
## <a name="create-a-business-rule-to-take-a-user-defined-script-as-a-condition-or-as-an-action"></a>Creare una regola business per eseguire uno script definito dall'utente come condizione o azione  
  
1.  In Gestione dati master fare clic su **Amministrazione sistema**.  
  
2.  Sulla barra dei menu scegliere **Gestisci** e fare clic su **Regole business**.  
  
3.  Nella pagina **Regole business** selezionare un modello nell'elenco a discesa **Modello** .  
  
4.  Nell'elenco a discesa **Entità** selezionare un'entità.  
  
5.  Nell'elenco a discesa **Tipi di membri** selezionare un tipo di membro a cui applicare la regola di business.  
  
6.  Fare clic su **Aggiungi**.  
  
7.  Eseguire le operazioni seguenti per creare uno script definito dall'utente come condizione.  
  
    1.  Nel blocco **If** fare clic sul pulsante **Aggiungi** . Viene visualizzato un pannello.  
  
    2.  Nell'elenco a discesa **Operatore** selezionare la funzione definita dall'utente in **Script definito dall'utente** .  
  
    3.  Vengono visualizzati tutti i parametri della funzione definita dall'utente.  
  
    4.  Assegnare un valore a ogni parametro  
  
    5.  Fare clic su **Salva**.  
  
8.  Eseguire le operazioni seguenti per usare uno script definito dall'utente come azione.  
  
    1.  Nel blocco **Then** fare clic sul pulsante **Aggiungi** . Viene visualizzato un pannello.  
  
    2.  Nell'elenco a discesa **Operatore** selezionare la funzione definita dall'utente in **Script definito dall'utente** .  
  
    3.  Fare clic su **Salva**.  
  
## <a name="see-also"></a>Vedere anche  
 [Regole business &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)   
 [Condizioni della regola business &#40;Master Data Services&#41;](../master-data-services/business-rule-conditions-master-data-services.md)   
 [Azioni Regola business &#40;Master Data Services&#41;](../master-data-services/business-rule-actions-master-data-services.md)  
  
  

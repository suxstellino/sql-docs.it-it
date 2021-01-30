---
description: Creare viste
title: Creare viste | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- views [SQL Server], creating
ms.assetid: 0b7bd2a1-544c-42ba-8e7b-4822f34d7b64
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d7c7e339afa580f81a834021670cc81fc61ab8ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161903"
---
# <a name="create-views"></a>Creare viste
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  È possibile creare viste in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Una vista può essere utilizzata per gli scopi seguenti:  
  
-   Per analizzare, semplificare e personalizzare la visualizzazione del database per ogni utente.  
  
-   Come meccanismo di sicurezza grazie al quale è possibile consentire agli utenti di accedere ai dati tramite una vista, senza concedere loro le autorizzazioni di accesso alle tabelle di base sottostanti.  
  
-   Per fornire un'interfaccia compatibile con le versioni precedenti con cui emulare una tabella il cui schema è stato modificato.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per creare una vista tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
 È possibile creare una vista solo nel database corrente.  
  
 Una vista può includere al massimo 1.024 colonne.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Sono richieste l'autorizzazione CREATE VIEW per il database e l'autorizzazione ALTER per lo schema in cui viene creata la vista.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-create-a-view-by-using-the-query-and-view-designer"></a>Per creare una vista tramite Progettazione query e Progettazione viste  
  
1.  In **Esplora oggetti** espandere il database in cui si desidera creare la nuova vista.  
  
2.  Fare clic con il pulsante destro del mouse sulla cartella **Viste** e quindi selezionare **Nuova vista**.  
  
3.  Nella finestra di dialogo **Aggiungi tabella** selezionare gli elementi che si desidera includere nella nuova vista da una delle schede seguenti: Tabelle, Viste, Funzioni e Sinonimi.  
  
4.  Fare clic su **Aggiungi**, quindi su **Chiudi**.  
  
5.  In **Riquadro diagramma** selezionare le colonne o gli altri elementi da includere nella nuova vista.  
  
6.  Nel riquadro **Criteri** selezionare criteri di ordinamento o filtro aggiuntivi per le colonne.  
  
7.  Nel menu **File** scegliere **Salva**_view name_.  
  
8.  Nella finestra di dialogo **Scegli nome** immettere un nome per la nuova vista, quindi scegliere **OK**.  

     Per altre informazioni su Progettazione query e Progettazione viste, vedere [Strumenti di progettazione di query e viste &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
  
#### <a name="to-create-a-view"></a>Per creare una vista  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012 ;   
    GO  
    CREATE VIEW HumanResources.EmployeeHireDate  
    AS  
    SELECT p.FirstName, p.LastName, e.HireDate  
    FROM HumanResources.Employee AS e JOIN Person.Person AS  p  
    ON e.BusinessEntityID = p.BusinessEntityID ;   
    GO  
    -- Query the view  
    SELECT FirstName, LastName, HireDate  
    FROM HumanResources.EmployeeHireDate  
    ORDER BY LastName;  
  
    ```  
  
 Per altre informazioni, vedere [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  

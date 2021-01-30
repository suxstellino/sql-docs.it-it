---
description: Modifica di chiavi primarie
title: Modificare chiavi primarie | Microsoft Docs
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- modifying primary keys
- primary keys [SQL Server], modifying
ms.assetid: 8e2a15ba-1cd1-4408-b860-16c3ee37d635
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3873b6327080f0d34152c61c7519cebd6712c069
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179191"
---
# <a name="modify-primary-keys"></a>Modifica di chiavi primarie

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  È possibile modificare chiave primaria in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. È possibile modificare la chiave primaria di una tabella modificando l'ordine della colonna, il nome dell'indice, l'opzione cluster o il fattore di riempimento.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Per modificare una chiave primaria:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessario disporre dell'autorizzazione ALTER per la tabella.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-modify-a-primary-key"></a>Per modificare una chiave primaria  
  
1.  Aprire Progettazione tabelle per la tabella di cui modificare la chiave primaria, fare clic con il pulsante destro del mouse in Progettazione tabelle e scegliere **Indici/chiavi** dal menu di scelta rapida.  
  
2.  Nella finestra di dialogo **Indici/chiavi** selezionare l'indice di chiave primaria dall'elenco **Indice o chiave primaria/univoca** .  
  
3.  Completare un'operazione dalla tabella seguente:  
  
    |A|seguire le operazioni di seguito riportate|  
    |--------|------------------------|  
    |Rinominare la chiave primaria|Digitare un nuovo nome nella casella **Nome** . Scegliere un nome che non sia ancora presente nell'elenco **Indice o chiave primari/univoci selezionati** .|  
    |Impostare l'opzione cluster|Per creare un indice cluster per la chiave primaria, selezionare **Crea come CLUSTERED**, quindi selezionare l'opzione dall'elenco a discesa. Per ogni tabella è possibile creare un solo indice cluster. Se questa opzione non è disponibile per l'indice desiderato, deselezionare dapprima la relativa opzione sull'indice cluster esistente.<br /><br /> Se questa opzione non viene selezionata, viene creato un indice unico non cluster.|  
    |Definire un fattore di riempimento|Espandere la categoria **Specifica riempimento** e digitare un numero intero compreso tra 0 e 100 nella casella **Riempimento** . Per altre informazioni sui fattori di riempimento e sul loro uso, vedere [Specificare un fattore di riempimento per un indice](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).|  
    |Cambiare l'ordine delle colonne|Selezionare **Colonne**, quindi fare clic sui puntini di sospensione **(...)** a destra della proprietà. Nella finestra di dialogo  **Colonne indice** rimuovere le colonne dalla chiave primaria, quindi aggiungerle nuovamente nell'ordine desiderato. Per rimuovere una colonna dalla chiave, eliminare semplicemente il nome colonna dall'elenco **Nome colonna** .|  
  
4.  Nel menu **File** fare clic su **Salva**_nome tabella_.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
 **Per modificare una chiave primaria**  
  
 Per modificare un vincolo PRIMARY KEY utilizzando Transact-SQL, è innanzitutto necessario eliminare il vincolo PRIMARY KEY esistente e quindi crearlo di nuovo con la nuova definizione. Per ulteriori informazioni, vedere [Delete Primary Keys](../../relational-databases/tables/delete-primary-keys.md) e [Create Primary Keys](../../relational-databases/tables/create-primary-keys.md).  
  
###  <a name="TsqlExample"></a>  

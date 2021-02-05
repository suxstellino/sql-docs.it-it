---
title: Abilitare i prerequisiti per la tabella FileTable | Microsoft Docs
description: Per usare tabelle FileTable, attivare prima di tutto FILESTREAM, specificare una directory e impostare determinate opzioni e livelli di accesso. Informazioni su come soddisfare tutti i prerequisiti.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- FileTables [SQL Server], prerequisites
ms.assetid: 6286468c-9dc9-4eda-9961-071d2a36ebd6
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
ms.openlocfilehash: d5e02aa025d65fd7f3db6d1f5bd8f43e44566ac9
ms.sourcegitcommit: 58e7069b5b2b6367e27b49c002ca854b31b1159d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/04/2021
ms.locfileid: "99552664"
---
# <a name="enable-the-prerequisites-for-filetable"></a>Abilitazione dei prerequisiti per la tabella FileTable
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Viene descritto come abilitare i prerequisiti per la creazione e l'utilizzo di tabelle FileTable.  
  
##  <a name="enabling-the-prerequisites-for-filetable"></a><a name="EnablePrereq"></a> Abilitazione dei prerequisiti per la tabella FileTable  
 Per abilitare i prerequisiti per la creazione e l'utilizzo di tabelle FileTable, abilitare gli elementi seguenti:  
  
-   **A livello di istanza:**  
  
    -   [Abilitazione di FILESTREAM a livello di istanza](#BasicsFilestream)  
  
-   **A livello di database:**  
  
    -   [Fornitura di un filegroup FILESTREAM a livello di database](#filegroup)  
  
    -   [Abilitazione dell'accesso non transazionale a livello di database](#BasicsNTAccess)  
  
    -   [Specifica di una directory per tabelle FileTable a livello di database](#BasicsDirectory)  
  
##  <a name="enabling-filestream-at-the-instance-level"></a><a name="BasicsFilestream"></a> Abilitazione di FILESTREAM a livello di istanza  
 Le tabelle FileTable estendono le funzionalità della caratteristica FILESTREAM di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È pertanto necessario abilitare FILESTREAM per l'accesso I/O ai file a livello di Windows e nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prima di poter creare e usare le tabelle FileTable.  
  
###  <a name="how-to-enable-filestream-at-the-instance-level"></a><a name="HowToFilestream"></a> Procedura: Abilitare FILESTREAM a livello di istanza  
 Per informazioni su come abilitare FILESTREAM, vedere [Abilitare e configurare FILESTREAM](../../relational-databases/blob/enable-and-configure-filestream.md).  
  
 Quando si chiama **sp_configure** per abilitare FILESTREAM a livello di istanza, è necessario impostare l'opzione filestream_access_level su 2. Per altre informazioni, vedere [Opzione di configurazione del server filestream access level](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md).  
  
###  <a name="how-to-allow-filestream-through-the-firewall"></a><a name="firewall"></a> Procedura: Abilitare FILESTREAM attraverso il firewall  
 Per informazioni sull'abilitazione di FILESTREAM attraverso il firewall, vedere [Configure a Firewall for FILESTREAM Access](../../relational-databases/blob/configure-a-firewall-for-filestream-access.md).  
  
##  <a name="providing-a-filestream-filegroup-at-the-database-level"></a><a name="filegroup"></a> Fornitura di un filegroup FILESTREAM a livello di database  
 Per poter creare FileTable in un database, è necessario che il database disponga di un filegroup FILESTREAM. Per altre informazioni su questo prerequisito, vedere [Creare un database abilitato per FILESTREAM](../../relational-databases/blob/create-a-filestream-enabled-database.md).  
  
##  <a name="enabling-non-transactional-access-at-the-database-level"></a><a name="BasicsNTAccess"></a> Abilitazione dell'accesso non transazionale a livello di database  
 Le tabelle FileTable consentono alle applicazioni di Windows di ottenere un handle di file Windows per i dati FILESTREAM senza che sia necessaria una transazione. Per consentire questo accesso non transazionale ai file archiviati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è necessario specificare il livello desiderato di accesso non transazionale a livello di database per ogni database che conterrà le tabelle FileTable.  
  
###  <a name="how-to-check-whether-non-transactional-access-is-enabled-on-databases"></a><a name="HowToCheckAccess"></a> Procedura: Verificare l'abilitazione dell'accesso non transazionale nei database  
 Eseguire una query sulla vista del catalogo [sys.database_filestream_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-filestream-options-transact-sql.md) e verificare le colonne **non_transacted_access** e **non_transacted_access_desc**.  

```sql
SELECT DB_NAME(database_id), non_transacted_access, non_transacted_access_desc  
    FROM sys.database_filestream_options;  
GO  
```

###  <a name="how-to-enable-non-transactional-access-at-the-database-level"></a><a name="HowToNTAccess"></a> Procedura: Abilitare l'accesso non transazionale a livello di database  
 I livelli disponibili di accesso non transazionale sono FULL, READ_ONLY e OFF.  
  
 **Specificare il livello di accesso non transazionale tramite Transact-SQL**  
 - Quando si **crea un nuovo database**, chiamare l'istruzione [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md) con l'opzione FILESTREAM **NON_TRANSACTED_ACCESS**.

   ```sql
   CREATE DATABASE database_name  
     WITH FILESTREAM ( NON_TRANSACTED_ACCESS = FULL, DIRECTORY_NAME = N'directory_name' )  
   ```

- Quando si **modifica un database esistente**, chiamare l'istruzione [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md) con l'opzione FILESTREAM **NON_TRANSACTED_ACCESS**.

   ```sql
   ALTER DATABASE database_name  
     SET FILESTREAM ( NON_TRANSACTED_ACCESS = FULL, DIRECTORY_NAME = N'directory_name' )  
   ```

 **Specificare il livello di accesso non transazionale tramite SQL Server Management Studio**  
 È possibile specificare il livello di accesso non transazionale nel campo **Accesso FILESTREAM non transazionale** della pagina **Opzioni** della finestra di dialogo **Proprietà database** . Per altre informazioni su questa finestra di dialogo, vedere [Proprietà del database &#40;pagina Opzioni&#41;](../../relational-databases/databases/database-properties-options-page.md).  
  
##  <a name="specifying-a-directory-for-filetables-at-the-database-level"></a><a name="BasicsDirectory"></a> Specifica di una directory per tabelle FileTable a livello di database  
 Quando si abilita l'accesso non transazionale ai file a livello di database, è anche possibile specificare simultaneamente un nome di directory usando l'opzione **DIRECTORY_NAME** . Se non si specifica un nome di directory quando si abilita l'accesso non transazionale, è necessario specificarlo in seguito prima di poter creare le tabelle FileTable nel database.  
  
 Nella gerarchia di cartelle FileTable questa directory a livello di database diventa l'elemento figlio del nome di condivisione specificato per FILESTREAM a livello di istanza e l'elemento padre delle tabelle FileTable create nel database. Per altre informazioni, vedere [Work with Directories and Paths in FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md).  
  
###  <a name="how-to-specify-a-directory-for-filetables-at-the-database-level"></a><a name="HowToDirectory"></a> Procedura: Specificare una directory per tabelle FileTable a livello di database  
 Il nome specificato deve essere univoco in tutta l'istanza per le directory a livello di database.  
  
**Specificare una directory per tabelle FileTable tramite Transact-SQL**  
- Quando si **crea un nuovo database**, chiamare l'istruzione [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md) con l'opzione FILESTREAM **DIRECTORY_NAME**.

   ```sql
   CREATE DATABASE database_name  
     WITH FILESTREAM ( NON_TRANSACTED_ACCESS = FULL, DIRECTORY_NAME = N'directory_name' );  
   GO  
   ```

-   Quando si **modifica un database esistente**, chiamare l'istruzione [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md) con l'opzione FILESTREAM **DIRECTORY_NAME**. Quando si usano queste opzioni per modificare il nome di directory, è necessario bloccare il database in modo esclusivo, senza handle di file aperti.  
  
    ```sql  
    ALTER DATABASE database_name  
        SET FILESTREAM ( NON_TRANSACTED_ACCESS = FULL, DIRECTORY_NAME = N'directory_name' );  
    GO  
    ```  
  
-   Quando si **collega un database**, chiamare l'istruzione [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md) con l'opzione **FOR ATTACH** e con l'opzione FILESTREAM **DIRECTORY_NAME**.  
  
    ```sql  
    CREATE DATABASE database_name  
        FOR ATTACH WITH FILESTREAM ( DIRECTORY_NAME = N'directory_name' );  
    GO  
    ```  
  
-   Quando si **ripristina un database**, chiamare l'istruzione [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md) con l'opzione FILESTREAM **DIRECTORY_NAME**.  
  
    ```sql  
    RESTORE DATABASE database_name  
        WITH FILESTREAM ( DIRECTORY_NAME = N'directory_name' );  
    GO  
    ```  
  
 **Specificare una directory per tabelle FileTable tramite SQL Server Management Studio**  
 È possibile specificare un nome di directory nel campo **Nome di directory FILESTREAM** della pagina **Opzioni** della finestra di dialogo **Proprietà database** . Per altre informazioni su questa finestra di dialogo, vedere [Proprietà del database &#40;pagina Opzioni&#41;](../../relational-databases/databases/database-properties-options-page.md).  
  
###  <a name="how-to-view-existing-directory-names-for-the-instance"></a><a name="viewnames"></a> Procedura: Visualizzare i nomi di directory esistenti per l'istanza  
 Per visualizzare l'elenco di nomi di directory esistenti per l'istanza, eseguire una query sulla vista del catalogo [sys.database_filestream_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-filestream-options-transact-sql.md) e verificare la colonna **filestream_database_directory_name**.  
  
```sql  
SELECT DB_NAME ( database_id ), directory_name  
    FROM sys.database_filestream_options;  
GO  
```  
  
###  <a name="requirements-and-restrictions-for-the-database-level-directory"></a><a name="ReqDirectory"></a> Requisiti e restrizioni per la directory a livello di database  
  
-   L'impostazione di **DIRECTORY_NAME** è facoltativa quando si chiama **CREATE DATABASE** o **ALTER DATABASE**. Se non si specifica un valore per **DIRECTORY_NAME**, il nome di directory rimane Null. Non è tuttavia possibile creare tabelle FileTable nel database se prima non si specifica un valore per **DIRECTORY_NAME** a livello di database.  
  
-   Il nome di directory specificato deve essere conforme ai requisiti del file system relativi ai nomi di directory validi.  
  
-   Quando il database contiene tabelle FileTable, non è possibile reimpostare **DIRECTORY_NAME** su un valore Null.  
  
-   Quando si collega o ripristina un database, l'operazione non riesce se il nuovo database include un valore per **DIRECTORY_NAME** già presente nell'istanza di destinazione. Specificare un valore univoco per **DIRECTORY_NAME** quando si chiama **CREATE DATABASE FOR ATTACH** o **RESTORE DATABASE**.  
  
-   Quando si aggiorna un database esistente, il valore di **DIRECTORY_NAME** è null.  
  
-   Quando si abilita o disabilita l'accesso non transazionale a livello di database, l'operazione non verifica se è stato specificato il nome di directory o se questo è univoco.  
  
-   Quando si elimina un database abilitato per tabelle FileTable, vengono rimosse la directory a livello di database e tutte le strutture di directory di tutte le tabelle FileTable in essa contenute.  
  

---
description: Specificare valori predefiniti per le colonne
title: Specificare valori predefiniti per le colonne | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], defaults
- default values
ms.assetid: 64514aed-b846-407b-992e-cf813f9a1a91
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dc4f26877483a1213b1c5ecd3363dd1ba0c75fee
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104738101"
---
# <a name="specify-default-values-for-columns"></a>Specificare valori predefiniti per le colonne

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

È possibile usare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per specificare un valore predefinito che verrà immesso nella colonna della tabella. È possibile impostare un valore predefinito usando la funzionalità Esplora oggetti dell'interfaccia utente o inoltrando [!INCLUDE[tsql](../../includes/tsql-md.md)].

Se non si assegna un valore predefinito alla colonna e l'utente lascia la colonna vuota, si verificherà quanto segue:

- Se è stata impostata l'opzione che consente l'immissione di valori Null, nella colonna verrà inserito il valore NULL.

- Se non è stata impostata l'opzione che consente l'immissione di valori Null, la colonna resterà vuota, ma non sarà possibile salvare la riga senza avere fornito un valore per la colonna.

## <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni

Prima di iniziare, tenere presenti le limitazioni e le restrizioni seguenti:

- Se la voce nel campo **Valore predefinito** sostituisce un valore predefinito associato (visualizzato senza parentesi), verrà chiesto se separare il valore predefinito e sostituirlo con il nuovo valore.

- Per immettere una stringa di testo, è necessario racchiudere il valore tra virgolette singole ('). Non è consentito l'utilizzo delle virgolette doppie (") poiché sono riservate per gli identificatori delimitati.

- Per immettere un valore predefinito numerico immettere il numero senza virgolette.

- Per specificare un oggetto o una funzione, immetterne il nome senza racchiuderlo tra virgolette.

### <a name="security-permissions"></a><a name="Security"></a> Autorizzazioni di sicurezza

Per le azioni descritte in questo articolo è necessaria l'autorizzazione ALTER per la tabella.

## <a name="use-ssms-to-specify-a-default"></a><a name="SSMSProcedure"></a> Usare SSMS per specificare un valore predefinito

È possibile usare Esplora oggetti per specificare un valore predefinito per una colonna della tabella.

### <a name="object-explorer"></a>Esplora oggetti

1. In **Esplora oggetti** fare clic con il pulsante destro del mouse sulle colonne della tabella di cui modificare la scala e scegliere **Progetta**.

2. Selezionare la colonna per la quale si desidera specificare un valore predefinito.

3. Nella scheda **Proprietà colonne** , immettere il nuovo valore predefinito nella proprietà **Valore predefinito dell'associazione** .

   > [!NOTE]
   > Per specificare un valore predefinito numerico, immettere il numero desiderato. Per specificare un oggetto o una funzione, immetterne il nome. Per specificare un valore predefinito alfanumerico, immettere il valore racchiudendolo tra virgolette singole.

4. Nel menu **File** fare clic su **Salva** _nome tabella_.

## <a name="use-transact-sql-to-specify-a-default"></a><a name="TsqlProcedure"></a> Usare Transact-SQL per specificare un valore predefinito

Esistono vari modi per specificare un valore predefinito per una colonna usando SSMS per inviare T-SQL.

### <a name="alter-table-t-sql"></a>ALTER TABLE (T-SQL)

1. In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].

2. Sulla barra Standard fare clic su **Nuova query**.

3. Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.

   ```sql
   CREATE TABLE dbo.doc_exz (column_a INT, column_b INT); -- Allows nulls.
   GO
   INSERT INTO dbo.doc_exz (column_a) VALUES (7);
   GO
   ALTER TABLE dbo.doc_exz
     ADD CONSTRAINT DF_Doc_Exz_Column_B
     DEFAULT 50 FOR column_b;
   GO
   ```

<!--
The following two T-SQL code examples were offered by 'nycdotnet' (Steve) via public PR 1660, Feb 2019.
-->

### <a name="create-table-t-sql"></a>CREATE TABLE (T-SQL)

```sql
    CREATE TABLE dbo.doc_exz (
      column_a INT,
      column_b INT DEFAULT 50);
```

### <a name="named-constraint-t-sql"></a>CONSTRAINT con nome (T-SQL)

```sql
    CREATE TABLE dbo.doc_exz (
      column_a INT,
      column_b INT CONSTRAINT DF_Doc_Exz_Column_B DEFAULT 50);
```

Per altre informazioni, vedere [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).

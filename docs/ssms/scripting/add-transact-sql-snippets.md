---
title: Aggiunta di frammenti Transact-SQL
description: Informazioni su come aggiungere frammenti di codice Transact-SQL personalizzati al set di frammenti predefiniti incluso in SQL Server.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 901c7995-8eb5-4d12-8bb0-de0a922b48f8
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 51eaabf3196c2b7142228908dcef3587bc819b94
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595105"
---
# <a name="add-transact-sql-snippets"></a>Aggiunta di frammenti Transact-SQL

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

È possibile aggiungere frammenti di codice Transact-SQL personalizzati al set di frammenti predefiniti incluso in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="creating-a-transact-sql-snippet-file"></a>Creazione di un file di frammento Transact-SQL  
 La prima parte della creazione di un frammento di codice [!INCLUDE[tsql](../../includes/tsql-md.md)] consiste nel creare un file XML con il testo del frammento di codice. Il file deve avere un'estensione di file snippet e soddisfare i requisiti dello [schema dei frammenti di codice](/previous-versions/visualstudio/visual-studio-2015/ide/code-snippets-schema-reference). Impostare la lingua del frammento su SQL.  
  
 È possibile utilizzare i frammenti predefiniti forniti con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] come esempi. Per trovare i frammenti predefiniti, aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], selezionare il menu **Strumenti** e quindi fare clic su **Gestione frammenti di codice**. Selezionare **SQL** nella casella di riepilogo **Lingua** . Il percorso dei frammenti [!INCLUDE[tsql](../../includes/tsql-md.md)] verrà visualizzato nella casella **Percorso** .  
  
## <a name="registering-the-code-snippet"></a>Registrazione del frammento di codice  
 Dopo avere creato il file di frammento, utilizzare Gestione frammenti di codice per registrare il frammento con [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. È possibile aggiungere una cartella contenente più frammenti oppure importare singoli frammenti nella cartella **Frammenti di codice** .  
  
## <a name="procedures"></a>Procedure  
  
#### <a name="adding-a-snippet-folder"></a>Aggiunta di una cartella per i frammenti  
  
1.  Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Scegliere **Gestione frammenti di codice** dal menu **Strumenti**.  
  
3.  Fare clic su **Add** .  
  
4.  Passare alla cartella contenente i frammenti di codice e fare clic sul pulsante **Seleziona cartella** .  
  
#### <a name="importing-a-snippet"></a>Importazione di un frammento  
  
1.  Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Scegliere **Gestione frammenti di codice** dal menu **Strumenti**.  
  
3.  Fare clic sul pulsante **Import** (Importa).  
  
4.  Passare alla cartella contenente il frammento, fare clic sul file con estensione snippet e quindi sul pulsante **Apri** .  
  
## <a name="examples"></a>Esempi  
 L'esempio seguente crea un frammento di inclusione **TRY-CATCH** e lo importa in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
1.  Incollare il codice seguente in Blocco note, quindi salvare il file con il nome TryCatch.snippet.  
  
    ```  
    <?xml version="1.0" encoding="utf-8" ?>  
    <CodeSnippets  xmlns="https://schemas.microsoft.com/VisualStudio/2005/CodeSnippet">  
    <_locDefinition xmlns="urn:locstudio">  
        <_locDefault _loc="locNone" />  
        <_locTag _loc="locData">Title</_locTag>  
        <_locTag _loc="locData">Description</_locTag>  
        <_locTag _loc="locData">Author</_locTag>  
        <_locTag _loc="locData">ToolTip</_locTag>  
       <_locTag _loc="locData">Default</_locTag>  
    </_locDefinition>  
    <CodeSnippet Format="1.0.0">  
    <Header>  
    <Title>TryCatch</Title>  
                            <Shortcut></Shortcut>  
    <Description>Example Snippet for Try-Catch.</Description>  
    <Author>SQL Server Books Online Example</Author>  
    <SnippetTypes>  
                                    <SnippetType>SurroundsWith</SnippetType>  
    </SnippetTypes>  
    </Header>  
    <Snippet>  
    <Declarations>  
                                    <Literal>  
                                    <ID>CatchCode</ID>  
                                    <ToolTip>Code to handle the caught error</ToolTip>  
                                    <Default>CatchCode</Default>  
                                    </Literal>  
    </Declarations>  
    <Code Language="SQL"><![CDATA[  
    BEGIN TRY  
  
    $selected$ $end$  
  
    END TRY  
    BEGIN CATCH  
  
    $CatchCode$  
  
    END CATCH;  
    ]]>  
    </Code>  
    </Snippet>  
    </CodeSnippet>  
    </CodeSnippets>  
    ```  
  
2.  Aprire [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
3.  Scegliere **Gestione frammenti di codice** dal menu **Strumenti**.  
  
4.  Fare clic sul pulsante **Import** (Importa).  
  
5.  Passare alla cartella contenente il file TryCatch.snippet, fare clic su tale file e quindi sul pulsante **Apri** . Nella cartella **Frammenti di codice** dovrebbe essere ora presente un frammento TryCatch.  
  
## <a name="see-also"></a>Vedere anche  
 [Inserire frammenti Transact-SQL racchiusi](./insert-surround-with-transact-sql-snippets.md)  

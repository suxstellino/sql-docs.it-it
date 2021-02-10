---
description: Gestione delle password (DB2ToSQL)
title: Gestione delle password (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 07/05/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 56d546e3-8747-4169-aace-693302667e94
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 3415515af0c29e0d1cca1ddbfab89859d26c83fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014592"
---
# <a name="managing-passwords-db2tosql"></a>Gestione delle password (DB2ToSQL)
In questa sezione viene illustrata la protezione delle password del database e la procedura per importarli o esportarli tra server:  
  
1.  Sicurezza della password  
  
2.  Esportazione o importazione della password crittografata  
  
## <a name="securing-password"></a>Sicurezza della password  
SSMA consente di proteggere la password di un database.  
  
Per implementare una connessione protetta, attenersi alla procedura seguente:  
  
Specificare una password valida utilizzando uno dei tre metodi seguenti:  
  
1.  **Testo non crittografato:** Digitare la password del database nell'attributo value del nodo ' password '. Si trova nel nodo definizione server della sezione server del file di script o di connessione al server.  
  
    Le password in testo non crittografato non sono sicure. Pertanto, nell'output della console verrà visualizzato il seguente messaggio di avviso: *"server &lt; -ID &gt; password è disponibile in formato testo non protetto, l'applicazione console SSMA fornisce un'opzione per proteggere la password tramite crittografia. per ulteriori informazioni, vedere l'opzione-SecurePassword nel file della Guida di SSMA."*  
  
    **Password crittografate:** La password specificata, in questo caso, viene archiviata in formato crittografato nel computer locale in ProtectedStorage. SSMA.  
  
    -   **Protezione delle password**  
  
        -   Eseguire `SSMAforDB2Console.exe` con `-securepassword` e aggiungere switch alla riga di comando passando la connessione al server o il file di script contenente il nodo password nella sezione relativa alla definizione del server.  
  
        -   Al prompt, all'utente viene richiesto di immettere la password del database e di confermarla.  
  
            Gli ID di definizione del server e le password crittografate corrispondenti vengono archiviati in un file nel computer locale  
            
            Esempio 1:
            
            ```console
            Specify password
            C:\SSMA\SSMAforDB2Console.EXE -securepassword -add all -s "D:\Program Files\Microsoft SQL Server Migration Assistant for DB2\Sample Console Scripts\AssessmentReportGenerationSample.xml" -v "D:\Program Files\Microsoft SQL Server Migration Assistant for DB2\Sample Console Scripts\ VariableValueFileSample.xml"
            
            Enter password for server_id 'XXX_1': xxxxxxx
            
            Re-enter password for server_id 'XXX_1': xxxxxxx
            ```
            
            Esempio 2:
            
            ```console
            C:\SSMA\SSMAforDB2Console.EXE -securepassword -add "source_1,target_1" -c "D:\Program Files\Microsoft SQL Server Migration Assistant for DB2\Sample Console Scripts\ServersConnectionFileSample.xml" - v "D:\Program Files\Microsoft SQL Server Migration Assistant for DB2\Sample Console Scripts\ VariableValueFileSample.xml" -o
            
            Enter password for server_id 'source_1': xxxxxxx
            
            Re-enter password for server_id 'source_1': xxxxxxx
            
            Enter password for server_id 'target_1': xxxxxxx
            
            Re-enter password for server_id 'target _1': xxxxxxx  
            ```
    
    -   **Rimozione delle password crittografate**  
  
        Eseguire `SSMAforDB2Console.exe` con l' `-securepassword` opzione e `-remove` nella riga di comando passando gli ID server per rimuovere le password crittografate dal file di archiviazione protetto presente nel computer locale.  
  
        Esempio:  

        ```console
        C:\SSMA\SSMAforDB2Console.EXE -securepassword -remove all
        C:\SSMA\SSMAforDB2Console.EXE -securepassword -remove "source_1,target_1"
        ```

    -   **Elenco di ID server le cui password sono crittografate**  
  
        Eseguire `SSMAforDB2Console.exe` con `-securepassword` e `-list` passare alla riga di comando per elencare tutti gli ID server le cui password sono state crittografate.  
  
        Esempio:  

        ```console
        C:\SSMA\SSMAforDB2Console.EXE -securepassword -list
        ```

    > [!NOTE]  
    > 1.  La password in testo non crittografato indicato nel file di connessione dello script o del server ha la precedenza sulla password crittografata in un file protetto.  
    > 2.  Se nella sezione server del file di connessione del server o del file di script non è presente alcuna password o se non è stata protetta nel computer locale, nella console viene richiesto di immettere la password.  
  
## <a name="exporting-or-importing-encrypted-passwords"></a>Esportazione o importazione di password crittografate  
L'applicazione console SSMA consente di esportare le password di database crittografate presenti in un file nel computer locale in un file protetto e viceversa. Consente di rendere le password crittografate indipendenti dal computer.

La _funzionalità di esportazione_ legge l'ID e la password del server dall'archivio locale protetto. Il sistema salva quindi l'ID e la password in un file crittografato. All'utente viene richiesto di immettere la password per il file protetto. Verificare che la password immessa abbia una lunghezza superiore a 8 caratteri. Questo file protetto è portabile tra computer diversi.

La _funzionalità di importazione_ legge le informazioni relative all'ID server e alla password dal file protetto. All'utente viene richiesto di immettere la password per il file protetto e le informazioni vengono aggiunte all'archivio protetto locale.  

### <a name="export-example"></a>Esempio di esportazione

1. Esportare la password.

2. Immettere la password per la protezione del file esportato.

3. Esegui: &nbsp;`C:\SSMA\SSMAforDB2Console.EXE -securepassword -export all "machine1passwords.file"`

4. Immettere la password per la protezione del file esportato: xxxxxxxx

5. Confermare la password: xxxxxxxx

6. Esegui: &nbsp;`C:\SSMA\SSMAforDB2Console.EXE -p -e "DB2DB_1_1,Sql_1" "machine2passwords.file"`

7. Immettere la password per la protezione del file esportato: xxxxxxxx

8. Confermare la password: xxxxxxxx  

### <a name="import-example"></a>Esempio di importazione

1. Importare una password crittografata.

2. Immettere la password per la protezione del file importato.

3. Esegui: &nbsp;`C:\SSMA\SSMAforDB2Console.EXE -securepassword -import all "machine1passwords.file"`

4. Immettere la password per importare i server dal file crittografato: xxxxxxxx

5. Confermare la password: xxxxxxxx

6. Esegui: &nbsp;`C:\SSMA\SSMAforDB2Console.EXE -p -i "DB2DB_1,Sql_1" "machine2passwords.file"`

7. Immettere la password per importare i server dal file crittografato: xxxxxxxx

8. Conferma password: xxxxxxxx

## <a name="see-also"></a>Vedere anche  
[Esecuzione della console SSMA](./executing-the-ssma-console-db2tosql.md)  

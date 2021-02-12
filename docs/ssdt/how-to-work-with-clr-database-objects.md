---
title: Utilizzare oggetti di database CLR
description: Informazioni su come usare i linguaggi .NET Framework per recuperare e aggiornare i dati. Scoprire come creare e usare un oggetto di database Common Language Runtime (CLR) in un progetto.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.allowsqlclrdebugging
ms.assetid: 4a28d43d-eb5e-444d-aace-5df691f38709
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 02f5aa47948ee5cdbf2cc7f95d749a5f56e47328
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017951"
---
# <a name="how-to-work-with-clr-database-objects"></a>Procedura: Utilizzare oggetti di database CLR

In aggiunta al linguaggio di programmazione Transact\-SQL, è possibile usare i linguaggi .NET Framework per creare oggetti di database che recuperano e aggiornano i dati. Gli oggetti di database scritti in codice gestito vengono denominati oggetti di database CLR (Common Language Runtime) di SQL Server. Per una spiegazione dei vantaggi derivanti dall'uso di oggetti di database CLR ospitati in SQL Server, nonché per informazioni su come scegliere tra Transact\-SQL e CLR, vedere [Vantaggi dell'integrazione con CLR](../relational-databases/clr-integration/clr-integration-overview.md) e [Vantaggi dell'uso di codice gestito per creare oggetti di database](/previous-versions/visualstudio/visual-studio-2010/k2e1fb36(v=vs.100)).  
  
Per creare un oggetto di database CLR mediante SQL Server Data Tools, è necessario creare un progetto di database a cui aggiungere un oggetto di database CLR. A differenza delle versioni precedenti di Visual Studio, non è necessario creare un progetto CLR separato a cui aggiungere un riferimento dal progetto di database. Quando si compila e si pubblica il progetto di database, gli oggetti CLR vengono pubblicati automaticamente nel progetto allo stesso momento. In seguito alla pubblicazione, gli oggetti CLR possono essere chiamati ed eseguiti come qualsiasi altro oggetto di database.  
  
Nelle pagine delle proprietà CLR e Compilazione CLR sono contenute molte impostazioni per l'utilizzo di oggetti di database CLR nel progetto in uso. In particolare, nella pagina delle proprietà CLR è disponibile un'impostazione a livello di autorizzazione con cui è possibile impostare le autorizzazioni per l'assembly CLR. È inoltre disponibile l'impostazione "Genera DDL" per controllare se vengono generate istruzioni DDL per gli oggetti di database CLR aggiunti al progetto. Nella pagina delle proprietà Compilazione CLR sono contenute tutte le opzioni del compilatore che è possibile impostare per configurare la compilazione di codice CLR nel progetto. Per accedere alle pagine delle proprietà, fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e selezionare **Proprietà**.  
  
Per abilitare il debug degli oggetti di database CLR, aprire **Esplora oggetti di SQL Server**. Fare clic con il pulsante destro del mouse sul server contenente gli elementi di database CLR di cui si vuole eseguire il debug, quindi scegliere **Consenti debug di SQL/CLR**. Verrà visualizzata una finestra di messaggio con l'avviso: "Si noti che durante il debug tutti i thread gestiti sul server verranno arrestati. Abilitare il debug SQL/CLR su questo server?". Quando si esegue il debug degli oggetti di database CLR, l'interruzione dell'esecuzione interromperà tutti i thread sul server, influendo su altri utenti. Per questo motivo non deve essere eseguito il debug di applicazioni per gli oggetti di database CLR su un server di produzione. Tenere presente che una volta avviato il debug non sarà più possibile modificare le impostazioni in **Esplora oggetti di SQL Server**. Le modifiche apportate in **Esplora oggetti di SQL Server** non saranno applicate fino all'avvio della prossima sessione di debug.  
  
Per altre informazioni sui requisiti di compilazione di oggetti di database CLR, vedere [Compilazione di oggetti di database con l'integrazione con CLR (Common Language Runtime)](../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md).  
  
> [!WARNING]  
> Nelle procedure seguenti vengono usate entità create nelle procedure precedenti nelle sezioni [Sviluppo del database connesso](../ssdt/connected-database-development.md) e [Sviluppo di database offline orientato ai progetti](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-add-a-clr-database-object-to-your-project"></a>Per aggiungere un oggetto di database CLR al progetto  
  
1.  Fare clic con il pulsante destro del mouse sul progetto di database **TradeDev** in **Esplora soluzioni**, selezionare **Aggiungi**, quindi **Nuovo elemento**.  
  
2.  Selezionare il modello **C# SQL CLR**, quindi **Funzione definita dall'utente SQL CLR**. Accettare il nome predefinito e fare clic su **Aggiungi**.  
  
3.  Aggiungere il codice seguente al corpo della classe. Questa funzione consente di convalidare un numero di telefono statunitense. Tale numero deve essere costituito da 3 caratteri numerici, racchiusi facoltativamente tra parentesi, seguiti da un set di 3 caratteri numerici, quindi da un set di 4 caratteri numerici. Esempi di formati supportati sono (425) 555-0123, 425-555-0123, 425 555 0123 e 1-425-555-0123.  
  
    ```  
  
    [SqlFunction(IsDeterministic = true, IsPrecise = true)]  
    public static SqlBoolean validatePhone(SqlString phone)  
    {  
        string aNorthAmericanPhoneNumberPattern = @"^[01]?[- .]?(\([2-9]\d{2}\)|[2-9]\d{2})[- .]?\d{3}[- .]?\d{4}$";  
        if (!phone.IsNull)  
        {  
           Regex regex = new Regex(aNorthAmericanPhoneNumberPattern);  
           return regex.IsMatch(phone.Value);  
        }  
        return true;  
     }  
    ```  
  
4.  Si noti che `Regex` è sottolineato in rosso. Fare clic con il pulsante destro del mouse su `Regex` e selezionare **Risolvi**, quindi **using System.Text.RegularExpressions**.  
  
5.  Se si sviluppa in un'istanza del server Microsoft SQL Server 2012, è possibile ignorare questo passaggio. In caso contrario, SQL Server 2005 e SQL Server 2008 supportano solo progetti di database compilati con la versione 2.0, 3.0 o 3.5 di .NET Framework. Per assicurarsi che la piattaforma di destinazione .NET sia impostata correttamente, fare clic con il pulsante destro del mouse sul progetto di database **TradeDev** in **Esplora soluzioni** e selezionare **Proprietà**. Nella pagina delle proprietà **SQLCLR** impostare **Piattaforma di destinazione** su **.NET Framework 3.5** o versione precedente. Fare clic su **Sì** nella schermata finale per chiudere e riaprire il progetto.  
  
6.  Fare clic con il pulsante destro del mouse sul progetto **TradeDev** e selezionare **Compila** per compilare il progetto.  
  
7.  Fare doppio clic su Suppliers.sql e selezionare **Progettazione viste** per aprire la tabella Suppliers in Progettazione tabelle.  
  
8.  Fare clic sulla riga vuota nella Griglia colonne per aggiungere una nuova colonna alla tabella. Immettere **phone** per il campo **Nome**, **nvarchar (128)** per **Tipo di dati** e lasciare il campo **Consenti valori Null** selezionato.  
  
9. Fare clic con il pulsante destro del mouse sul nodo **Vincoli CHECK** nel riquadro Contesto e selezionare **Aggiungi nuovo vincolo CHECK**.  
  
10. Sostituire la definizione predefinita del vincolo nel riquadro di script con quanto riportato di seguito.  
  
    ```  
    CONSTRAINT [CK_Suppliers_CheckPhone] CHECK (dbo.validatePhone(phone)=1),  
    ```  
  
    In questo modo qualsiasi input nel campo del telefono nuovo sarà controllato tramite la funzione definita dall'utente CLR aggiunta in precedenza.  
  
11. Premere F5 per compilare e distribuire il progetto nel database locale.  
  
### <a name="to-use-clr-database-objects"></a>Per utilizzare oggetti di database CLR  
  
1.  In **Esplora oggetti di SQL Server** passare al database locale in cui viene distribuito il progetto.  
  
2.  Per impostazione predefinita, l'integrazione con CLR è disattivata in SQL Server. Per utilizzare oggetti di database CLR, è necessario abilitare l'integrazione CLR. A tale scopo, usare l'opzione "clr enabled" della stored procedure sp_configure. Per altre informazioni, vedere l'argomento relativo all'[opzione clr enabled](../relational-databases/clr-integration/clr-integration-enabling.md).  
  
    Fare clic con il pulsante destro del mouse sul database e selezionare **Nuova query**. Nel riquadro della query incollare il codice seguente e premere il pulsante **Esegui query**.  
  
    ```  
  
    sp_configure 'clr enabled', 1;  
    GO  
    RECONFIGURE;  
    GO  
    ```  
  
3.  Fare clic con il pulsante destro del mouse sulla tabella Suppliers e selezionare **Visualizza dati**.  
  
4.  Immettere **5** per **id**, **Contoso** per **name**, lasciare il campo **Address** vuoto e **425 3122 1222** per **phone**. Uscire dal campo **phone**. Si noti che viene visualizzato un messaggio in cui viene indicato che l'istruzione `INSERT` è in conflitto con il vincolo CHECK esistente mediante il quale viene controllato l'input del campo **phone** usando un formato di numero di telefono predefinito.  
  
5.  Impostare l'input su **425 312 1222** e uscire. Si noti che, questa volta, l'input viene accettato.  
  
## <a name="see-also"></a>Vedere anche  
[Vantaggi dell'integrazione con CLR](../relational-databases/clr-integration/clr-integration-overview.md)  
[Vantaggi dell'uso di codice gestito per creare oggetti di database](/previous-versions/visualstudio/visual-studio-2010/k2e1fb36(v=vs.100))  
[Compilazione di oggetti di database con l'integrazione con CLR (Common Language Runtime)](../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md)  

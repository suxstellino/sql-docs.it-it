---
description: Programmazione ADO in Visual C++
title: Programmazione ADO Visual C++ | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- ADO, Visual C++
- Visual C++ [ADO]
ms.assetid: 11233b96-e05c-4221-9aed-5f20944b0f1c
author: rothja
ms.author: jroth
ms.openlocfilehash: 420bfbba2eaa3946ce9a373c4ac31a88b540f7d2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028706"
---
# <a name="visual-c-ado-programming"></a>Programmazione ADO in Visual C++
Il riferimento all'API ADO descrive la funzionalità di ADO Application Programming Interface (API) usando una sintassi simile a Microsoft Visual Basic. Sebbene i destinatari siano tutti utenti, i programmatori ADO utilizzano linguaggi diversi, ad esempio Visual Basic, Visual C++ (con e senza la direttiva **#import** ) e Visual J++ (con il pacchetto della classe ADO/WFC).  

> [!NOTE]
> Microsoft ha terminato il supporto per Visual J++ in 2004.

 Per adattarsi a questa diversità, gli [indici della sintassi ADO for Visual C++](./using-ado-with-microsoft-visual-c.md) forniscono Visual C++ sintassi specifica del linguaggio con collegamenti a descrizioni comuni di funzionalità, parametri, comportamenti eccezionali e così via, nel riferimento all'API.  
  
 ADO viene implementato con le interfacce COM (Component Object Model). Tuttavia, è più facile per i programmatori usare COM in determinati linguaggi di programmazione rispetto ad altri. Quasi tutti i dettagli dell'utilizzo di COM, ad esempio, vengono gestiti in modo implicito per i programmatori di Visual Basic, mentre Visual C++ programmatori devono partecipare a tali dettagli.  
  
 Le sezioni seguenti riepilogano i dettagli dei programmatori C e C++ usando ADO e la direttiva **#import** . È incentrato sui tipi di dati specifici di COM (**Variant**, **BSTR** e **SAFEARRAY**) e sulla gestione degli errori (_com_error).  
  
## <a name="using-the-import-compiler-directive"></a>Uso della direttiva del compilatore #import  
 La direttiva del compilatore **#import** Visual C++ semplifica l'utilizzo delle proprietà e dei metodi ADO. La direttiva accetta il nome di un file contenente una libreria dei tipi, ad esempio ADO. dll (Msado15.dll), e genera file di intestazione contenenti le dichiarazioni typedef, i puntatori intelligenti per le interfacce e le costanti enumerate. Ogni interfaccia è incapsulata, o di cui è stato eseguito il wrapper, in una classe.  
  
 Per ogni operazione all'interno di una classe (ovvero una chiamata a un metodo o una proprietà), è presente una dichiarazione per chiamare direttamente l'operazione (ovvero il formato "non elaborato" dell'operazione) e una dichiarazione per chiamare l'operazione non elaborata e generare un errore COM se l'esecuzione dell'operazione non riesce correttamente. Se l'operazione è una proprietà, in genere esiste una direttiva del compilatore che crea una sintassi alternativa per l'operazione con sintassi come Visual Basic.  
  
 Le operazioni che recuperano il valore di una proprietà hanno nomi nel formato, **Get**_Property_. Le operazioni che impostano il valore di una proprietà hanno nomi del form, **put**_Property_. Le operazioni che impostano il valore di una proprietà con un puntatore a un oggetto ADO hanno nomi nel formato **PutRef**_Proprietà_.  
  
 È possibile ottenere o impostare una proprietà con le chiamate di questi moduli:  
  
```cpp
variable = objectPtr->GetProperty(); // get property value   
objectPtr->PutProperty(value);       // set property value  
objectPtr->PutRefProperty(&value);   // set property with object pointer  
```
  
## <a name="using-property-directives"></a>Uso delle direttive Property  
 La direttiva del compilatore **__declspec (Property...)** è un'estensione del linguaggio C specifica di Microsoft che dichiara una funzione utilizzata come proprietà per avere una sintassi alternativa. Di conseguenza, è possibile impostare o ottenere i valori di una proprietà in modo simile a Visual Basic. Ad esempio, è possibile impostare e ottenere una proprietà in questo modo:  
  
```cpp
objectPtr->property = value;        // set property value  
variable = objectPtr->property;     // get property value  
```
  
 Si noti che non è necessario scrivere codice:  
  
```cpp
objectPtr->PutProperty(value);      // set property value  
variable = objectPtr->GetProperty;  // get property value  
```
  
 Il compilatore genererà la  _-_ chiamata di _Proprietà_ Get, **put**-o **PutRef** appropriata in base alla sintassi alternativa dichiarata e alla possibilità di leggere o scrivere la proprietà.  
  
 La direttiva del compilatore **__declspec (Property...)** può dichiarare solo **Get**, **put** o **Get** e **inserire** la sintassi alternativa per una funzione. Le operazioni di sola lettura hanno solo una Dichiarazione **Get** ; le operazioni di sola scrittura hanno solo una Dichiarazione **put** ; le operazioni di lettura e scrittura contengono entrambe dichiarazioni **Get** e **put** .  
  
 Con questa direttiva sono possibili solo due dichiarazioni; Tuttavia, ogni proprietà può avere tre funzioni di proprietà: **Get**_Property_, **put**_Property_ e **PutRef**_Property_. In tal caso, solo due forme della proprietà hanno la sintassi alternativa.  
  
 Ad esempio, la proprietà **ActiveConnection** dell'oggetto **Command** viene dichiarata con una sintassi alternativa per **Get**_ActiveConnection_ e **PutRef**_ActiveConnection_. La sintassi **PutRef** è una scelta ottimale perché, in pratica, si vuole inserire un oggetto di **connessione** aperto (ovvero un puntatore all'oggetto di **connessione** ) in questa proprietà. D'altra parte, per l'oggetto **Recordset** sono disponibili operazioni **Get**-, **put**-e **PutRef**_ActiveConnection_ , ma nessuna sintassi alternativa.  
  
## <a name="collections-the-getitem-method-and-the-item-property"></a>Raccolte, metodo GetItem e proprietà Item  

 ADO definisce diverse raccolte, inclusi **campi**, **parametri**, **proprietà** ed **errori**. In Visual C++, il metodo **GetItem (_index_)** restituisce un membro della raccolta. *Index* è una **variante**, il cui valore è un indice numerico del membro nella raccolta o una stringa contenente il nome del membro.  
  
 La direttiva del compilatore **__declspec (Property...)** dichiara la proprietà **Item** come sintassi alternativa al metodo **GetItem ()** fondamentale di ogni raccolta. La sintassi alternativa usa le parentesi quadre e ha un aspetto simile a un riferimento alla matrice. In generale, i due formati hanno un aspetto simile al seguente:  
  
```cpp
  
      collectionPtr->GetItem(index);  
collectionPtr->Item[index];  
```
  
 Ad esempio, assegnare un valore a un campo di un oggetto **Recordset** , denominato **_RS_**, derivato dalla tabella **authors** del database **pubs** . Utilizzare la proprietà **Item ()** per accedere al terzo **campo** della raccolta dei **campi** dell'oggetto **Recordset** (le raccolte sono indicizzate da zero; si supponga che il terzo campo sia denominato **_au \_ fname_**). Chiamare quindi il metodo **value ()** sull'oggetto **Field** per assegnare un valore stringa.  
  
 Questo può essere espresso in Visual Basic nei quattro modi seguenti (le ultime due forme sono univoche per Visual Basic; gli altri linguaggi non hanno equivalenti):  
  
```cpp
rs.Fields.Item(2).Value = "value"  
rs.Fields.Item("au_fname").Value = "value"  
rs(2) = "value"  
rs!au_fname = "value"  
```
  
 L'equivalente in Visual C++ alle prime due forme precedenti è:  
  
```cpp
rs->Fields->GetItem(long(2))->PutValue("value");   
rs->Fields->GetItem("au_fname")->PutValue("value");  
```
  
 -o-(viene visualizzata anche la sintassi alternativa per la proprietà **value** )  
  
```cpp
rs->Fields->Item[long(2)]->Value = "value";  
rs->Fields->Item["au_fname"]->Value = "value";  
```
  
 Per esempi di iterazione in una raccolta, vedere la sezione "raccolte ADO" di "riferimento ADO".  
  
## <a name="com-specific-data-types"></a>Tipi di dati COM-Specific  
 In generale, qualsiasi tipo di dati Visual Basic trovato nel riferimento all'API ADO ha un Visual C++ equivalente. Sono inclusi i **tipi di dati** standard **come unsigned char** per un Visual Basic **byte**, **short** per **Integer** e **Long** . Esaminare la sintassi Indexesto vedere esattamente ciò che è necessario per gli operandi di un metodo o di una proprietà specifica.  
  
 Le eccezioni a questa regola sono i tipi di dati specifici di COM: **Variant**, **BSTR** e **SAFEARRAY**.  
  
### <a name="variant"></a>Variant  
 Una **variante** è un tipo di dati strutturato che contiene un membro del valore e un membro del tipo di dati. Una **variante** può contenere un'ampia gamma di altri tipi di dati, tra cui un altro Variant, BSTR, Boolean, IDispatch o un puntatore IUnknown, valuta, data e così via. COM fornisce anche metodi che facilitano la conversione di un tipo di dati in un altro.  
  
 La classe **_variant_t** incapsula e gestisce il tipo di dati **Variant** .  
  
 Quando il riferimento all'API ADO indica che l'operando di un metodo o di una proprietà accetta un valore, in genere significa che il valore viene passato in un **_variant_t**.  
  
 Questa regola è esplicitamente true quando la sezione **Parameters** negli argomenti di riferimento all'API ADO indica che un operando è una **variante**. Un'eccezione si verifica quando la documentazione indica in modo esplicito che l'operando accetta un tipo di dati standard, ad esempio **Long** o **byte**, oppure un'enumerazione. Un'altra eccezione si verifica quando l'operando accetta una **stringa**.  
  
### <a name="bstr"></a>BSTR  
 Un **BSTR** (**B** ASIC **Str** Ing) è un tipo di dati strutturato che contiene una stringa di caratteri e la lunghezza della stringa. COM fornisce metodi per allocare, modificare e liberare un **BSTR**.  
  
 La classe **_bstr_t** incapsula e gestisce il tipo di dati **BSTR** .  
  
 Quando il riferimento all'API ADO indica che un metodo o una proprietà accetta un valore **stringa** , significa che il valore è nel formato di un **_bstr_t**.  
  
### <a name="casting-_variant_t-and-_bstr_t-classes"></a>Cast delle classi _variant_t e _bstr_t  
 Spesso non è necessario codificare in modo esplicito un **_variant_t** o **_bstr_t** in un argomento di un'operazione. Se la classe **_variant_t** o **_bstr_t** dispone di un costruttore che corrisponde al tipo di dati dell'argomento, il compilatore genererà il **_variant_t** o **_bstr_t** appropriato.  
  
 Tuttavia, se l'argomento è ambiguo, ovvero il tipo di dati dell'argomento corrisponde a più di un costruttore, è necessario eseguire il cast dell'argomento con il tipo di dati appropriato per richiamare il costruttore corretto.  
  
 La dichiarazione per il metodo **Recordset:: Open** , ad esempio, è:  
  
```cpp
    HRESULT Open (  
        const _variant_t & Source,  
        const _variant_t & ActiveConnection,  
        enum CursorTypeEnum CursorType,  
        enum LockTypeEnum LockType,  
        long Options );  
```
  
 L' `ActiveConnection` argomento accetta un riferimento a un **_variant_t**, che può essere codificato come una stringa di connessione o un puntatore a un oggetto **connessione** aperta.  
  
 Il **_variant_t** corretto verrà creato in modo implicito se si passa una stringa come " `DSN=pubs;uid=MyUserName;pwd=MyPassword;` " o un puntatore come " `(IDispatch *) pConn` ".  
  
> [!NOTE]
>  Se ci si connette a un provider dell'origine dati che supporta l'autenticazione di Windows, è necessario specificare **Trusted_Connection = Yes** o **Integrated Security = SSPI** anziché le informazioni relative a ID utente e password nella stringa di connessione.  
  
 In alternativa, è possibile codificare in modo esplicito un **_variant_t** contenente un puntatore, ad esempio " `_variant_t((IDispatch *) pConn, true)` ". Il cast, `(IDispatch *)` , risolve l'ambiguità con un altro costruttore che accetta un puntatore a un'interfaccia IUnknown.  
  
 Si tratta di un aspetto cruciale, sebbene raramente noto, che ADO è un'interfaccia IDispatch. Ogni volta che un puntatore a un oggetto ADO deve essere passato come **Variant**, è necessario eseguire il cast di tale puntatore come puntatore a un'interfaccia IDispatch.  
  
 L'ultimo caso codifica in modo esplicito il secondo argomento booleano del costruttore con il relativo valore predefinito facoltativo `true` . Questo argomento fa sì che il costruttore **Variant** chiami il metodo **AddRef**(), che compensa per ADO la chiamata automatica al metodo **_variant_t:: Release**() quando il metodo ADO o la chiamata di proprietà viene completata.  
  
### <a name="safearray"></a>SafeArray  
 **SAFEARRAY** è un tipo di dati strutturato che contiene una matrice di altri tipi di dati. Un **SAFEARRAY** viene chiamato *Safe* perché contiene informazioni sui limiti di ogni dimensione della matrice e limita l'accesso agli elementi della matrice in tali limiti.  
  
 Quando il riferimento all'API ADO indica che un metodo o una proprietà accetta o restituisce una matrice, significa che il metodo o la proprietà accetta o restituisce un **SAFEARRAY**, non una matrice C/C++ nativa.  
  
 Il secondo parametro del metodo **OpenSchema** dell'oggetto **Connection** , ad esempio, richiede una matrice di valori **Variant** . Tali valori **Variant** devono essere passati come elementi di **SAFEARRAY** e tale **SAFEARRAY** deve essere impostato come valore di un'altra **variante**. Si tratta di un'altra **variante** che viene passata come secondo argomento di **OpenSchema**.  
  
 Come ulteriore esempio, il primo argomento del metodo **Find** è una **variante** il cui valore è un **SAFEARRAY** unidimensionale; ognuno degli argomenti primo e secondo facoltativi di **AddNew** è un **SAFEARRAY** unidimensionale; e il valore restituito del metodo **GetRows** è una **variante** il cui valore è un **SAFEARRAY** bidimensionale.  
  
## <a name="missing-and-default-parameters"></a>Parametri mancanti e predefiniti  
 Visual Basic consente parametri mancanti nei metodi. Il metodo **Open** dell'oggetto **Recordset** , ad esempio, ha cinque parametri, ma è possibile ignorare i parametri intermedi e lasciare i parametri finali. Un **BSTR** o una **variante** predefinita verrà sostituito a seconda del tipo di dati dell'operando mancante.  
  
 In C/C++ è necessario specificare tutti gli operandi. Se si desidera specificare un parametro mancante il cui tipo di dati è una stringa, specificare un **_bstr_t** contenente una stringa null. Se si desidera specificare un parametro mancante il cui tipo di dati è una **variante**, specificare un **_variant_t** con un valore DISP_E_PARAMNOTFOUND e un tipo di VT_ERROR. In alternativa, specificare la costante **_variant_t** equivalente, **vtMissing**, fornita dalla direttiva **#import** .  
  
 Tre metodi sono eccezioni all'uso tipico di **vtMissing**. Questi sono i metodi **Execute** degli oggetti **Connection** e **Command** e il metodo **NextRecordset** dell'oggetto **Recordset** . Di seguito sono riportate le firme:  
  
```cpp
_RecordsetPtr <A HREF="mdmthcnnexecute.htm">Execute</A>( _bstr_t CommandText, VARIANT * RecordsAffected,   
        long Options );  // Connection  
_RecordsetPtr <A HREF="mdmthcmdexecute.htm">Execute</A>( VARIANT * RecordsAffected, VARIANT * Parameters,   
        long Options );  // Command  
_RecordsetPtr <A HREF="mdmthnextrec.htm">NextRecordset</A>( VARIANT * RecordsAffected );  // Recordset  
```
  
 I parametri, *RecordsAffected* e *Parameters*, sono puntatori a una **variante**. *Parameters* è un parametro di input che specifica l'indirizzo di una **variante** contenente un singolo parametro, o una matrice di parametri, che modificherà il comando in esecuzione. *RecordsAffected* è un parametro di output che specifica l'indirizzo di una **variante**, in cui viene restituito il numero di righe interessate dal metodo.  
  
 Nel metodo **Execute** dell'oggetto **Command** , indicare che non viene specificato alcun parametro impostando *Parameters* su `&vtMissing` (opzione consigliata) o sul puntatore null (ovvero **null** o zero (0)). Se *Parameters* è impostato sul puntatore null, il metodo sostituisce internamente l'equivalente di **vtMissing**, quindi completa l'operazione.  
  
 In tutti i metodi, indicare che il numero di record interessati non deve essere restituito impostando *RecordsAffected* sul puntatore null. In questo caso, il puntatore null non è molto un parametro mancante per indicare che il metodo deve eliminare il numero di record interessati.  
  
 Per questi tre metodi, quindi, è possibile scrivere codice simile al seguente:  
  
```cpp
pConnection->Execute("commandText", NULL, adCmdText);   
pCommand->Execute(NULL, NULL, adCmdText);  
pRecordset->NextRecordset(NULL);  
```
  
## <a name="error-handling"></a>Gestione degli errori  
 In COM, la maggior parte delle operazioni restituisce un codice restituito HRESULT che indica se una funzione è stata completata correttamente. La direttiva **#import** genera il codice wrapper intorno a ogni proprietà o metodo "Raw" e controlla l'HRESULT restituito. Se HRESULT indica un errore, il codice wrapper genera un errore COM chiamando _com_issue_errorex () con il codice restituito HRESULT come argomento. Gli oggetti errore com possono essere rilevati in un blocco **try** - **catch** . (Per motivi di efficienza, rilevare un riferimento a un oggetto **_com_error** ).  
  
 Si noti che si tratta di errori ADO: risultanti dall'operazione ADO non riuscita. Gli errori restituiti dal provider sottostante vengono visualizzati come oggetti **errore** nella raccolta **errori** oggetto **connessione** .  
  
 La direttiva **#import** crea solo routine di gestione degli errori per metodi e proprietà dichiarati in ADO. dll. Tuttavia, è possibile sfruttare lo stesso meccanismo di gestione degli errori scrivendo la macro o la funzione inline con il controllo degli errori. Per esempi, vedere l'argomento [Visual C++ Extensions](./visual-c-extensions-for-ado.md)o il codice illustrato nelle sezioni seguenti.  
  
## <a name="visual-c-equivalents-of-visual-basic-conventions"></a>Equivalenti Visual C++ delle convenzioni di Visual Basic  
 Di seguito è riportato un riepilogo di diverse convenzioni nella documentazione di ADO, codificate in Visual Basic, nonché gli equivalenti in Visual C++.  
  
### <a name="declaring-an-ado-object"></a>Dichiarazione di un oggetto ADO  
 In Visual Basic, una variabile oggetto ADO (in questo caso per un oggetto **Recordset** ) viene dichiarata come segue:  
  
```vb
Dim rst As ADODB.Recordset  
```
  
 La clausola " `ADODB.Recordset` " è il ProgID dell'oggetto **Recordset** come definito nel registro di sistema. Una nuova istanza di un oggetto **record** viene dichiarata come segue:  
  
```vb
Dim rst As New ADODB.Recordset  
```
  
 -oppure-  
  
```vb
Dim rst As ADODB.Recordset  
Set rst = New ADODB.Recordset  
```
  
 In Visual C++ la direttiva **#import** genera dichiarazioni di tipo puntatore intelligente per tutti gli oggetti ADO. Ad esempio, una variabile che punta a un oggetto **_Recordset** è di tipo **_RecordsetPtr** e viene dichiarata come segue:  
  
```cpp
_RecordsetPtr  rs;  
```
  
 Una variabile che punta a una nuova istanza di un oggetto **_Recordset** viene dichiarata come segue:  
  
```cpp
_RecordsetPtr  rs("ADODB.Recordset");  
```
  
 -oppure-  
  
```cpp
_RecordsetPtr  rs;  
rs.CreateInstance("ADODB.Recordset");  
```
  
 -oppure-  
  
```cpp
_RecordsetPtr  rs;  
rs.CreateInstance(__uuidof(_Recordset));  
```
  
 Una volta chiamato il metodo **CreateInstance** , la variabile può essere usata come indicato di seguito:  
  
```cpp
rs->Open(...);  
```
  
 Si noti che in un caso l' `.` operatore "" viene usato come se la variabile fosse un'istanza di una classe ( `rs.CreateInstance` ) e in un altro caso, l' `->` operatore "" viene usato come se la variabile fosse un puntatore a un'interfaccia ( `rs->Open` ).  
  
 Una variabile può essere usata in due modi perché l' `->` operatore "" è in overload per consentire a un'istanza di una classe di comportarsi come un puntatore a un'interfaccia. Un membro della classe privata della variabile di istanza contiene un puntatore all'interfaccia **_Recordset** ; l' `->` operatore "" restituisce il puntatore e il puntatore restituito accede ai membri dell'oggetto **_Recordset** .  
  
### <a name="coding-a-missing-parameter---string"></a>Codifica di una stringa di parametro mancante  
 Quando è necessario codificare un operando **stringa** mancante in Visual Basic, si omette semplicemente l'operando. È necessario specificare l'operando in Visual C++. Codificare un **_bstr_t** con una stringa vuota come valore.  
  
```cpp
_bstr_t strMissing(L"");  
```
  
### <a name="coding-a-missing-parameter---variant"></a>Codifica di un parametro mancante-variante  
 Quando è necessario codificare un operando **Variant** mancante in Visual Basic, si omette semplicemente l'operando. È necessario specificare tutti gli operandi in Visual C++. Codificare un parametro **Variant** mancante con un **_variant_t** impostato sul valore speciale, DISP_E_PARAMNOTFOUND e sul tipo VT_ERROR. In alternativa, specificare **vtMissing**, che è una costante predefinita equivalente fornita dalla direttiva **#import** .  
  
```cpp
_variant_t  vtMissingYours(DISP_E_PARAMNOTFOUND, VT_ERROR);   
```
  
 -oppure usare-  
  
```cpp
...vtMissing...;  
```
  
### <a name="declaring-a-variant"></a>Dichiarazione di una variante  
 In Visual Basic, una **variante** viene dichiarata con l'istruzione **Dim** come indicato di seguito:  
  
```vb
Dim VariableName As Variant  
```
  
 In Visual C++ dichiarare una variabile come tipo **_variant_t**. Di seguito sono illustrate alcune dichiarazioni di **_variant_t** schematiche.  
  
> [!NOTE]
>  Queste dichiarazioni forniscono semplicemente un'idea approssimativa di ciò che si codifica nel programma. Per ulteriori informazioni, vedere gli esempi riportati di seguito e la documentazione di Visual C++.  
  
```cpp
_variant_t  VariableName(value);  
_variant_t  VariableName((data type cast) value);  
_variant_t  VariableName(value, VT_DATATYPE);  
_variant_t  VariableName(interface * value, bool fAddRef = true);  
```
  
### <a name="using-arrays-of-variants"></a>Uso di matrici di varianti  
 In Visual Basic, le matrici di **varianti** possono essere codificate con l'istruzione **Dim** oppure è possibile usare la funzione di **matrice** , come illustrato nel codice di esempio seguente:  
  
```vb
Public Sub ArrayOfVariants  
Dim cn As ADODB.Connection  
Dim rs As ADODB.Recordset  
Dim fld As ADODB.Field  
  
    cn.Open "DSN=pubs"  
    rs = cn.OpenSchema(adSchemaColumns, _  
        Array(Empty, Empty, "authors", Empty))  
    For Each fld in rs.Fields  
        Debug.Print "Name = "; fld.Name  
    Next fld  
    rs.Close  
    cn.Close  
End Sub  
```
  
 Nell'esempio di Visual C++ seguente viene illustrato l'utilizzo di un **SAFEARRAY** utilizzato con una **_variant_t**.  
  
#### <a name="notes"></a>Note  
 Le note seguenti corrispondono alle sezioni con commenti nell'esempio di codice.  
  
1.  Ancora una volta, la funzione inline TESTHR () viene definita per sfruttare il meccanismo di gestione degli errori esistente.  
  
2.  È necessaria solo una matrice unidimensionale, quindi è possibile usare **SafeArrayCreateVector**, invece della Dichiarazione **SAFEARRAYBOUND** per utilizzo generico e della funzione **SafeArrayCreate** . Di seguito è riportato il codice che dovrebbe essere simile all'uso di **SafeArrayCreate**:  
  
    ```cpp
       SAFEARRAYBOUND   sabound[1];  
       sabound[0].lLbound = 0;  
       sabound[0].cElements = 4;  
       pSa = SafeArrayCreate(VT_VARIANT, 1, sabound);  
    ```
  
3.  Lo schema identificato dalla costante enumerata, **adSchemaColumns**, è associato a quattro colonne del vincolo: TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME e column_name. Viene pertanto creata una matrice di valori **Variant** con quattro elementi. Viene quindi specificato un valore di vincolo che corrisponde alla terza colonna, TABLE_NAME.  
  
     Il **Recordset** restituito è costituito da più colonne, un subset di che rappresenta le colonne del vincolo. I valori delle colonne vincolo per ogni riga restituita devono corrispondere ai valori di vincolo corrispondenti.  
  
4.  Coloro che hanno familiarità con **SafeArrays** possono essere sorpresi che **SafeArrayDestroy**() non viene chiamato prima dell'uscita. In realtà, la chiamata a **SafeArrayDestroy**() in questo caso provocherà un'eccezione in fase di esecuzione. Il motivo è che il distruttore per chiamerà `vtCriteria` **VariantClear**() quando il **_variant_t** esce dall'ambito, che consente di liberare **SAFEARRAY**. Se si chiama **SafeArrayDestroy**, senza cancellare manualmente il **_variant_t**, il distruttore tenterà di cancellare un puntatore **SAFEARRAY** non valido.  
  
     Se è stato chiamato **SafeArrayDestroy** , il codice sarà simile al seguente:  
  
    ```cpp
          TESTHR(SafeArrayDestroy(pSa));  
       vtCriteria.vt = VT_EMPTY;  
          vtCriteria.parray = NULL;  
    ```
  
     Tuttavia, è molto più semplice consentire al **_variant_t** di gestire **SAFEARRAY**.  
  
```cpp
// Visual_CPP_ADO_Prog_1.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
// Note 1  
inline void TESTHR( HRESULT _hr ) {   
   if FAILED(_hr)   
      _com_issue_error(_hr);   
}  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _RecordsetPtr pRs("ADODB.Recordset");  
      _ConnectionPtr pCn("ADODB.Connection");  
      _variant_t vtTableName("authors"), vtCriteria;  
      long ix[1];  
      SAFEARRAY *pSa = NULL;  
  
      pCn->Provider = "sqloledb";  
      pCn->Open("Data Source='(local)';Initial Catalog='pubs';Integrated Security=SSPI", "", "", adConnectUnspecified);  
      // Note 2, Note 3  
      pSa = SafeArrayCreateVector(VT_VARIANT, 1, 4);  
      if (!pSa)   
         _com_issue_error(E_OUTOFMEMORY);  
  
      // Specify TABLE_NAME in the third array element (index of 2).   
      ix[0] = 2;        
      TESTHR(SafeArrayPutElement(pSa, ix, &vtTableName));  
  
      // There is no Variant constructor for a SafeArray, so manually set the   
      // type (SafeArray of Variant) and value (pointer to a SafeArray).  
  
      vtCriteria.vt = VT_ARRAY | VT_VARIANT;  
      vtCriteria.parray = pSa;  
  
      pRs = pCn->OpenSchema(adSchemaColumns, vtCriteria, vtMissing);  
  
      long limit = pRs->GetFields()->Count;  
      for ( long x = 0 ; x < limit ; x++ )  
         printf( "%d: %s\n", x + 1, ((char*) pRs->GetFields()->Item[x]->Name) );  
      // Note 4  
      pRs->Close();  
      pCn->Close();  
   }  
   catch (_com_error &e) {  
      printf("Error:\n");  
      printf("Code = %08lx\n", e.Error());  
      printf("Code meaning = %s\n", (char*) e.ErrorMessage());  
      printf("Source = %s\n", (char*) e.Source());  
      printf("Description = %s\n", (char*) e.Description());  
   }  
   CoUninitialize();  
}  
```
  
### <a name="using-property-getputputref"></a>Uso della proprietà get/put/PutRef  
 In Visual Basic, il nome di una proprietà non è qualificato dal fatto che sia stato recuperato, assegnato o assegnato un riferimento.  
  
```vb
Public Sub GetPutPutRef  
Dim rs As New ADODB.Recordset  
Dim cn As New ADODB.Connection  
Dim sz as Integer  
cn.Open "Provider=sqloledb;Data Source=yourserver;" & _  
         "Initial Catalog=pubs;Integrated Security=SSPI;"  
rs.PageSize = 10  
sz = rs.PageSize  
rs.ActiveConnection = cn  
rs.Open "authors",,adOpenStatic  
' ...  
rs.Close  
cn.Close  
End Sub  
```
  
 Questo Visual C++ esempio illustra la **Proprietà Get** / **put** / **PutRef**.  
  
#### <a name="notes"></a>Note  
 Le note seguenti corrispondono alle sezioni con commenti nell'esempio di codice.  
  
1.  In questo esempio vengono utilizzate due forme di un argomento stringa mancante, ovvero una costante esplicita, **strMissing**, e una stringa che verrà utilizzata dal compilatore per creare una **_bstr_t** temporanea esistente per l'ambito del metodo **Open** .  
  
2.  Non è necessario eseguire il cast dell'operando di `rs->PutRefActiveConnection(cn)` a `(IDispatch *)` perché il tipo dell'operando è già `(IDispatch *)` .  
  
```cpp
// Visual_CPP_ado_prog_2.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr cn("ADODB.Connection");  
      _RecordsetPtr rs("ADODB.Recordset");  
      _bstr_t strMissing(L"");  
      long oldPgSz = 0, newPgSz = 5;  
  
      // Note 1  
      cn->Provider = "sqloledb";  
      cn->Open("Data Source='(local)';Initial Catalog=pubs;Integrated Security=SSPI;", strMissing, "", adConnectUnspecified);  
  
      oldPgSz = rs->GetPageSize();  
      // -or-  
      // oldPgSz = rs->PageSize;  
  
      rs->PutPageSize(newPgSz);  
      // -or-  
      // rs->PageSize = newPgSz;  
  
      // Note 2  
      rs->PutRefActiveConnection( cn );  
      rs->Open("authors", vtMissing, adOpenStatic, adLockReadOnly, adCmdTable);  
      printf("Original pagesize = %d, new pagesize = %d\n", oldPgSz, rs->GetPageSize());  
      rs->Close();  
      cn->Close();  
  
   }  
   catch (_com_error &e) {  
      printf("Description = %s\n", (char*) e.Description());  
   }  
   ::CoUninitialize();  
}  
```
  
### <a name="using-getitemx-and-itemx"></a>Utilizzo di GetItem (x) e Item [x]  
 Questo Visual Basic esempio illustra la sintassi standard e alternativa per **Item**().  
  
```vb
Public Sub GetItemItem  
Dim rs As New ADODB.Recordset  
Dim name as String  
rs = rs.Open "authors", "DSN=pubs;", adOpenDynamic, _  
         adLockBatchOptimistic, adTable  
name = rs(0)  
' -or-  
name = rs.Fields.Item(0)  
rs(0) = "Test"  
rs.UpdateBatch  
' Restore name  
rs(0) = name  
rs.UpdateBatch  
rs.Close  
End Sub  
```
  
 In questo Visual C++ esempio viene illustrato l' **elemento**.  
  
> [!NOTE]
>  La nota seguente corrisponde alle sezioni commentate nell'esempio di codice: quando si accede alla raccolta con **Item**, è necessario eseguire il cast dell'indice, **2**, a **Long** , in modo che venga richiamato un costruttore appropriato.  
  
```cpp
// Visual_CPP_ado_prog_3.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
void main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr cn("ADODB.Connection");  
      _RecordsetPtr rs("ADODB.Recordset");  
      _variant_t vtFirstName;  
  
      cn->Provider = "sqloledb";  
      cn->Open("Data Source='(local)';Initial Catalog=pubs;Integrated Security=SSPI;", "", "", adConnectUnspecified);  
  
      rs->PutRefActiveConnection( cn );  
      rs->Open("authors", vtMissing, adOpenStatic, adLockOptimistic, adCmdTable);  
      rs->MoveFirst();  
  
      // Note 1. Get a field.  
      vtFirstName = rs->Fields->GetItem((long)2)->GetValue();  
      // -or-  
      vtFirstName = rs->Fields->Item[(long)2]->Value;  
  
      printf( "First name = '%s'\n", (char*)( (_bstr_t)vtFirstName) );  
  
      rs->Fields->GetItem((long)2)->Value = L"TEST";  
      rs->Update(vtMissing, vtMissing);  
  
      // Restore name  
      rs->Fields->GetItem((long)2)->PutValue(vtFirstName);  
      // -or-  
      rs->Fields->GetItem((long)2)->Value = vtFirstName;  
      rs->Update(vtMissing, vtMissing);  
      rs->Close();  
   }  
   catch (_com_error &e) {  
      printf("Description = '%s'\n", (char*) e.Description());  
   }  
   ::CoUninitialize();  
}  
```
  
### <a name="casting-ado-object-pointers-with-idispatch-"></a>Cast di puntatori a oggetti ADO con (IDispatch *)  
 Nell'esempio di Visual C++ seguente viene illustrato l'utilizzo di (IDispatch *) per eseguire il cast dei puntatori a oggetti ADO.  
  
#### <a name="notes"></a>Note  
 Le note seguenti corrispondono alle sezioni con commenti nell'esempio di codice.  
  
1.  Specificare un oggetto **Connection** aperto in una **variante** codificata in modo esplicito. Eseguirne il cast con (IDispatch), in \* modo che venga richiamato il costruttore corretto. Inoltre, impostare in modo esplicito il secondo parametro **_variant_t** sul valore predefinito **true**, in modo che il conteggio dei riferimenti all'oggetto venga corretto al termine dell'operazione **Recordset:: Open** .  
  
2.  L'espressione, `(_bstr_t)` , non è un cast, ma un operatore **_variant_t** che estrae una stringa di **_bstr_t** dal **Variant** restituito per **valore**.  
  
 L'espressione, `(char*)` , non è un cast, ma un operatore **_bstr_t** che estrae un puntatore alla stringa incapsulata in un oggetto **_bstr_t** .  
  
 In questa sezione di codice vengono illustrati alcuni dei comportamenti utili degli operatori **_variant_t** e **_bstr_t** .  
  
```cpp
// Visual_CPP_ado_prog_4.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr pConn("ADODB.Connection");  
      _RecordsetPtr pRst("ADODB.Recordset");  
  
      pConn->Provider = "sqloledb";  
      pConn->Open("Data Source='(local)';Initial Catalog='pubs';Integrated Security=SSPI", "", "", adConnectUnspecified);  
  
      // Note 1.  
      pRst->Open("authors", _variant_t((IDispatch *) pConn, true), adOpenStatic, adLockReadOnly, adCmdTable);  
      pRst->MoveLast();  
  
      // Note 2.  
      printf("Last name is '%s %s'\n",   
         (char*) ((_bstr_t) pRst->GetFields()->GetItem("au_fname")->GetValue()),  
         (char*) ((_bstr_t) pRst->Fields->Item["au_lname"]->Value));  
  
      pRst->Close();  
      pConn->Close();  
   }  
   catch (_com_error &e) {  
      printf("Description = '%s'\n", (char*) e.Description());  
   }     
   ::CoUninitialize();  
}  
```
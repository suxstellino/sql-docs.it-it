---
title: API di estendibilità
description: Informazioni sulle API di estendibilità di Azure Data Studio, che consentono alle estensioni di interagire con altre parti di Azure Data Studio, ad esempio Esplora oggetti.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: d795d96d47fa7f07744c492d8639cdecdc2e23fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052157"
---
# <a name="azure-data-studio-extensibility-apis"></a>API di estendibilità Azure Data Studio

Azure Data Studio mette a disposizione un'API che le estensioni possono usare per interagire con altre parti di Azure Data Studio, ad esempio Esplora oggetti. Queste API sono disponibili dal file [`src/sql/azdata.d.ts`](https://github.com/Microsoft/azuredatastudio/blob/main/src/sql/azdata.d.ts) e sono descritte di seguito.

## <a name="connection-management"></a>Gestione connessioni
`azdata.connection`

### <a name="top-level-functions"></a>Funzioni di primo livello

- `getCurrentConnection(): Thenable<azdata.connection.Connection>` Ottiene la connessione corrente in base all'editor attivo o alla selezione in Esplora oggetti.

- `getActiveConnections(): Thenable<azdata.connection.Connection[]>` Ottiene un elenco di tutte le connessioni dell'utente attive. Restituisce un elenco vuoto se non sono presenti connessioni di questo tipo.

- `getCredentials(connectionId: string): Thenable<{ [name: string]: string }>` Ottiene un dizionario che contiene le credenziali associate a una connessione. Queste verrebbero altrimenti restituite come parte del dizionario delle opzioni in un oggetto `azdata.connection.Connection`, ma rimosse da tale oggetto. 

### `Connection`
- `options: { [name: string]: string }` Dizionario delle opzioni di connessione
- `providerName: string` Nome del provider di connessione (ad esempio "MSSQL")
- `connectionId: string` Identificatore univoco della connessione

### <a name="example-code"></a>Codice di esempio
```
> let connection = azdata.connection.getCurrentConnection();
connection: {
    providerName: 'MSSQL',
    connectionId: 'd97bb63a-466e-4ef0-ab6f-00cd44721dcc',
    options: {
        server: 'mairvine-sql-server',
        user: 'sa',
        authenticationType: 'sqlLogin',
        ...
    },
    ...
}
> let credentials = azdata.connection.getCredentials(connection.connectionId);
credentials: {
    password: 'abc123'
}

```

## <a name="object-explorer"></a>Esplora oggetti

`azdata.objectexplorer`


### <a name="top-level-functions"></a>Funzioni di primo livello
- `getNode(connectionId: string, nodePath?: string): Thenable<azdata.objectexplorer.ObjectExplorerNode>` Ottiene un nodo Esplora oggetti corrispondente alla connessione e al percorso specificati. Se non viene specificato alcun percorso, viene restituito il nodo di primo livello per la connessione specificata. Se nel percorso specificato non è presente alcun nodo, restituisce `undefined`. Nota: La variabile `nodePath` per un oggetto viene generata dal back-end del servizio SQL Tools ed è difficile da creare manualmente. I miglioramenti futuri dell'API consentiranno di ottenere i nodi in base ai metadati forniti sul nodo, ad esempio nome, tipo e schema.

- `getActiveConnectionNodes(): Thenable<azdata.objectexplorer.ObjectExplorerNode>` Ottiene tutti i nodi di connessione di Esplora oggetti attivi.

- `findNodes(connectionId: string, type: string, schema: string, name: string, database: string, parentObjectNames: string[]): Thenable<azdata.objectexplorer.ObjectExplorerNode[]>` Trova tutti i nodi di Esplora oggetti che corrispondono ai metadati specificati. Gli argomenti `schema`, `database` e `parentObjectNames` devono essere `undefined` se non sono applicabili. `parentObjectNames` è un elenco di oggetti padre non di database, dal livello più alto a quello più basso in Esplora oggetti, in cui si trova l'oggetto desiderato. Ad esempio, quando si cerca una colonna "column1" appartenente a una tabella "schema1.table1" e al database "database1" con ID di connessione `connectionId`, chiamare `findNodes(connectionId, 'Column', undefined, 'column1', 'database1', ['schema1.table1'])`. Vedere anche l'[elenco dei tipi supportati da Azure Data Studio per impostazione predefinita](https://github.com/Microsoft/azuredatastudio/wiki/Object-Explorer-types-supported-by-FindNodes-API) per questa chiamata API.

### <a name="objectexplorernode"></a>ObjectExplorerNode
- `connectionId: string` ID della connessione in cui esiste il nodo

- `nodePath: string` Percorso del nodo, come usato per una chiamata alla funzione `getNode`.

- `nodeType: string` Stringa che rappresenta il tipo del nodo

- `nodeSubType: string` Stringa che rappresenta il sottotipo del nodo

- `nodeStatus: string` Stringa che rappresenta lo stato del nodo

- `label: string` Etichetta del nodo come appare in Esplora oggetti

- `isLeaf: boolean` Indica se il nodo è un nodo foglia e pertanto non ha elementi figlio

- `metadata: azdata.ObjectMetadata` Metadati che descrivono l'oggetto rappresentato da questo nodo

- `errorMessage: string` Messaggio visualizzato se il nodo è in stato di errore

- `isExpanded(): Thenable<boolean>` Indica se il nodo è attualmente espanso in Esplora oggetti

- `setExpandedState(expandedState: vscode.TreeItemCollapsibleState): Thenable<void>` Impostare se il nodo è espanso o compresso. Se lo stato è impostato su None, il nodo non verrà modificato.

- `setSelected(selected: boolean, clearOtherSelections?: boolean): Thenable<void>` Impostare se il nodo è selezionato. Se `clearOtherSelections` è true, deselezionare eventuali altre selezioni quando si effettua la nuova selezione. Se è false, lasciare le selezioni esistenti. `clearOtherSelections` viene impostato su true per impostazione predefinita quando `selected` è true e su false quando `selected` è false.

- `getChildren(): Thenable<azdata.objectexplorer.ObjectExplorerNode[]>` Ottiene tutti i nodi figlio del nodo. Restituisce un elenco vuoto se non sono presenti elementi figlio.

- `getParent(): Thenable<azdata.objectexplorer.ObjectExplorerNode>` Ottiene il nodo padre di questo nodo. Restituisce undefined se non esiste alcun nodo padre.

### <a name="example-code"></a>Codice di esempio

```cs
private async interactWithOENode(selectedNode: azdata.objectexplorer.ObjectExplorerNode): Promise<void> {
    let choices = ['Expand', 'Collapse', 'Select', 'Select (multi)', 'Deselect', 'Deselect (multi)'];
    if (selectedNode.isLeaf) {
        choices[0] += ' (is leaf)';
        choices[1] += ' (is leaf)';
    } else {
        let expanded = await selectedNode.isExpanded();
        if (expanded) {
            choices[0] += ' (is expanded)';
        } else {
            choices[1] += ' (is collapsed)';
        }
    }
    let parent = await selectedNode.getParent();
    if (parent) {
        choices.push('Get Parent');
    }
    let children = await selectedNode.getChildren();
    children.forEach(child => choices.push(child.label));
    let choice = await vscode.window.showQuickPick(choices);
    let nextNode: azdata.objectexplorer.ObjectExplorerNode = undefined;
    if (choice === choices[0]) {
        selectedNode.setExpandedState(vscode.TreeItemCollapsibleState.Expanded);
    } else if (choice === choices[1]) {
        selectedNode.setExpandedState(vscode.TreeItemCollapsibleState.Collapsed);
    } else if (choice === choices[2]) {
        selectedNode.setSelected(true);
    } else if (choice === choices[3]) {
        selectedNode.setSelected(true, false);
    } else if (choice === choices[4]) {
        selectedNode.setSelected(false);
    } else if (choice === choices[5]) {
        selectedNode.setSelected(false, true);
    } else if (choice === 'Get Parent') {
        nextNode = parent;
    } else {
        let childNode = children.find(child => child.label === choice);
        nextNode = childNode;
    }
    if (nextNode) {
        let updatedNode = await azdata.objectexplorer.getNode(nextNode.connectionId, nextNode.nodePath);
        this.interactWithOENode(updatedNode);
    }
}

vscode.commands.registerCommand('mssql.objectexplorer.interact', () => {
    azdata.objectexplorer.getActiveConnectionNodes().then(activeConnections => {
        vscode.window.showQuickPick(activeConnections.map(connection => connection.label + ' ' + connection.connectionId)).then(selection => {
            let selectedNode = activeConnections.find(connection => connection.label + ' ' + connection.connectionId === selection);
            this.interactWithOENode(selectedNode);
        });
    });
});
```

## <a name="proposed-apis"></a>API proposte

Abbiamo aggiunto un elenco di API proposte per consentire alle estensioni di visualizzare l'interfaccia utente personalizzata in finestre di dialogo, procedure guidate e schede di documenti, tra le altre funzionalità. Per altre informazioni, vedere il [file dei tipi di API proposte](https://github.com/Microsoft/azuredatastudio/blob/main/src/sql/azdata.proposed.d.ts), ma tenere presente che queste API sono soggette a modifica in qualsiasi momento. Esempi di come usare alcune di queste API sono disponibili nell'[estensione di esempio "sqlservices"](https://github.com/Microsoft/azuredatastudio/tree/main/samples/sqlservices).



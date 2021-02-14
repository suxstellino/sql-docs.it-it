---
title: Configurare un'istanza del cluster di failover tramite archiviazione NFS - SQL Server in Linux
description: Informazioni su come configurare un'istanza del cluster di failover usando l'archiviazione NFS per SQL Server in Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 90240b383e19fefd254cd5f28b6d113b40b20d98
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346315"
---
# <a name="configure-failover-cluster-instance---nfs---sql-server-on-linux"></a>Configurare un'istanza del cluster di failover - NFS - SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo illustra come configurare la risorsa di archiviazione NFS per un'istanza del cluster di failover in Linux. 

Il file system di rete (NFS) è un metodo comune per condividere dischi in ambito Linux, ma non in Windows. Analogamente a iSCSI, è possibile configurare NFS in un server o in qualche appliance o unità di archiviazione, purché soddisfi i requisiti di archiviazione per SQL Server.

## <a name="important-nfs-server-information"></a>Informazioni importanti sul server NFS

L'origine che ospita NFS (un server Linux o altro) deve usare/essere conforme alla versione 4.2 o successiva. Le versioni precedenti non funzionano con SQL Server in Linux.

Quando si configurano le cartelle da condividere nel server NFS, assicurarsi che seguano queste linee guida generali:
- `rw` per assicurarsi che sia possibile leggere e scrivere nella cartella
- `sync` per assicurare operazioni di scrittura garantite nella cartella
- Non usare `no_root_squash` come opzione perché è considerata un rischio per la sicurezza
- Verificare che alla cartella siano applicati i diritti completi (777)

Assicurarsi che vengano applicati gli standard di sicurezza per l'accesso. Quando si configura la cartella, verificare che solo i server che partecipano all'istanza del cluster di failover possano visualizzare la cartella NFS. Di seguito è riportato un esempio di /etc/exports modificato in una soluzione NFS basata su Linux, in cui la cartella è limitata a FCIN1 e FCIN2.

![Screenshot di un esempio di /etc/exports modificato in una soluzione NFS basata su Linux, in cui la cartella è limitata a FCIN1 e FCIN2.][1]

## <a name="instructions"></a>Istruzioni

1. Scegliere uno dei server che parteciperanno alla configurazione dell'istanza del cluster di failover. Non è rilevante quale. 

2. Verificare che il server riesca a visualizzare i montaggi sul server NFS.

    ```bash
    sudo showmount -e <IPAddressOfNFSServer>
    ```

    \<IPAddressOfNFSServer> è l'indirizzo IP del server NFS da usare.

3. Per i database di sistema o qualsiasi altro elemento archiviato nel percorso dati predefinito, seguire questa procedura. In caso contrario, andare al passaggio 4.
 
   * Verificare che SQL Server venga arrestato nel server su cui si sta lavorando.

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```
   * Eseguire il comando per operare come utente con privilegi avanzati. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

    ```bash
    sudo -i
    ```

   * Eseguire il comando per operare come utente mssql. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

    ```bash
    su mssql
    ```

   * Creare una directory temporanea per archiviare i file di dati e di log di SQL Server. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

    ```bash
    mkdir <TempDir>
    ```

    \<TempDir> è il nome della cartella. L'esempio seguente crea una cartella denominata /var/opt/mssql/tmp.

    ```bash
    mkdir /var/opt/mssql/tmp
    ```

   * Copiare i file di dati e di log di SQL Server nella directory temporanea. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.
    
    ```bash
    cp /var/opt/mssql/data/* <TempDir>
    ```

    \<TempDir> è il nome della cartella del passaggio precedente.

   * Verificare che i file si trovino nella directory.

    ```bash
    ls TempDir
    ```

    \<TempDir> è il nome della cartella del passaggio d.

   * Eliminare i file dalla directory di dati di SQL Server esistente. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

    ```bash
    rm - f /var/opt/mssql/data/*
    ```

   * Verificare che i file siano stati eliminati. 

    ```bash
    ls /var/opt/mssql/data
    ```
    
   * Digitare exit per tornare all'utente ROOT.

   * Montare la condivisione NFS nella cartella di dati di SQL Server. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

    ```bash
    mount -t nfs4 <IPAddressOfNFSServer>:<FolderOnNFSServer> /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

    \<IPAddressOfNFSServer> è l'indirizzo IP del server NFS da usare 

    \<FolderOnNFSServer> è il nome della condivisione NFS. La sintassi di esempio seguente corrisponde alle informazioni NFS del passaggio 2.

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci1 /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

   * Verificare che il montaggio abbia avuto esito positivo eseguendo mount senza alcuna opzione.

    ```bash
    mount
    ```

    ![Screenshot del comando mount e della risposta al comando senza opzioni.][2]

   * Eseguire il comando per operare come utente mssql. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

    ```bash
    su mssql
    ```

   * Copiare i file dalla directory temporanea /var/opt/mssql/data. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

    ```bash
    cp /var/opt/mssql/tmp/* /var/opt/mssqldata
    ```

   * Verificare che i file siano presenti.

    ```bash
    ls /var/opt/mssql/data
    ```

   * Immettere exit per non operare come mssql 
    
   * Immettere exit per non operare come root

   * Avviare SQL Server. Se tutti gli elementi sono stati copiati correttamente e la sicurezza è applicata correttamente, SQL Server deve risultare avviato.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    ```
    
   * Creare un database per verificare che la sicurezza sia configurata correttamente. L'esempio seguente illustra come eseguire questa operazione tramite Transact-SQL, ma è possibile eseguirla anche tramite SSMS.
 
    ![CreateTestdatabase][3]

   * Arrestare SQL Server e verificare che l'operazione sia stata eseguita.

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```

   * Se non si ha intenzione di creare altri montaggi NFS, smontare la condivisione. In caso contrario, non smontarla.

    ```bash
    sudo umount <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn>
    ```

    \<IPAddressOfNFSServer> è l'indirizzo IP del server NFS da usare

    \<FolderOnNFSServer> è il nome della condivisione NFS

    \<FolderMountedIn> è la cartella creata nel passaggio precedente. 

4. Per elementi diversi dai database di sistema, ad esempio i database utente o i backup, seguire questa procedura. Se si usa solo il percorso predefinito, andare al passaggio 5.

   * Eseguire il comando per operare come utente con privilegi avanzati. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

    ```bash
    sudo -i
    ```

   * Creare una cartella che verrà usata da SQL Server. 

    ```bash
    mkdir <FolderName>
    ```

    \<FolderName> è il nome della cartella. Se non si trova nella posizione corretta, è necessario specificare il percorso completo della cartella. L'esempio seguente crea una cartella denominata /var/opt/mssql/userdata.

    ```bash
    mkdir /var/opt/mssql/userdata
    ```

   * Montare la condivisione NFS nella cartella creata nel passaggio precedente. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

    ```bash
    Mount -t nfs4 <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn> -o nfsvers=4.2,timeo=14,intr
    ```

    \<IPAddressOfNFSServer> è l'indirizzo IP del server NFS da usare

    \<FolderOnNFSServer> è il nome della condivisione NFS

    \<FolderToMountIn> è la cartella creata nel passaggio precedente. Di seguito è riportato un esempio. 

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci2 /var/opt/mssql/userdata -o nfsvers=4.2,timeo=14,intr
    ```

   * Verificare che il montaggio abbia avuto esito positivo eseguendo mount senza alcuna opzione.
  
   * Digitare exit per non operare più come utente con privilegi avanzati.

   * Per eseguire il test, creare un database in tale cartella. L'esempio seguente usa sqlcmd per creare un database, cambiare il contesto e verificare che i file esistano a livello di sistema operativo e quindi elimina il percorso temporaneo. È possibile usare SSMS.

    ![Screenshot del comando sqlcmd e della risposta al comando.][4]
 
   * Smontare la condivisione 

    ```bash
    sudo umount <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn>
    ```

    \<IPAddressOfNFSServer> è l'indirizzo IP del server NFS da usare
    
    \<FolderOnNFSServer> è il nome della condivisione NFS

    \<FolderMountedIn> è la cartella creata nel passaggio precedente. Di seguito è riportato un esempio. 
 
5. Ripetere la procedura sugli altri nodi.


## <a name="next-steps"></a>Passaggi successivi

[Configurare un'istanza del cluster di failover - SQL Server in Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/05-nfsacl.png
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/10-mountnoswitches.png
[3]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/20-createtestdatabase.png
[4]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/15-createtestdatabase.png

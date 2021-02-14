---
title: Configurare un'istanza del cluster di failover tramite archiviazione SMB - SQL Server in Linux
description: Informazioni su come configurare un'istanza del cluster di failover usando l'archiviazione SMB per SQL Server in Linux.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 6cc84164c2829d2cf6d5e66f40fd13921fdb105b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346323"
---
# <a name="configure-failover-cluster-instance---smb---sql-server-on-linux"></a>Configurare un'istanza del cluster di failover - SMB - SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo illustra come configurare la risorsa di archiviazione SMB per un'istanza del cluster di failover in Linux. 
 
Nel mondo non Windows, SMB è spesso indicato come condivisione CIFS (Common Internet File System) e viene implementato tramite Samba. Nel mondo Windows, l'accesso a una condivisione SMB viene eseguito in questo modo: \\NOMESERVER\NOMECONDIVISIONE. Per le installazioni di SQL Server basate su Linux, la condivisione SMB deve essere montata come cartella.

## <a name="important-source-and-server-information"></a>Informazioni importanti sul server e sull'origine

Ecco alcuni suggerimenti e diverse note per l'uso corretto di SMB:
- La condivisione SMB può trovarsi in Windows, in Linux o anche in un'appliance, purché usi SMB 3.0 o versione successiva. Per altre informazioni su Samba e SMB 3.0 e per verificare se l'implementazione di Samba in uso è conforme a SMB 3.0, vedere [SMB 3.0](https://wiki.samba.org/index.php/Samba3/SMB2#SMB_3.0).
- La condivisione SMB deve essere a disponibilità elevata.
- Per la condivisione SMB è necessario impostare correttamente la sicurezza. Di seguito è riportato un esempio da /etc/samba/smb.conf, dove SQLData1 è il nome della condivisione.

![Screenshot che mostra che SQLData1 è il nome della condivisione.][1]

## <a name="instructions"></a>Istruzioni

1. Scegliere uno dei server che parteciperanno alla configurazione dell'istanza del cluster di failover. Non è rilevante quale.
   
1. Ottenere informazioni sull'utente mssql.
   
   ```bash
    sudo id mssql
   ```
   
   Prendere nota di UID, GID e gruppi. 
   
1. Eseguire `sudo smbclient -L //NameOrIP/ShareName -U User`.
   
   \<NameOrIP> è il nome DNS o l'indirizzo IP del server che ospita la condivisione SMB.
   
   \<ShareName> è il nome della condivisione SMB. 
   
1. Per i database di sistema o per qualsiasi altro elemento archiviato nel percorso dati predefinito, seguire questa procedura. In caso contrario, andare al passaggio 5. 
   
   1. Verificare che SQL Server venga arrestato nel server su cui si sta lavorando.
      ```bash
      sudo systemctl stop mssql-server
      sudo systemctl status mssql-server
      ```
      
   1. Eseguire il comando per operare come utente con privilegi avanzati. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.
      
      ```bash
      sudo -i
      ```
      
   1. Eseguire il comando per operare come utente mssql. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.
      
      ```bash
      su mssql
      ```
      
   1. Creare una directory temporanea per archiviare i file di dati e di log di SQL Server. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.
      
      ```bash
      mkdir <TempDir>
      ```
      
      \<TempDir> è il nome della cartella. L'esempio seguente crea una cartella denominata /var/opt/mssql/tmp.
      
      ```bash
      mkdir /var/opt/mssql/tmp
      ```
      
   1. Copiare i file di dati e di log di SQL Server nella directory temporanea. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.
      
      ```bash
      cp /var/opt/mssql/data/* <TempDir>
      ```
      
      \<TempDir> è il nome della cartella del passaggio precedente.
      
   1. Verificare che i file si trovino nella directory.
      
      ```bash
      ls <TempDir>
      ```
      
      \<TempDir> è il nome della cartella del passaggio d.
      
   1. Eliminare i file dalla directory di dati di SQL Server esistente. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.
      
      ```bash
      rm - f /var/opt/mssql/data/*
      ```
      
   1. Verificare che i file siano stati eliminati. 
      
      ```bash
      ls /var/opt/mssql/data
      ```
      
   1. Digitare exit per tornare all'utente ROOT.
      
   1. Montare la condivisione SMB nella cartella di dati di SQL Server. Se l'operazione ha esito positivo, non si riceverà alcuna conferma. Questo esempio illustra la sintassi per la connessione a una condivisione SMB 3.0 basata su Windows Server.
      
      ```bash
      Mount -t cifs //<ServerName>/<ShareName> /var/opt/mssql/data -o vers=3.0,username=<UserName>,password=<Password>,domain=<domain>,uid=<mssqlUID>,gid=<mssqlGID>,file_mode=0777,dir_mode=0777
      ```
      
      \<ServerName> è il nome del server con la condivisione SMB
      
      \<ShareName> è il nome della condivisione
      
      \<UserName> è il nome dell'utente con cui accedere alla condivisione
      
      \<Password> è la password dell'utente
      
      \<domain> è il nome di Active Directory
      
      \<mssqlUID> è l'UID dell'utente mssql 
      
      \<mssqlGID> è il GID dell'utente mssql
      
   1. Verificare che il montaggio abbia avuto esito positivo eseguendo mount senza alcuna opzione.
      
      ```bash
      mount
      ```
      
   1. Eseguire il comando per operare come utente mssql. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.
      
      ```bash
      su mssql
      ```
      
   1. Copiare i file dalla directory temporanea /var/opt/mssql/data. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.
      
      ```bash
      cp /var/opt/mssql/tmp/* /var/opt/mssql/data
      ```
      
   1. Verificare che i file siano presenti.
      
      ```bash
      ls /var/opt/mssql/data
      ```
      
   1. Immettere exit per non operare come mssql 
      
   1. Immettere exit per non operare come root
   
   1. Avviare SQL Server. Se tutti gli elementi sono stati copiati correttamente e la sicurezza è applicata correttamente, SQL Server deve risultare avviato.
      
      ```bash
      sudo systemctl start mssql-server
      sudo systemctl status mssql-server
      ```
      
   1. Per eseguire altri test e assicurarsi che le autorizzazioni siano appropriate, creare un database. L'esempio seguente usa Transact-SQL ma è possibile usare SSMS.
      
      ![10_testcreatedb][2] 
      
   1. Arrestare SQL Server e verificare che l'operazione sia stata eseguita. Se si intende aggiungere o testare altri dischi, arrestare SQL Server solo dopo che questi sono stati aggiunti e testati.
      
      ```bash
      sudo systemctl stop mssql-server
      sudo systemctl status mssql-server
      ```
      
   1. Solo se queste operazioni sono state completate, smontare la condivisione. In caso contrario, attendere il completamento dell'aggiunta o del test di tutti i dischi aggiuntivi.
      
      ```bash
      sudo umount //<IPAddressorServerName>/<ShareName /<FolderMountedIn>
      ```
      
      \<IPAddressOrServerName> è l'indirizzo IP o il nome dell'host SMB
      
      \<ShareName> è il nome della condivisione
      
      \<FolderMountedIn> è il nome della cartella in cui SMB è montato
      
5. Per elementi diversi dai database di sistema, ad esempio i database utente o i backup, seguire questa procedura. Se si usa solo il percorso predefinito, andare al passaggio 14.
   
   1. Eseguire il comando per operare come utente con privilegi avanzati. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.
      
      ```bash
      sudo -i
      ```
      
   1. Creare una cartella che verrà usata da SQL Server. 
      
      ```bash
      mkdir <FolderName>
      ```
      
      \<FolderName> è il nome della cartella. Se non si trova nella posizione corretta, è necessario specificare il percorso completo della cartella. L'esempio seguente crea una cartella denominata /var/opt/mssql/userdata.
      
      ```bash
      mkdir /var/opt/mssql/userdata
      ```
      
   1. Montare la condivisione SMB nella cartella di dati di SQL Server. Se l'operazione ha esito positivo, non si riceverà alcuna conferma. Questo esempio illustra la sintassi per la connessione a una condivisione SMB 3.0 basata su Samba.
      
      ```bash
      Mount -t cifs //<ServerName>/<ShareName> <FolderName> -o vers=3.0,username=<UserName>,password=<Password>,uid=<mssqlUID>,gid=<mssqlGID>,file_mode=0777,dir_mode=0777
      ```
      
      \<ServerName> è il nome del server con la condivisione SMB
      
      \<ShareName> è il nome della condivisione
      
      \<FolderName> è il nome della cartella creata nell'ultimo passaggio  
      
      \<UserName> è il nome dell'utente con cui accedere alla condivisione
      
      \<Password> è la password dell'utente
      
      \<mssqlUID> è l'UID dell'utente mssql
      
      \<mssqlGID> è il GID dell'utente mssql.
      
   1. Verificare che il montaggio abbia avuto esito positivo eseguendo mount senza alcuna opzione.
   
   1. Digitare exit per non operare più come utente con privilegi avanzati.
   
   1. Per eseguire il test, creare un database in tale cartella. L'esempio seguente usa sqlcmd per creare un database, cambiare il contesto e verificare che i file esistano a livello di sistema operativo e quindi elimina il percorso temporaneo. È possibile usare SSMS.
   
   1. Smontare la condivisione 
      
      ```bash
      sudo umount //<IPAddressorServerName>/<ShareName> /<FolderMountedIn>
      ```
      
      \<IPAddressOrServerName> è l'indirizzo IP o il nome dell'host SMB
      
      \<ShareName> è il nome della condivisione
      
      \<FolderMountedIn> è il nome della cartella in cui SMB è montato.
   
1. Ripetere la procedura sugli altri nodi.

A questo punto è possibile configurare l'istanza del cluster di failover.

## <a name="next-steps"></a>Passaggi successivi

[Configurare un'istanza del cluster di failover - SQL Server in Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-smb/05-smbsource.png 
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-smb/10-testcreatedb.png 

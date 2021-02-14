---
title: Configurare una risorsa di archiviazione per un'istanza del cluster di failover iSCSI - SQL Server in Linux
description: Informazioni su come configurare un'istanza del cluster di failover usando iSCSI per SQL Server in Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 06/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: dfd63aa749b6bf158ae5e9dcef06ba032c31ddee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346336"
---
# <a name="configure-failover-cluster-instance---iscsi---sql-server-on-linux"></a>Configurare un'istanza del cluster di failover - iSCSI - SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo illustra come configurare la risorsa di archiviazione iSCSI per un'istanza del cluster di failover in Linux. 

## <a name="configure-iscsi"></a>Configurare iSCSI 
iSCSI usa la rete per presentare i dischi da un server noto come destinazione ad altri server. Per i server che si connettono alla destinazione iSCSI è necessario che sia configurato un iniziatore iSCSI. Ai dischi della destinazione vengono concesse autorizzazioni esplicite, in modo che solo gli iniziatori che devono riuscire ad accedervi possano farlo. La destinazione deve essere a disponibilità elevata e affidabile.

### <a name="important-iscsi-target-information"></a>Informazioni importanti sulla destinazione iSCSI
Anche se in questa sezione non verrà illustrato come configurare una destinazione iSCSI perché questa configurazione dipende dal tipo di origine che si userà, assicurarsi che sia configurata la sicurezza per i dischi che verranno usati dai nodi del cluster.  

Se si usa una destinazione iSCSI basata su Linux, la destinazione non deve mai essere configurata in nessuno dei nodi dell'istanza del cluster di failover. Per quanto riguarda prestazioni e disponibilità, le reti iSCSI devono essere separate da quelle usate dal normale traffico di rete sia nel server di origine che in quello client. Le reti usate per iSCSI dovrebbero essere veloci. Tenere presente che la rete utilizza parte della larghezza di banda del processore. Se quindi si usa un server normale, pianificare di conseguenza.
L'aspetto più importante da verificare nella destinazione è che ai dischi creati siano assegnate le autorizzazioni appropriate, in modo che solo i server che partecipano all'istanza del cluster di failover possano accedervi. Un esempio è quello della destinazione iSCSI Microsoft, illustrato sotto, dove linuxnodes1 è il nome creato e, in questo caso, vengono assegnati gli indirizzi IP dei nodi in modo che possano visualizzare NewFCIDisk1.vhdx.

![Iniziatore][1]

### <a name="instructions"></a>Istruzioni

In questa sezione viene illustrato come configurare un iniziatore iSCSI nei server che fungeranno da nodi per l'istanza del cluster di failover. Le istruzioni dovrebbero essere valide così come sono in RHEL e in Ubuntu.

Per altre informazioni sull'iniziatore iSCSI per le distribuzioni supportate, fare clic sui collegamenti seguenti:
- [Red Hat](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Storage_Administration_Guide/iscsi-api.html)
- [SUSE](https://www.suse.com/documentation/sles11/stor_admin/data/sec_inst_system_iscsi_initiator.html) 
- [Ubuntu](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html)

1.  Scegliere uno dei server che parteciperanno alla configurazione dell'istanza del cluster di failover. Non è rilevante quale. iSCSI deve trovarsi in una rete dedicata, quindi configurare iSCSI in modo che riconosca e usi tale rete. Eseguire `sudo iscsiadm -m iface -I <iSCSIIfaceName> -o new` dove `<iSCSIIfaceName>` è il nome descrittivo o univoco della rete. Nell'esempio seguente viene usato `iSCSINIC`:
   
    ```bash
    sudo iscsiadm -m iface -I iSCSINIC -o new
    ```
    ![Screenshot del comando iface e della risposta al comando.][6]
 
2.  Modificare `/var/lib/iscsi/ifaces/iSCSIIfaceName`. Assicurarsi che i valori seguenti siano completamente compilati:

    - iface.net_ifacename è il nome della scheda di rete, visualizzato nel sistema operativo.
    - iface.hwaddress è l'indirizzo MAC del nome univoco che verrà creato per questa interfaccia.
    - iface.ipaddress
    - iface.subnet_Mask 

    Vedere l'esempio seguente:

    ![Screenshot del file con i valori completamente compilati.][2]

3.  Trovare la destinazione iSCSI.

    ```bash
    sudo iscsiadm -m discovery -t sendtargets -I <iSCSINetName> -p <TargetIPAddress>:<TargetPort>
    ```

     \<iSCSINetName> è il nome descrittivo/univoco della rete, \<TargetIPAddress> è l'indirizzo IP della destinazione iSCSI e \<TargetPort> è la porta della destinazione iSCSI. 

    ![Screenshot del comando discovery e della risposta al comando.][3]

 
4.  Accedere alla destinazione

    ```bash
    sudo iscsiadm -m node -I <iSCSIIfaceName> -p TargetIPAddress -l
    ```

    \<iSCSIIfaceName> è il nome descrittivo/univoco della rete e \<TargetIPAddress> è l'indirizzo IP della destinazione iSCSI.

    ![iSCSITargetLogin][4]

5.  Verificare che sia presente una connessione alla destinazione iSCSI.

    ```bash
    sudo iscsiadm -m session
    ```

    ![iSCSIVerify][5]

 
6.  Controllare i dischi iSCSI collegati

    ```bash
    sudo grep "Attached SCSI" /var/log/messages
    ```
    ![Screenshot del comando grep e della risposta al comando che mostra i dischi SCSI collegati.][7]

7.  Creare un volume fisico sul disco iSCSI.

    ```bash
    sudo pvcreate /dev/<devicename>
    ```

    \<devicename> è il nome del dispositivo del passaggio precedente. 

 
8.  Creare un gruppo di volumi nel disco iSCSI. I dischi assegnati a un singolo gruppo di volumi vengono visualizzati come pool o come raccolta. 

    ```bash
    sudo vgcreate <VolumeGroupName> /dev/devicename
    ```

    \<VolumeGroupName> è il nome del gruppo di volumi e \<devicename> è il nome del dispositivo del passaggio 6. 
 
9.  Creare e verificare il volume logico per il disco.

    ```bash
    sudo lvcreate -Lsize -n <LogicalVolumeName> <VolumeGroupName>
    ```
    
    \<size> è la dimensione del volume da creare e può essere specificata in G (gigabyte), T (terabyte) e così via. \<LogicalVolumeName> è il nome del volume logico e \<VolumeGroupName> è il nome del gruppo di volumi del passaggio precedente. 

    Nell'esempio seguente viene creato un volume da 25 GB.
 
    ![Create25GBVol][10]

10. Eseguire `sudo lvs` per visualizzare il volume LVM creato.
 
11. Formattare il volume logico con un file system supportato. Per EXT4, usare l'esempio seguente:

    ```bash
    sudo mkfs.ext4 /dev/<VolumeGroupName>/<LogicalVolumeName>
    ```

    \<VolumeGroupName> è il nome del gruppo di volumi del passaggio precedente. \<LogicalVolumeName> è il nome del volume logico del passaggio precedente.  

12. Per i database di sistema o qualsiasi altro elemento archiviato nel percorso dati predefinito, seguire questa procedura. In caso contrario, andare al passaggio 13.

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
    
        \<TempDir> è il nome della cartella. L'esempio seguente crea una cartella denominata /var/opt/mssql/TempDir.

        ```bash
        mkdir /var/opt/mssql/TempDir
        ```
        
   * Copiare i file di dati e di log di SQL Server nella directory temporanea. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

        ```bash
        cp /var/opt/mssql/data/* <TempDir>
        ```
    
        \<TempDir> è il nome della cartella del passaggio precedente.
    
   * Verificare che i file si trovino nella directory.

        ```bash
        ls \<TempDir>
        ```
 
        \<TempDir> è il nome della cartella del passaggio d.

   * Eliminare i file dalla directory di dati di SQL Server esistente. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

        ```bash
        rm - f /var/opt/mssql/data/*
        ```
    
   * Verificare che i file siano stati eliminati. Nell'immagine seguente viene illustrato un esempio dell'intera sequenza da c a h.

        ```bash
        ls /var/opt/mssql/data
        ```
    
        ![Screenshot del comando ls e della risposta al comando.][8]

   * Digitare `exit` per tornare all'utente ROOT.

   * Montare il volume logico iSCSI nella cartella di dati di SQL Server. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

        ```bash
        mount /dev/<VolumeGroupName>/<LogicalVolumeName> /var/opt/mssql/data
        ```

        \<VolumeGroupName> è il nome del gruppo di volumi e \<LogicalVolumeName> è il nome del volume logico creato. La sintassi di esempio seguente corrisponde al gruppo di volumi e al volume logico del comando precedente.

        ```bash
        mount /dev/FCIDataVG1/FCIDataLV1 /var/opt/mssql/data
        ```

   * Modificare il proprietario del montaggio impostandolo su mssql. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

        ```bash
        chown mssql /var/opt/mssql/data
        ```

   * Modificare la proprietà del gruppo del montaggio impostandolo su mssql. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

        ```bash
        chgrp mssql /var/opt/mssql/data
        ```

   * Eseguire il comando per operare come utente mssql. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

        ```bash
        su mssql
        ``` 

   * Copiare i file dalla directory temporanea /var/opt/mssql/data. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

        ```bash
        cp /var/opt/mssql/TempDir/* /var/opt/mssql/data
        ``` 
    
   * Verificare che i file siano presenti.

        ```bash
        ls /var/opt/mssql/data
        ``` 

   *    Immettere `exit` per non operare come mssql.

   *    Immettere `exit` per non operare come root.

   *    Avviare SQL Server. Se tutti gli elementi sono stati copiati correttamente e la sicurezza è applicata correttamente, SQL Server deve risultare avviato.

        ```bash
        sudo systemctl start mssql-server
        sudo systemctl status mssql-server
        ``` 

   *    Arrestare SQL Server e verificare che l'operazione sia stata eseguita.

        ```bash
        sudo systemctl stop mssql-server
        sudo systemctl status mssql-server
        ``` 

13. Per elementi diversi dai database di sistema, ad esempio i database utente o i backup, seguire questa procedura. Se si usa solo il percorso predefinito, andare al passaggio 14.

   *    Eseguire il comando per operare come utente con privilegi avanzati. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

        ```bash
        sudo -i
        ```

   *    Creare una cartella che verrà usata da SQL Server. 

        ```bash
        mkdir <FolderName>
        ```

        \<FolderName> è il nome della cartella. Se non si trova nella posizione corretta, è necessario specificare il percorso completo della cartella. L'esempio seguente crea una cartella denominata /var/opt/mssql/userdata.

        ```bash
        mkdir /var/opt/mssql/userdata
        ```

   *    Montare il volume logico iSCSI nella cartella creata nel passaggio precedente. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

        ```bash
        mount /dev/<VolumeGroupName>/<LogicalVolumeName> <FolderName>
        ```

        \<VolumeGroupName> è il nome del gruppo di volumi, \<LogicalVolumeName> è il nome del volume logico creato e \<FolderName> è il nome della cartella. Di seguito è riportata la sintassi di esempio.

        ```bash
        mount /dev/FCIDataVG2/FCIDataLV2 /var/opt/mssql/userdata 
        ```

   *    Modificare la proprietà della cartella creata impostandola su mssql. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

        ```bash
        chown mssql <FolderName>
        ```

        \<FolderName> è il nome della cartella creata. Di seguito è illustrato un esempio.

        ```bash
        chown mssql /var/opt/mssql/userdata
        ```

   *    Modificare il gruppo della cartella creata impostandolo su mssql. Se l'operazione ha esito positivo, non si riceverà alcuna conferma.

        ```bash
        chown mssql <FolderName>
        ```

        \<FolderName> è il nome della cartella creata. Di seguito è illustrato un esempio.

        ```bash
        chown mssql /var/opt/mssql/userdata
        ```

   *    Digitare `exit` per non operare più come utente con privilegi avanzati.

   *    Per eseguire il test, creare un database in tale cartella. L'esempio illustrato di seguito usa sqlcmd per creare un database, cambiare il contesto e verificare che i file esistano a livello di sistema operativo e quindi elimina il percorso temporaneo. È possibile usare SSMS.
  
        ![Screenshot del comando sqlcmd e della risposta al comando.][9]

   *    Smontare la condivisione 

        ```bash
        sudo umount /dev/<VolumeGroupName>/<LogicalVolumeName> <FolderName>
        ```

        \<VolumeGroupName> è il nome del gruppo di volumi, \<LogicalVolumeName> è il nome del volume logico creato e \<FolderName> è il nome della cartella. Di seguito è riportata la sintassi di esempio.

        ```bash
        sudo umount /dev/FCIDataVG2/FCIDataLV2 /var/opt/mssql/userdata 
        ```

14. Configurare il server in modo che solo Pacemaker possa attivare il gruppo di volumi.

    ```bash
    sudo lvmconf --enable-halvm --services -startstopservices
    ```

15. Generare un elenco dei gruppi di volumi nel server. Qualsiasi elemento elencato che non sia il disco iSCSI viene usato dal sistema, ad esempio il disco del sistema operativo.

    ```bash
    sudo vgs
    ```

16. Modificare la sezione di configurazione dell'attivazione del file /etc/lvm/lvm.conf. Configurare la riga seguente:

    ```bash
    volume_list = [ <ListOfVGsNotUsedByPacemaker> ]
    ```

    \<ListOfVGsNotUsedByPacemaker> è l'elenco dei gruppi di volumi dell'output del passaggio 20 che non verranno usati dall'istanza del cluster di failover. Inserirli ognuno tra virgolette e separarli con una virgola. Di seguito è illustrato un esempio.

    ![Screenshot che mostra un esempio di un valore volume_list.][11]

17. All'avvio di Linux, il file system verrà montato. Per assicurarsi che solo Pacemaker possa montare il disco iSCSI, ricompilare l'immagine del file system radice. 

    Eseguire il comando seguente che può richiedere alcuni minuti per il completamento. Se l'operazione ha esito positivo, non viene restituito alcun messaggio.

    ```bash
    sudo dracut -H -f /boot/initramfs-$(uname -r).img $(uname -r)
    ```

18. Riavviare il server.

19. In un altro server che parteciperà all'istanza del cluster di failover eseguire i passaggi da 1 a 6. La destinazione iSCSI verrà presentata a SQL Server. 
 
20. Generare un elenco dei gruppi di volumi nel server. Verrà visualizzato il gruppo di volumi creato in precedenza. 

    ```bash
    sudo vgs
    ``` 

23. Avviare SQL Server e verificare che possa essere avviato in questo server.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    ```

24. Arrestare SQL Server e verificare che l'operazione sia stata eseguita.

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```

25. Ripetere i passaggi da 1 a 6 in tutti gli altri server che parteciperanno all'istanza del cluster di failover.

A questo punto è possibile configurare l'istanza del cluster di failover.

| Distribuzione | Argomento |
| :----------- | :---- |
| Red Hat Enterprise Linux con componente aggiuntivo a disponibilità elevata | [Configurare](sql-server-linux-shared-disk-cluster-configure.md)<br/>[Gestire](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md) |
| SUSE Linux Enterprise Server con componente aggiuntivo a disponibilità elevata | [Configurare](sql-server-linux-shared-disk-cluster-sles-configure.md) |

## <a name="next-steps"></a>Passaggi successivi

[Configurare un'istanza del cluster di failover - SQL Server in Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/05-initiator.png
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/10-iscsiTargetSettings.png
[3]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/15-iSCSITargetResults.png
[4]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/20-iSCSITargetLogin.png
[5]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/25-iSCSIVerify.png
[6]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/7-setiscsinetwork.png
[7]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/30-iSCSIattachedDisks.png
[8]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/45-CopyMove.png
[9]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/50-ExampleCreateSSMS.png
[10]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/40-Create25GBVol.png
[11]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/55-ListOfVGs.png

# Maîtrise de poste - Day 1

## I.Self-footprinting

### 1.Host OS

Avec la commande ```systeminfo```, on peut avoir toutes les infos ci-dessous excepté les informations pour la RAM.

-Nom de la machine 
```
Nom de l’hôte:                              DESKTOP-D729Q32
```

-OS et version 
```
Nom du système d’exploitation:              Microsoft Windows 10 Famille
```

-Architecture processeur (32-bit, 64-bit, ARM, etc)
```
Type du système:                            x64-based PC
```

-Modèle du processeur
```
Processeur(s):                              1 processeur(s) installé(s).
                                            [01] : Intel64 Family 6 Model 142 Stepping 10 GenuineIntel ~1800 MHz
```

-Quantité RAM et modèle de la RAM
```
Manufacturer Banklabel Configuredclockspeed   Capacity
------------ --------- --------------------   --------
Samsung      BANK 0                    2400 4294967296
Samsung      BANK 2                    2400 2147483648
```

Pour trouver ces informations, on entre la commande ```gwmi win32_physicalmemory | Format-Table Manufacturer,Banklabel,Configuredclockspeed,Capacity -autosize```.

### 2.Devices

#### 2.1.Trouver

-Marque du processeur
```
PS C:\Users\lilit> Get-WmiObject Win32_Processor


Caption           : Intel64 Family 6 Model 142 Stepping 10
DeviceID          : CPU0
Manufacturer      : GenuineIntel
MaxClockSpeed     : 1800
Name              : Intel(R) Core(TM) i5-8250U CPU @ 1.60GHz
SocketDesignation : U3E1
```

-identifier le nombre de processeurs, le nombre de coeur
```
PS C:\Users\lilit> echo $env:NUMBER_OF_PROCESSORS
8
```

La commande `Get-PnpDevice` affiche beaucoup d'informations. Il faut donc aller chercher celles qui nous intéresse.

-Marque et modèle du touchpad/trackpad
```
PS C:\Users\lilit> Get-PnpDevice

OK         SoftwareComp... ELAN Touchpad Component  
```

-Marque et modèle des enceintes intégrés
```
PS C:\Users\lilit> Get-PnpDevice

OK         AudioEndpoint   Haut-parleur/Ecouteurs (Realtek(R) Audio)
```

-Marque et modèle du disque dur principale
```
PS C:\Users\lilit> Get-PnpDevice

OK         DiskDrive       SAMSUNG MZNLN256HAJQ-000H1
```

#### 2.2.Disque Dur

-Identifier les différentes partitions de votre/vos disque(s) dur(s)
```
PS C:\Users\lilit> Get-Partition


   DiskPath : \\?\scsi#disk&ven_samsung&prod_mznln256hajq-000#4&22c3b0c9&0&000100#{53f56307-b6bf-11d0-94f2-00a0c91efb8b}

PartitionNumber  DriveLetter Offset                                         Size Type
---------------  ----------- ------                                         ---- ----
1                            1048576                                      260 MB System
2                            273678336                                     16 MB Reserved
3                C           290455552                                 221.26 GB Basic
4                            237870514176                                 980 MB Recovery
5                D           238898118656                               15.98 GB Basic
```

On cherche son nombre processeur et de coeur du disque. 

```
PS C:\Users\lilit> Get-WmiObject -class Win32_processor | ft NumberOfCores,NumberOfLogicalProcessors                                   
NumberOfCores NumberOfLogicalProcessors
------------- -------------------------
            4                         8
```
Mon disque a 4 coeurs et 8 processeurs.


-Déterminer le système de fichier de chaque partition

```
DiskPath : \\?\scsi#disk&ven_samsung&prod_mznln256hajq-000#4&22c3b0c9&0&000100#{53f56307-b6bf-11d0-94f2-00a0c91efb8b}
```

-Expliquer la fonction de chaque partition

**System** : Cette partition contient les fichier de boot de l'ordinateur.

**Reserved** : Cette partition contient les fichier essentiel à l'ordinateur.

**Basic** : Cette partition contient l'OS de l'ordinateur.

**Recovery** : Cette partition contient les fichier et dossier de récupération en cas de problème.


### 3.Network

-Afficher la liste des cartes réseau de votre machine

```
PS C:\Users\lilit> Get-NetAdapter | fl Name, InterfaceIndex


Name           : VirtualBox Host-Only Network #3
InterfaceIndex : 29

Name           : Ethernet 2
InterfaceIndex : 27

Name           : VirtualBox Host-Only Network #5
InterfaceIndex : 25

Name           : Ethernet 3
InterfaceIndex : 23

Name           : VirtualBox Host-Only Network #4
InterfaceIndex : 21

Name           : VirtualBox Host-Only Network
InterfaceIndex : 19

Name           : VirtualBox Host-Only Network #10
InterfaceIndex : 14

Name           : Wi-Fi
InterfaceIndex : 12

Name           : VirtualBox Host-Only Network #9
InterfaceIndex : 11

Name           : VirtualBox Host-Only Network #7
InterfaceIndex : 9

Name           : VirtualBox Host-Only Network #2
InterfaceIndex : 8

Name           : VirtualBox Host-Only Network #6
InterfaceIndex : 6

Name           : VirtualBox Host-Only Network #8
InterfaceIndex : 3
```

Toutes les cartes réseau avec "VirtualBox" dans leurs nom, sont celles générés par VirtualBox lorsque l'on créé un réseau host-only.
La carte réseau avec comme nom "Wi-fi" est la carte utilisé pour le Wi-fi par notre pc.
Les cartes réseaux ethernet sont les cartes réseaux utilisées par nos port ethernet.

-Lister tous les ports TCP et UDP en utilisation
```
PS C:\Users\lilit> netstat -ano

Connexions actives

  Proto  Adresse locale         Adresse distante       État
  TCP    0.0.0.0:80             0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING       1096
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:902            0.0.0.0:0              LISTENING       5436
  TCP    0.0.0.0:912            0.0.0.0:0              LISTENING       5436
  TCP    0.0.0.0:5040           0.0.0.0:0              LISTENING       9924
  TCP    0.0.0.0:5357           0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:8733           0.0.0.0:0              LISTENING       16408
  TCP    0.0.0.0:49664          0.0.0.0:0              LISTENING       916
  TCP    0.0.0.0:49665          0.0.0.0:0              LISTENING       748
  TCP    0.0.0.0:49666          0.0.0.0:0              LISTENING       1592
  TCP    0.0.0.0:49667          0.0.0.0:0              LISTENING       2244
  TCP    0.0.0.0:49668          0.0.0.0:0              LISTENING       2932
  TCP    0.0.0.0:49669          0.0.0.0:0              LISTENING       3804
  TCP    0.0.0.0:49677          0.0.0.0:0              LISTENING       896
  TCP    0.0.0.0:50002          0.0.0.0:0              LISTENING       4572
  TCP    0.0.0.0:50003          0.0.0.0:0              LISTENING       4572
  TCP    10.2.1.3:139           0.0.0.0:0              LISTENING       4
  TCP    10.3.1.1:139           0.0.0.0:0              LISTENING       4
  TCP    10.3.2.1:139           0.0.0.0:0              LISTENING       4
  TCP    10.3.3.1:139           0.0.0.0:0              LISTENING       4
  TCP    10.4.1.1:139           0.0.0.0:0              LISTENING       4
  TCP    10.6.1.1:139           0.0.0.0:0              LISTENING       4
  TCP    10.7.1.1:139           0.0.0.0:0              LISTENING       4
  TCP    10.8.1.1:139           0.0.0.0:0              LISTENING       4
  TCP    127.0.0.1:5354         0.0.0.0:0              LISTENING       4680
  TCP    127.0.0.1:5939         0.0.0.0:0              LISTENING       4920
  TCP    127.0.0.1:6341         0.0.0.0:0              LISTENING       26352
  TCP    127.0.0.1:6342         0.0.0.0:0              LISTENING       26352
  TCP    127.0.0.1:6463         0.0.0.0:0              LISTENING       4400
  TCP    127.0.0.1:11476        127.0.0.1:11477        ESTABLISHED     21844
  TCP    127.0.0.1:11477        127.0.0.1:11476        ESTABLISHED     21844
  TCP    127.0.0.1:11680        127.0.0.1:29818        ESTABLISHED     14672
  TCP    127.0.0.1:11843        127.0.0.1:11844        ESTABLISHED     18756
  TCP    127.0.0.1:11844        127.0.0.1:11843        ESTABLISHED     18756
  TCP    127.0.0.1:12264        127.0.0.1:12265        ESTABLISHED     20824
  TCP    127.0.0.1:12265        127.0.0.1:12264        ESTABLISHED     20824
  TCP    127.0.0.1:12327        127.0.0.1:12328        ESTABLISHED     27632
  TCP    127.0.0.1:12328        127.0.0.1:12327        ESTABLISHED     27632
  TCP    127.0.0.1:12372        127.0.0.1:12373        ESTABLISHED     21472
  TCP    127.0.0.1:12373        127.0.0.1:12372        ESTABLISHED     21472
  TCP    127.0.0.1:12527        0.0.0.0:0              LISTENING       10836
  TCP    127.0.0.1:15292        0.0.0.0:0              LISTENING       28476
  TCP    127.0.0.1:15393        0.0.0.0:0              LISTENING       28476
  TCP    127.0.0.1:16494        0.0.0.0:0              LISTENING       28476
  TCP    127.0.0.1:29735        127.0.0.1:29736        ESTABLISHED     4748
  TCP    127.0.0.1:29736        127.0.0.1:29735        ESTABLISHED     4748
  TCP    127.0.0.1:29742        127.0.0.1:29743        ESTABLISHED     17772
  TCP    127.0.0.1:29743        127.0.0.1:29742        ESTABLISHED     17772
  TCP    127.0.0.1:29754        127.0.0.1:29755        ESTABLISHED     12152
  TCP    127.0.0.1:29755        127.0.0.1:29754        ESTABLISHED     12152
  TCP    127.0.0.1:29818        0.0.0.0:0              LISTENING       10836
  TCP    127.0.0.1:29818        127.0.0.1:11680        ESTABLISHED     10836
  TCP    127.0.0.1:29844        0.0.0.0:0              LISTENING       10836
  TCP    127.0.0.1:30454        127.0.0.1:30455        ESTABLISHED     11620
  TCP    127.0.0.1:30455        127.0.0.1:30454        ESTABLISHED     11620
  TCP    127.0.0.1:45623        0.0.0.0:0              LISTENING       10836
  TCP    127.0.0.1:63225        0.0.0.0:0              LISTENING       19620
  TCP    169.254.111.188:139    0.0.0.0:0              LISTENING       4
  TCP    192.168.1.8:139        0.0.0.0:0              LISTENING       4
  TCP    192.168.1.8:10621      88.221.148.12:443      CLOSE_WAIT      21936
  TCP    192.168.1.8:11587      13.107.3.128:443       ESTABLISHED     908
  TCP    192.168.1.8:11597      52.114.77.33:443       ESTABLISHED     908
  TCP    192.168.1.8:11656      54.149.145.192:443     ESTABLISHED     4748
  TCP    192.168.1.8:11683      162.159.136.234:443    ESTABLISHED     25312
  TCP    192.168.1.8:11696      40.67.254.36:443       ESTABLISHED     4852
  TCP    192.168.1.8:11732      13.88.181.35:443       ESTABLISHED     4572
  TCP    192.168.1.8:11734      63.33.159.31:443       ESTABLISHED     14116
  TCP    192.168.1.8:11737      35.175.25.57:443       ESTABLISHED     14116
  TCP    192.168.1.8:12248      162.159.130.235:443    ESTABLISHED     25312
  TCP    192.168.1.8:12401      162.159.133.233:443    ESTABLISHED     25312
  TCP    192.168.1.8:12419      143.204.222.96:443     ESTABLISHED     4748
  TCP    192.168.1.8:12434      2.18.245.83:80         ESTABLISHED     4572
  TCP    192.168.1.8:12438      23.202.119.118:80      ESTABLISHED     4572
  TCP    192.168.56.2:139       0.0.0.0:0              LISTENING       4
  TCP    192.168.81.1:139       0.0.0.0:0              LISTENING       4
  TCP    192.168.84.1:139       0.0.0.0:0              LISTENING       4
  TCP    [::]:80                [::]:0                 LISTENING       4
  TCP    [::]:135               [::]:0                 LISTENING       1096
  TCP    [::]:445               [::]:0                 LISTENING       4
  TCP    [::]:5357              [::]:0                 LISTENING       4
  TCP    [::]:8733              [::]:0                 LISTENING       16408
  TCP    [::]:49664             [::]:0                 LISTENING       916
  TCP    [::]:49665             [::]:0                 LISTENING       748
  TCP    [::]:49666             [::]:0                 LISTENING       1592
  TCP    [::]:49667             [::]:0                 LISTENING       2244
  TCP    [::]:49668             [::]:0                 LISTENING       2932
  TCP    [::]:49669             [::]:0                 LISTENING       3804
  TCP    [::]:49677             [::]:0                 LISTENING       896
  TCP    [::]:50003             [::]:0                 LISTENING       4572
  TCP    [::1]:49925            [::]:0                 LISTENING       16680
  TCP    [2001:861:4481:38f0:8442:7774:9ee1:5b8b]:11599  [2606:4700::6812:18f3]:80  ESTABLISHED     908
  TCP    [2001:861:4481:38f0:8442:7774:9ee1:5b8b]:11812  [2a05:f500:10:101::b93f:9101]:443  CLOSE_WAIT      21388
  TCP    [2001:861:4481:38f0:8442:7774:9ee1:5b8b]:12254  [2406:da14:88d:a100:3a47:3832:1889:bc3f]:443  ESTABLISHED     4748
  TCP    [2001:861:4481:38f0:8442:7774:9ee1:5b8b]:12421  [2406:da14:88d:a100:3a47:3832:1889:bc3f]:443  ESTABLISHED     4748
  TCP    [2001:861:4481:38f0:8442:7774:9ee1:5b8b]:12427  [2a00:1450:4007:80c::200a]:443  ESTABLISHED     4748
  TCP    [2001:861:4481:38f0:8442:7774:9ee1:5b8b]:12433  [2a00:1450:4007:805::2003]:80  ESTABLISHED     4572
  TCP    [2001:861:4481:38f0:8442:7774:9ee1:5b8b]:12435  [2a00:1450:4007:805::200e]:80  ESTABLISHED     4572
  UDP    0.0.0.0:500            *:*                                    4300
  UDP    0.0.0.0:3702           *:*                                    18440
  UDP    0.0.0.0:3702           *:*                                    18440
  UDP    0.0.0.0:3702           *:*                                    5424
  UDP    0.0.0.0:3702           *:*                                    5424
  UDP    0.0.0.0:4500           *:*                                    4300
  UDP    0.0.0.0:5050           *:*                                    9924
  UDP    0.0.0.0:5353           *:*                                    3128
  UDP    0.0.0.0:5355           *:*                                    3128
  UDP    0.0.0.0:58024          *:*                                    4680
  UDP    0.0.0.0:59084          *:*                                    18440
  UDP    0.0.0.0:59652          *:*                                    4920
  UDP    0.0.0.0:61993          *:*                                    5424
  UDP    0.0.0.0:63008          *:*                                    908
  UDP    0.0.0.0:63080          *:*                                    4400
  UDP    10.2.1.3:137           *:*                                    4
  UDP    10.2.1.3:138           *:*                                    4
  UDP    10.2.1.3:1900          *:*                                    6664
  UDP    10.2.1.3:2177          *:*                                    16048
  UDP    10.2.1.3:5353          *:*                                    4920
  UDP    10.2.1.3:5353          *:*                                    4680
  UDP    10.2.1.3:61579         *:*                                    6664
  UDP    10.3.1.1:137           *:*                                    4
  UDP    10.3.1.1:138           *:*                                    4
  UDP    10.3.1.1:1900          *:*                                    6664
  UDP    10.3.1.1:2177          *:*                                    16048
  UDP    10.3.1.1:5353          *:*                                    4920
  UDP    10.3.1.1:5353          *:*                                    4680
  UDP    10.3.1.1:61578         *:*                                    6664
  UDP    10.3.2.1:137           *:*                                    4
  UDP    10.3.2.1:138           *:*                                    4
  UDP    10.3.2.1:1900          *:*                                    6664
  UDP    10.3.2.1:2177          *:*                                    16048
  UDP    10.3.2.1:5353          *:*                                    4920
  UDP    10.3.2.1:5353          *:*                                    4680
  UDP    10.3.2.1:61581         *:*                                    6664
  UDP    10.3.3.1:137           *:*                                    4
  UDP    10.3.3.1:138           *:*                                    4
  UDP    10.3.3.1:1900          *:*                                    6664
  UDP    10.3.3.1:2177          *:*                                    16048
  UDP    10.3.3.1:5353          *:*                                    4680
  UDP    10.3.3.1:5353          *:*                                    4920
  UDP    10.3.3.1:61582         *:*                                    6664
  UDP    10.4.1.1:137           *:*                                    4
  UDP    10.4.1.1:138           *:*                                    4
  UDP    10.4.1.1:1900          *:*                                    6664
  UDP    10.4.1.1:2177          *:*                                    16048
  UDP    10.4.1.1:5353          *:*                                    4920
  UDP    10.4.1.1:5353          *:*                                    4680
  UDP    10.4.1.1:61583         *:*                                    6664
  UDP    10.6.1.1:137           *:*                                    4
  UDP    10.6.1.1:138           *:*                                    4
  UDP    10.6.1.1:1900          *:*                                    6664
  UDP    10.6.1.1:2177          *:*                                    16048
  UDP    10.6.1.1:5353          *:*                                    4920
  UDP    10.6.1.1:5353          *:*                                    4680
  UDP    10.6.1.1:61584         *:*                                    6664
  UDP    10.7.1.1:137           *:*                                    4
  UDP    10.7.1.1:138           *:*                                    4
  UDP    10.7.1.1:1900          *:*                                    6664
  UDP    10.7.1.1:2177          *:*                                    16048
  UDP    10.7.1.1:5353          *:*                                    4920
  UDP    10.7.1.1:5353          *:*                                    4680
  UDP    10.7.1.1:61576         *:*                                    6664
  UDP    10.8.1.1:137           *:*                                    4
  UDP    10.8.1.1:138           *:*                                    4
  UDP    10.8.1.1:1900          *:*                                    6664
  UDP    10.8.1.1:2177          *:*                                    16048
  UDP    10.8.1.1:5353          *:*                                    4680
  UDP    10.8.1.1:5353          *:*                                    4920
  UDP    10.8.1.1:61585         *:*                                    6664
  UDP    127.0.0.1:1900         *:*                                    6664
  UDP    127.0.0.1:58026        *:*                                    5376
  UDP    127.0.0.1:61589        *:*                                    6664
  UDP    169.254.111.188:137    *:*                                    4
  UDP    169.254.111.188:138    *:*                                    4
  UDP    169.254.111.188:1900   *:*                                    6664
  UDP    169.254.111.188:2177   *:*                                    16048
  UDP    169.254.111.188:5353   *:*                                    4680
  UDP    169.254.111.188:5353   *:*                                    4920
  UDP    169.254.111.188:61580  *:*                                    6664
  UDP    192.168.1.8:137        *:*                                    4
  UDP    192.168.1.8:138        *:*                                    4
  UDP    192.168.1.8:1900       *:*                                    6664
  UDP    192.168.1.8:2177       *:*                                    16048
  UDP    192.168.1.8:5353       *:*                                    4680
  UDP    192.168.1.8:61588      *:*                                    6664
  UDP    192.168.56.2:137       *:*                                    4
  UDP    192.168.56.2:138       *:*                                    4
  UDP    192.168.56.2:1900      *:*                                    6664
  UDP    192.168.56.2:2177      *:*                                    16048
  UDP    192.168.56.2:5353      *:*                                    4680
  UDP    192.168.56.2:5353      *:*                                    4920
  UDP    192.168.56.2:61577     *:*                                    6664
  UDP    192.168.81.1:137       *:*                                    4
  UDP    192.168.81.1:138       *:*                                    4
  UDP    192.168.81.1:1900      *:*                                    6664
  UDP    192.168.81.1:2177      *:*                                    16048
  UDP    192.168.81.1:5353      *:*                                    4680
  UDP    192.168.81.1:5353      *:*                                    4920
  UDP    192.168.81.1:61587     *:*                                    6664
  UDP    192.168.84.1:137       *:*                                    4
  UDP    192.168.84.1:138       *:*                                    4
  UDP    192.168.84.1:1900      *:*                                    6664
  UDP    192.168.84.1:2177      *:*                                    16048
  UDP    192.168.84.1:5353      *:*                                    4680
  UDP    192.168.84.1:5353      *:*                                    4920
  UDP    192.168.84.1:61586     *:*                                    6664
  UDP    [::]:500               *:*                                    4300
  UDP    [::]:3702              *:*                                    18440
  UDP    [::]:3702              *:*                                    5424
  UDP    [::]:3702              *:*                                    18440
  UDP    [::]:3702              *:*                                    5424
  UDP    [::]:4500              *:*                                    4300
  UDP    [::]:5353              *:*                                    3128
  UDP    [::]:5355              *:*                                    3128
  UDP    [::]:58025             *:*                                    4680
  UDP    [::]:59085             *:*                                    18440
  UDP    [::]:59653             *:*                                    4920
  UDP    [::]:61994             *:*                                    5424
  UDP    [::]:63008             *:*                                    908
  UDP    [::1]:1900             *:*                                    6664
  UDP    [::1]:61575            *:*                                    6664
  UDP    [2001:861:4481:38f0:8442:7774:9ee1:5b8b]:2177  *:*                                    16048
  UDP    [2001:861:4481:38f0:c507:3dc8:4a41:6646]:2177  *:*                                    16048
  UDP    [fe80::3c4b:a2fb:d5d4:6fbc%29]:1900  *:*                                    6664
  UDP    [fe80::3c4b:a2fb:d5d4:6fbc%29]:2177  *:*                                    16048
  UDP    [fe80::3c4b:a2fb:d5d4:6fbc%29]:5353  *:*                                    4920
  UDP    [fe80::3c4b:a2fb:d5d4:6fbc%29]:5353  *:*                                    4680
  UDP    [fe80::3c4b:a2fb:d5d4:6fbc%29]:61566  *:*                                    6664
  UDP    [fe80::3d1a:4fb:4787:b1c4%23]:1900  *:*                                    6664
  UDP    [fe80::3d1a:4fb:4787:b1c4%23]:2177  *:*                                    16048
  UDP    [fe80::3d1a:4fb:4787:b1c4%23]:61573  *:*                                    6664
  UDP    [fe80::603c:b00b:3fb:c388%3]:1900  *:*                                    6664
  UDP    [fe80::603c:b00b:3fb:c388%3]:2177  *:*                                    16048
  UDP    [fe80::603c:b00b:3fb:c388%3]:61570  *:*                                    6664
  UDP    [fe80::6916:f4c:cdb:ec45%14]:1900  *:*                                    6664
  UDP    [fe80::6916:f4c:cdb:ec45%14]:2177  *:*                                    16048
  UDP    [fe80::6916:f4c:cdb:ec45%14]:61571  *:*                                    6664
  UDP    [fe80::908a:4039:2e1e:5935%19]:1900  *:*                                    6664
  UDP    [fe80::908a:4039:2e1e:5935%19]:2177  *:*                                    16048
  UDP    [fe80::908a:4039:2e1e:5935%19]:61563  *:*                                    6664
  UDP    [fe80::9d79:7c8a:a252:ef71%21]:1900  *:*                                    6664
  UDP    [fe80::9d79:7c8a:a252:ef71%21]:2177  *:*                                    16048
  UDP    [fe80::9d79:7c8a:a252:ef71%21]:61564  *:*                                    6664
  UDP    [fe80::9df5:9420:a008:3210%11]:1900  *:*                                    6664
  UDP    [fe80::9df5:9420:a008:3210%11]:2177  *:*                                    16048
  UDP    [fe80::9df5:9420:a008:3210%11]:61562  *:*                                    6664
  UDP    [fe80::b93e:325b:747c:6dd0%6]:1900  *:*                                    6664
  UDP    [fe80::b93e:325b:747c:6dd0%6]:2177  *:*                                    16048
  UDP    [fe80::b93e:325b:747c:6dd0%6]:61568  *:*                                    6664
  UDP    [fe80::c507:3dc8:4a41:6646%12]:1900  *:*                                    6664
  UDP    [fe80::c507:3dc8:4a41:6646%12]:2177  *:*                                    16048
  UDP    [fe80::c507:3dc8:4a41:6646%12]:61574  *:*                                    6664
  UDP    [fe80::dc33:6916:56b1:36a4%8]:1900  *:*                                    6664
  UDP    [fe80::dc33:6916:56b1:36a4%8]:2177  *:*                                    16048
  UDP    [fe80::dc33:6916:56b1:36a4%8]:61565  *:*                                    6664
  UDP    [fe80::f4ee:794d:a080:543a%9]:1900  *:*                                    6664
  UDP    [fe80::f4ee:794d:a080:543a%9]:2177  *:*                                    16048
  UDP    [fe80::f4ee:794d:a080:543a%9]:61569  *:*                                    6664
  UDP    [fe80::f812:fc57:57ab:f627%27]:1900  *:*                                    6664
  UDP    [fe80::f812:fc57:57ab:f627%27]:2177  *:*                                    16048
  UDP    [fe80::f812:fc57:57ab:f627%27]:61572  *:*                                    6664
  UDP    [fe80::fc89:5468:c544:619%25]:1900  *:*                                    6664
  UDP    [fe80::fc89:5468:c544:619%25]:2177  *:*                                    16048
  UDP    [fe80::fc89:5468:c544:619%25]:61567  *:*                                    6664
```

-Déterminer quel programme tourne derrière chacun des ports

```
PS C:\Users\lilit> Get-Process | Where-Object {$_.mainWindowTitle} | Format-Table Id, Name, mainWindowtitle -AutoSize

   Id Name                                                           MainWindowTitle
   -- ----                                                           ---------------
11924 ApplicationFrameHost                                           Paramètres
 4748 firefox                                                        Maîtrise de poste - Day 1 - HackMD - Mozilla Firefox
 5200 powershell                                                     Windows PowerShell
25060 SystemSettings                                                 CN=Microsoft Windows, O=Microsoft Corporation, L=Redmond, S=Wa...
24228 WindowsInternal.ComposableShell.Experiences.TextInput.InputApp Microsoft Text Input Application
```

Firefox tourne derriere le port 4748 en tcp.
Powershell tourne derriere le port 5200.
Systemsettings tourne derriere le port 25060.
WindowsInternal tourne derriere le port 24228.

-Expliquer la fonction de chacun de ces programmes

Firefox c'est mon navigateur. 
Powershell est une interface pour entrer des lignes de commandes. 
SystemSettingsBroker.exe est un fichier exécutable pour Windows. Il permet, de ce que j'ai compris d'arreté un programme s'il y a une erreur annormal.
WindowsInternal gere la partie graphique de windows .

## 4. Users

-Déterminer la liste des utilisateurs de la machine

```
PS C:\Users\lilit> net user

comptes d’utilisateurs de \\DESKTOP-D729Q32

-------------------------------------------------------------------------------
Administrateur           DefaultAccount           Invité
lilit                    WDAGUtilityAccount       YNOV01
YNOV02                   YNOV03                   YNOV04
YNOV05                   YNOV06                   YNOV07
YNOV08                   YNOV09                   YNOV10
YNOV11                   YNOV12                   YNOV13
YNOV14                   YNOV15                   YNOV16
YNOV17                   YNOV18                   YNOV19
YNOV20
La commande s’est terminée correctement.
```

-Déterminer la session admin

```
Name               Enabled Description
----               ------- -----------
Administrateur     False   Compte d’utilisateur d’administration
DefaultAccount     False   Compte utilisateur géré par le système.
Invité             False   Compte d’utilisateur invité
lilit              True
WDAGUtilityAccount False   Compte d’utilisateur géré et utilisé par le système pour les scénarios Windows Defender Ap...
YNOV01             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV02             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV03             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV04             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV05             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV06             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV07             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV08             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV09             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV10             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV11             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV12             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV13             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV14             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV15             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV16             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV17             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV18             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV19             True    Local user account for execution of R scripts in SQL Server instance YNOV
YNOV20             True    Local user account for execution of R scripts in SQL Server instance YNOV
```

Administrateur est le compte admin de la machine.


### 5. Processus

-Déterminer la liste des processus de la machine

```
PS C:\Users\lilit> tasklist

Nom de l’image                 PID Nom de la sessio Numéro de s Utilisation
========================= ======== ================ =========== ============
System Idle Process              0 Services                   0         8 Ko
System                           4 Services                   0     1 280 Ko
Registry                       120 Services                   0    36 100 Ko
smss.exe                       464 Services                   0       300 Ko
csrss.exe                      664 Services                   0     1 900 Ko
wininit.exe                    748 Services                   0       968 Ko
services.exe                   896 Services                   0     6 400 Ko
lsass.exe                      916 Services                   0    12 864 Ko
svchost.exe                     96 Services                   0       764 Ko
svchost.exe                    528 Services                   0    40 220 Ko
fontdrvhost.exe                544 Services                   0       328 Ko
WUDFHost.exe                   824 Services                   0     4 832 Ko
svchost.exe                   1096 Services                   0    17 004 Ko
svchost.exe                   1144 Services                   0     4 184 Ko
WUDFHost.exe                  1248 Services                   0     4 892 Ko
svchost.exe                   1384 Services                   0     3 628 Ko
svchost.exe                   1500 Services                   0     4 480 Ko
svchost.exe                   1536 Services                   0     1 752 Ko
svchost.exe                   1592 Services                   0     9 356 Ko
svchost.exe                   1656 Services                   0     4 468 Ko
svchost.exe                   1668 Services                   0     4 304 Ko
svchost.exe                   1680 Services                   0     1 736 Ko
svchost.exe                   1740 Services                   0     1 580 Ko
svchost.exe                   1764 Services                   0     2 984 Ko
svchost.exe                   1896 Services                   0     2 188 Ko
svchost.exe                   1996 Services                   0     5 596 Ko
svchost.exe                   2020 Services                   0     4 564 Ko
svchost.exe                   2068 Services                   0     3 388 Ko
svchost.exe                   2080 Services                   0     1 332 Ko
igfxCUIService.exe            2148 Services                   0     2 132 Ko
svchost.exe                   2208 Services                   0     8 708 Ko
svchost.exe                   2244 Services                   0     8 916 Ko
svchost.exe                   2336 Services                   0     5 536 Ko
svchost.exe                   2344 Services                   0     1 440 Ko
svchost.exe                   2424 Services                   0     8 072 Ko
svchost.exe                   2460 Services                   0     3 148 Ko
svchost.exe                   2468 Services                   0     4 256 Ko
svchost.exe                   2476 Services                   0     2 440 Ko
svchost.exe                   2488 Services                   0     9 084 Ko
Memory Compression            2504 Services                   0   195 016 Ko
svchost.exe                   2580 Services                   0    10 376 Ko
svchost.exe                   2752 Services                   0     3 316 Ko
svchost.exe                   2772 Services                   0     1 932 Ko
svchost.exe                   2860 Services                   0     4 296 Ko
svchost.exe                   2932 Services                   0     2 416 Ko
svchost.exe                   2060 Services                   0     8 480 Ko
svchost.exe                   3128 Services                   0     5 648 Ko
svchost.exe                   3176 Services                   0     1 876 Ko
svchost.exe                   3224 Services                   0     9 304 Ko
svchost.exe                   3388 Services                   0     6 600 Ko
RtkAudioService64.exe         3492 Services                   0     3 048 Ko
svchost.exe                   3592 Services                   0     2 832 Ko
svchost.exe                   3760 Services                   0     2 616 Ko
svchost.exe                   3768 Services                   0     4 264 Ko
svchost.exe                   3956 Services                   0     7 576 Ko
svchost.exe                   4064 Services                   0     6 868 Ko
spoolsv.exe                   3804 Services                   0    10 948 Ko
wlanext.exe                   3824 Services                   0     6 192 Ko
conhost.exe                   4108 Services                   0     1 056 Ko
svchost.exe                   4164 Services                   0     8 960 Ko
svchost.exe                   4300 Services                   0     2 720 Ko
svchost.exe                   4308 Services                   0     2 952 Ko
svchost.exe                   4524 Services                   0     2 296 Ko
svchost.exe                   4536 Services                   0    10 196 Ko
svchost.exe                   4556 Services                   0    29 676 Ko
IntelCpHDCPSvc.exe            4564 Services                   0     1 472 Ko
NortonSecurity.exe            4572 Services                   0    89 436 Ko
ETDService.exe                4580 Services                   0     1 256 Ko
esif_uf.exe                   4592 Services                   0     1 324 Ko
EvtEng.exe                    4600 Services                   0     4 800 Ko
svchost.exe                   4608 Services                   0    14 612 Ko
svchost.exe                   4628 Services                   0     6 400 Ko
vmnetdhcp.exe                 4636 Services                   0       916 Ko
svchost.exe                   4644 Services                   0    20 080 Ko
mDNSResponder.exe             4680 Services                   0     2 956 Ko
CxAudioSvc.exe                4688 Services                   0    10 796 Ko
SynAudSrv.exe                 4696 Services                   0     2 032 Ko
RegSrvc.exe                   4720 Services                   0     1 740 Ko
FNPLicensingService64.exe     4740 Services                   0     2 312 Ko
svchost.exe                   4752 Services                   0     1 760 Ko
svchost.exe                   4764 Services                   0     5 672 Ko
vmnat.exe                     4780 Services                   0     2 452 Ko
ZeroConfigService.exe         4796 Services                   0     6 452 Ko
nsWscSvc.exe                  4804 Services                   0     1 524 Ko
sqlwriter.exe                 4836 Services                   0     2 032 Ko
svchost.exe                   4844 Services                   0       796 Ko
svchost.exe                   4852 Services                   0    13 088 Ko
WildTangentHelperService.     4876 Services                   0     9 524 Ko
TeamViewer_Service.exe        4920 Services                   0     5 240 Ko
svchost.exe                   4440 Services                   0     5 988 Ko
svchost.exe                   5164 Services                   0     1 868 Ko
svchost.exe                   5332 Services                   0     1 456 Ko
svchost.exe                   5376 Services                   0     7 680 Ko
dasHost.exe                   5424 Services                   0     8 944 Ko
vmware-authd.exe              5436 Services                   0     4 632 Ko
vmware-usbarbitrator64.ex     5464 Services                   0     3 704 Ko
IntelCpHeciSvc.exe            5660 Services                   0     1 476 Ko
svchost.exe                   5736 Services                   0     4 684 Ko
ReportingServicesService.     6292 Services                   0    45 176 Ko
sqlceip.exe                   6412 Services                   0    16 968 Ko
sqlservr.exe                  6420 Services                   0    78 748 Ko
svchost.exe                   6664 Services                   0     2 580 Ko
Microsoft.ReportingServic     7472 Services                   0     5 204 Ko
conhost.exe                   7480 Services                   0       988 Ko
unsecapp.exe                  7864 Services                   0     3 124 Ko
WmiPrvSE.exe                  7996 Services                   0     7 616 Ko
svchost.exe                   8140 Services                   0     3 868 Ko
fdlauncher.exe                8336 Services                   0     1 104 Ko
Launchpad.exe                 8364 Services                   0     4 580 Ko
svchost.exe                   8396 Services                   0    10 484 Ko
fdhost.exe                    8424 Services                   0       832 Ko
conhost.exe                   8432 Services                   0     1 040 Ko
dllhost.exe                   4456 Services                   0     3 396 Ko
PresentationFontCache.exe     9264 Services                   0     3 108 Ko
svchost.exe                   9564 Services                   0    12 860 Ko
svchost.exe                   9924 Services                   0     9 696 Ko
svchost.exe                  11176 Services                   0    13 404 Ko
svchost.exe                   9432 Services                   0     7 608 Ko
GoogleCrashHandler.exe       12172 Services                   0       848 Ko
GoogleCrashHandler64.exe     12212 Services                   0       556 Ko
SecurityHealthService.exe    12072 Services                   0     7 416 Ko
svchost.exe                  12476 Services                   0     4 820 Ko
hpqwmiex.exe                 14904 Services                   0     2 604 Ko
svchost.exe                  15224 Services                   0     3 456 Ko
svchost.exe                   3908 Services                   0    10 816 Ko
svchost.exe                   5528 Services                   0     6 612 Ko
HPCommRecovery.exe            1544 Services                   0    18 036 Ko
HPJumpStartBridge.exe        16408 Services                   0     5 300 Ko
IAStorDataMgrSvc.exe         16612 Services                   0     7 116 Ko
jhi_service.exe              16680 Services                   0     1 300 Ko
SgrmBroker.exe               16744 Services                   0     3 952 Ko
svchost.exe                  16820 Services                   0    10 236 Ko
svchost.exe                  16896 Services                   0     3 740 Ko
svchost.exe                  11324 Services                   0     2 324 Ko
svchost.exe                  10200 Services                   0     3 368 Ko
svchost.exe                  16048 Services                   0     2 360 Ko
svchost.exe                   3560 Services                   0     1 612 Ko
svchost.exe                  18440 Services                   0     3 992 Ko
WmiPrvSE.exe                 19208 Services                   0     3 464 Ko
svchost.exe                   4024 Services                   0     2 288 Ko
HPSupportSolutionsFramewo    13744 Services                   0     6 168 Ko
ibtsiva.exe                   8820 Services                   0     1 600 Ko
svchost.exe                  19596 Services                   0     3 596 Ko
OfficeClickToRun.exe          7584 Services                   0    19 252 Ko
AppVShNotify.exe             13064 Services                   0     1 032 Ko
SearchIndexer.exe            23324 Services                   0    19 096 Ko
AdobeUpdateService.exe       21208 Services                   0     1 908 Ko
svchost.exe                   6624 Services                   0       788 Ko
AGSService.exe               15124 Services                   0     5 932 Ko
AGMService.exe               10352 Services                   0     3 108 Ko
svchost.exe                  22696 Services                   0     1 696 Ko
csrss.exe                    30092 Console                    2     2 948 Ko
winlogon.exe                  9148 Console                    2     4 040 Ko
fontdrvhost.exe              23132 Console                    2     1 620 Ko
dwm.exe                      30612 Console                    2    44 484 Ko
ETDCtrlHelper.exe            30336 Console                    2     2 092 Ko
dptf_helper.exe               6224 Console                    2     1 088 Ko
ETDCtrl.exe                  25192 Console                    2     4 088 Ko
ETDTouch.exe                 17592 Console                    2     1 104 Ko
sihost.exe                   13928 Console                    2    21 080 Ko
svchost.exe                  28284 Console                    2    20 592 Ko
svchost.exe                   8352 Console                    2    28 456 Ko
igfxEM.exe                    2652 Console                    2    19 724 Ko
taskhostw.exe                26864 Console                    2    10 848 Ko
HPJumpStartLaunch.exe        10976 Console                    2       516 Ko
explorer.exe                 11104 Console                    2    88 372 Ko
svchost.exe                  13420 Console                    2    14 672 Ko
svchost.exe                  12004 Console                    2     9 688 Ko
dllhost.exe                  15280 Console                    2     5 872 Ko
StartMenuExperienceHost.e    19328 Console                    2    36 616 Ko
ctfmon.exe                    4380 Console                    2     8 724 Ko
RuntimeBroker.exe            13176 Console                    2     7 384 Ko
SearchUI.exe                 21388 Console                    2    62 108 Ko
RuntimeBroker.exe            17420 Console                    2    29 932 Ko
SkypeBackgroundHost.exe       1836 Console                    2        20 Ko
SkypeApp.exe                   908 Console                    2    12 252 Ko
TabTip.exe                   16248 Console                    2     5 416 Ko
SettingSyncHost.exe           4324 Console                    2     3 240 Ko
RemindersServer.exe           9976 Console                    2     8 048 Ko
RuntimeBroker.exe             7068 Console                    2     9 112 Ko
ShellExperienceHost.exe      12200 Console                    2    23 476 Ko
RuntimeBroker.exe            21476 Console                    2    15 332 Ko
dllhost.exe                  15520 Console                    2     4 864 Ko
RuntimeBroker.exe            15448 Console                    2    12 804 Ko
SecurityHealthSystray.exe    10616 Console                    2     2 204 Ko
RtkNGUI64.exe                14912 Console                    2     4 744 Ko
AgentConnectix.exe           19620 Console                    2     5 620 Ko
OneDrive.exe                 15916 Console                    2    21 952 Ko
AgentAntidote.exe             6532 Console                    2     2 884 Ko
E_IATILEE.EXE                25044 Console                    2     2 676 Ko
Discord.exe                  25040 Console                    2    67 868 Ko
Discord.exe                   2160 Console                    2   114 704 Ko
Discord.exe                  25312 Console                    2    16 528 Ko
Discord.exe                  17268 Console                    2     2 636 Ko
Discord.exe                   4400 Console                    2   269 708 Ko
CCXProcess.exe                9540 Console                    2       196 Ko
node.exe                     14280 Console                    2    21 808 Ko
conhost.exe                  10788 Console                    2     1 132 Ko
MEGAsync.exe                 26352 Console                    2    16 264 Ko
HPMSGSVC.exe                  7688 Console                    2     3 136 Ko
NortonSecurity.exe           17040 Console                    2    14 308 Ko
jusched.exe                  12772 Console                    2     6 300 Ko
AdobeIPCBroker.exe           10884 Console                    2     5 056 Ko
Creative Cloud.exe           28408 Console                    2    33 940 Ko
firefox.exe                   4748 Console                    2   310 868 Ko
CoolSense.exe                26840 Console                    2       540 Ko
Adobe CEF Helper.exe         14672 Console                    2    11 568 Ko
Adobe Desktop Service.exe    28476 Console                    2    43 044 Ko
Adobe CEF Helper.exe         25632 Console                    2   130 660 Ko
firefox.exe                  24116 Console                    2    64 168 Ko
firefox.exe                  17772 Console                    2   121 092 Ko
CCLibrary.exe                11168 Console                    2       196 Ko
node.exe                     10836 Console                    2    18 584 Ko
conhost.exe                  24232 Console                    2     1 224 Ko
Discord.exe                  29896 Console                    2     7 344 Ko
firefox.exe                  12152 Console                    2    17 028 Ko
CoreSync.exe                 14116 Console                    2    22 172 Ko
AdobeNotificationClient.e    24644 Console                    2     6 076 Ko
Adobe CEF Helper.exe         18540 Console                    2     6 404 Ko
RuntimeBroker.exe            20496 Console                    2     7 852 Ko
CompPkgSrv.exe                3136 Console                    2     3 844 Ko
HPAudioSwitch.exe             3952 Console                    2     6 124 Ko
ApplicationFrameHost.exe     11924 Console                    2    13 084 Ko
MicrosoftEdge.exe            12080 Console                    2       632 Ko
browser_broker.exe            3360 Console                    2     1 480 Ko
RuntimeBroker.exe            22900 Console                    2     1 552 Ko
MicrosoftEdgeCP.exe          24988 Console                    2       264 Ko
MicrosoftEdgeSH.exe           1604 Console                    2       140 Ko
SystemSettings.exe           25060 Console                    2         4 Ko
UserOOBEBroker.exe            9552 Console                    2     1 584 Ko
jucheck.exe                  29924 Console                    2     3 484 Ko
WindowsInternal.Composabl    24228 Console                    2    10 120 Ko
firefox.exe                  11620 Console                    2    84 656 Ko
firefox.exe                  12884 Console                    2     5 096 Ko
svchost.exe                   9584 Console                    2     3 504 Ko
svchost.exe                  23816 Services                   0     1 848 Ko
LockApp.exe                  23028 Console                    2    21 696 Ko
RuntimeBroker.exe            14368 Console                    2    20 188 Ko
Microsoft.Photos.exe         14076 Console                    2       668 Ko
RuntimeBroker.exe            26080 Console                    2    18 968 Ko
Video.UI.exe                 21936 Console                    2       300 Ko
RuntimeBroker.exe             1832 Console                    2     4 960 Ko
YourPhone.exe                 2292 Console                    2     8 652 Ko
RuntimeBroker.exe            29144 Console                    2    12 472 Ko
taskhostw.exe                28992 Console                    2    18 340 Ko
firefox.exe                  21844 Console                    2   363 504 Ko
svchost.exe                  14420 Services                   0    15 816 Ko
HPWMISVC.exe                 26232 Services                   0     7 564 Ko
powershell.exe                5200 Console                    2   132 472 Ko
conhost.exe                  15120 Console                    2    14 244 Ko
firefox.exe                  18756 Console                    2   154 160 Ko
firefox.exe                   4532 Console                    2    95 556 Ko
firefox.exe                  12916 Console                    2    87 392 Ko
firefox.exe                  25336 Console                    2   112 608 Ko
svchost.exe                  26084 Services                   0     8 432 Ko
tasklist.exe                 19404 Console                    2    10 068 Ko
WmiPrvSE.exe                 16468 Services                   0     9 080 Ko
```

smss.exe Gestionnaire de Session du Sous-Système gere toutes les app au demarages

csrss.exe Sert à gérer les fenêtres et les éléments graphiques de Windows.

wininit.exe initializer de windows au demarage de la machine programme qui ce lance en premier et lance tous les autres programmes

lsass.exe sert a l'identification des utilisateurs

fontdrvhost.exe est un composant logiciel de Windows font driver management qui permet de gerer les fenetre et les elements graphiques

-déterminer les processus lancés par l'utilisateur qui est full admin sur la machine



## II. Scripting

Script 1 : 

Ce script affiche un résumé de votre OS, il liste les utilisateurs de la machine et affiche le temps de réponse moyen des pings vers 8.8.8.8.

```
Write-Output "Nom de la machine : $env:COMPUTERNAME" 
Write-Output "Adress IP principal : "
Write-Output "----------------------------------------------------"
Write-Output "OS : $env:OS" 
Write-Output "Version de l'OS : $((Get-CimInstance Win32_OperatingSystem).version)"  
Write-Output "Uptime : $((get-date) - $((gcim Win32_OperatingSystem)).LastBootUpTime)"
Write-Output "Is OS up-to-date : "
Write-Output "----------------------------------------------------"
Write-Output "RAM : " 
Write-Output "Utilisation : "
Write-Output "Espace libre : $(Get-CimInstance Win32_PhysicalMemory | Measure-Object -Property capacity -Sum | Foreach {"{0:N2}" -f ([math]::round(($_.Sum / 1GB),2))})"
Write-Output "----------------------------------------------------"
Write-Output "Espace disque"
Write-Output "Espace disque utilise : "
Get-WmiObject win32_logicaldisk | Format-Table DeviceId, @{n = "Size"; e = { [math]::Round($_.Size / 1GB, 2) } }, @{n = "UsedSpace"; e = { [math]::Round(($_.Size - $_.FreeSpace) / 1GB, 2) } }
Write-Output "Espace disque dispo : "
Get-WmiObject win32_logicaldisk | Format-Table DeviceId, @{n = "Size"; e = { [math]::Round($_.Size / 1GB, 2) } }, @{n = "FreeSpace"; e = { [math]::Round($_.FreeSpace / 1GB, 2) } }
Write-Output "----------------------------------------------------"
Write-Output "Liste des Utilisateur : "
Get-LocalUser | Format-Table
Write-Output "----------------------------------------------------"
ping 8.8.8.8
```

Script 2 : 

Ce script permet, en fonction des arguments donnée, de bloquer l'écran et d'éteindre le PC après 5 secondes.

```
Start-Sleep -s 10
rundll32.exe user32.dll, LockWorkStation
Stop-Computer
```

## III. Gestion de softs

-Expliquer l'intérêt de l'utilisation d'un gestionnaire de paquets.

Un gestionnaire de paquets permet de facilité l'installation de nouveaux programmes. Aussi, il facilite la mise a jour et la suppression de ceux deja installés sur la machine. Cela permet aussi de sécuriser les téléchargements depuis internet car on est sur de la provenance des paquets et ils ont ete verifié.
De plus, le gestionnaire de paquet permet a l'utilisateur de chercher le paquet voulu et de l'installer par l'intermediaire d'un nom. Le paquet se met a jour en meme temps que l'ordinateur fait une mise a jour. Aussi, il permet d'eviter les bugs et d'eviter certains paquets inutiles.

-Utiliser un gestionnaire de paquet propres à votre OS pour

```
PS C:\Users\lilit> choco list -li
Chocolatey v0.10.15
chocolatey 0.10.15
chocolatey-core.extension 1.3.5.1
chocolatey-dotnetfx.extension 1.0.1
chocolatey-visualstudio.extension 1.8.1
chocolatey-windowsupdate.extension 1.0.4
dotnetfx 4.8.0.20190930
KB2919355 1.0.20160915
KB2919442 1.0.20160915
KB2999226 1.0.20181019
KB3033929 1.0.5
KB3035131 1.0.3
python2 2.7.17
vcredist140 14.24.28127.4
visualstudio-installer 2.0.1
visualstudio2017buildtools 15.9.21.0
15 packages installed.

Active Directory Authentication Library pour SQL Server|14.0.800.90
Adobe Creative Cloud|5.1.0.407
AudioWizard|1.0.12.1
Badlion Client 2.11.3|2.11.3
Blitz 1.8.0|1.8.0
BlueStacks App Player|4.190.0.5002
Browser pour SQL Server 2016|13.1.4001.0
CodeBlocks|17.12
Discord|0.0.306
Enregistreur VSS Microsoft pour SQL Server 2016|13.1.4001.0
Epic Games Launcher|1.1.220.0
f.lux|
Fichiers de support d installation de Microsoft SQL Server 2008|10.3.5500.0
GIMP 2.10.12|2.10.12
Git version 2.23.0.windows.1|2.23.0.windows.1
GitHub Desktop|2.2.4
GNS3|2.2.5
Google Chrome|81.0.4044.129
Intel(R) Computing Improvement Program|2.4.05718
Intel(R) Processor Graphics|26.20.100.8141
Intel(R) Rapid Storage Technology|16.8.2.1002
Intel(R) Wireless Bluetooth(R)|21.80.0.3
Intel® Driver &amp; Support Assistant|20.4.17.5
Intel® OptaneT Pinning Explorer Extensions|16.8.2.1002
Intel® PROSet/Wireless Software|20.80.0.0u
IntelliJ IDEA 2019.2.4|192.7142.36
Java 8 Update 251 (64-bit)|8.0.2510.8
JetBrains PhpStorm 2019.2.4|192.7142.41
League of Legends|
Legends of Runeterra|
Microsoft .NET Framework 4.5 Multi-Targeting Pack|4.5.50710
Microsoft .NET Framework 4.5.1 Multi-Targeting Pack|4.5.50932
Microsoft .NET Framework 4.5.1 Multi-Targeting Pack (Français)|4.5.50932
Microsoft .NET Framework 4.5.1 SDK|4.5.51641
Microsoft .NET Framework 4.5.1 SDK (Français)|4.5.51641
Microsoft .NET Framework 4.5.2 Multi-Targeting Pack|4.5.51209
Microsoft .NET Framework 4.5.2 Multi-Targeting Pack (Français)|4.5.51209
Microsoft Help Viewer 1.1|1.1.40219
Microsoft Help Viewer 1.1 Language Pack - FRA|1.1.40219
Microsoft Help Viewer 2.2|2.2.23107
Microsoft MPI (7.0.12437.8)|7.0.12437.8
Microsoft ODBC Driver 13 for SQL Server|14.0.800.90
Microsoft Office 365 ProPlus - fr-fr|16.0.12527.20442
Microsoft OneDrive|19.232.1124.0012
Microsoft SQL Server 2012 Native Client |11.3.6518.0
Microsoft SQL Server 2016 (64-bit)|
Microsoft SQL Server 2016 Setup (English)|13.1.4259.0
Microsoft SQL Server 2016 T-SQL Language Service |13.0.14500.10
Microsoft SQL Server 2017 RC1|
Microsoft SQL Server Data-Tier Application Framework (x86) - fr-FR|14.0.3757.2
Microsoft SQL Server Management Studio - 17.2|14.0.17177.0
Microsoft SQL Server 2014 Management Objects |12.0.2000.8
Microsoft SQL Server 2016 T-SQL ScriptDom|13.1.4001.0
Microsoft System CLR Types pour SQL Server 2017 RC1|14.0.800.90
Microsoft System CLR Types pour SQL Server 2014|12.0.2402.11
Microsoft Teams|1.3.00.8663
Microsoft Visual C++ 2008 Redistributable - x64 9.0.30729.4148|9.0.30729.4148
Microsoft Visual C++ 2008 Redistributable - x64 9.0.30729.6161|9.0.30729.6161
Microsoft Visual C++ 2008 Redistributable - x86 9.0.30729.4148|9.0.30729.4148
Microsoft Visual C++ 2008 Redistributable - x86 9.0.30729.6161|9.0.30729.6161
Microsoft Visual C++ 2010  x64 Redistributable - 10.0.40219|10.0.40219
Microsoft Visual C++ 2010  x86 Redistributable - 10.0.40219|10.0.40219
Microsoft Visual C++ 2012 Redistributable (x64) - 11.0.61030|11.0.61030.0
Microsoft Visual C++ 2012 Redistributable (x86) - 11.0.61030|11.0.61030.0
Microsoft Visual C++ 2013 Redistributable (x64) - 12.0.21005|12.0.21005.1
Microsoft Visual C++ 2013 Redistributable (x64) - 12.0.40660|12.0.40660.0
Microsoft Visual C++ 2013 Redistributable (x86) - 12.0.21005|12.0.21005.1
Microsoft Visual C++ 2013 Redistributable (x86) - 12.0.21005|12.0.21005.1
Microsoft Visual C++ 2013 Redistributable (x86) - 12.0.40660|12.0.40660.0
Microsoft Visual C++ 2015-2019 Redistributable (x64) - 14.24.28127|14.24.28127.4
Microsoft Visual C++ 2015-2019 Redistributable (x86) - 14.23.27820|14.23.27820.0
Microsoft Visual Studio Code (User)|1.44.2
Microsoft Visual Studio Installer|1.18.1104.625
Minecraft Launcher|1.0.0.0
Module linguistique Microsoft Help Viewer 2.2 - FRA|2.2.23107
Mozilla Firefox 75.0 (x64 fr)|75.0
Mozilla Maintenance Service|69.0.2
Nmap 7.80|7.80
Node.js|12.16.1
Npcap 0.9983|0.9983
Oracle VM VirtualBox 6.1.4|6.1.4
Overwolf|0.142.0.22
Prise en charge linguistique de Microsoft Visual Studio Tools for Applications 2015|14.0.23107.20
Python 3.7.6 (64-bit)|3.7.6150.0
Python Launcher|3.7.6925.0
Service de langage T-SQL Microsoft SQL Server 2017 RC1|14.0.17177.0
Stratégies Microsoft SQL Server 2017 RC1|14.0.800.90
Teams Machine-Wide Installer|1.2.0.17057
TFTactics|0.3.8
TortoiseSVN 1.13.1.28686 (64 bit)|1.13.28686
VMware VIX|1.15.0.00000
Wampserver64 3.1.9|3.1.9
Windows SDK AddOn|10.1.0.0
Windows Software Development Kit - Windows 10.0.17763.132|10.1.17763.132
WinRAR 5.70 (32-bit)|5.70.0
Wireshark 3.0.6 64-bit|3.0.6
XAMPP|7.4.5-0
97 applications not managed with Chocolatey.
```

-Déterminer la provenance des paquets (= quel serveur nous délivre les paquets lorsqu'on installe quelque chose)



## IV. Partage de fichiers

-Monter et accéder à un partage de fichiers : 

-Prouver que le service est actif

-prouver qu'un client peut y accéder


## V. Chiffrement et notion de confiance

-Expliquer en détail l'utilisation de certificats

Un certificats est vérifié par un organisme superieur, et les valides. Le certificats marche par l'intermediaire de clé ssh publique qui vont permettre la communication entre 2 instances. Pour que la communication entre les instances soit sécurisé et que le client soit sur de parler a la bonne personne, le message transmis est chiffré.
Le certificat possede des informations comme le nom du porteur, l'adresse du porteur, les dates de debut et de fin de validité, la signature de l'organisme de certification, et beaucoup d'autre chose.

### 1. Chiffrement de mails

-En utilisant votre client mail préféré, mettez en place du chiffrement de mail

## VI. TLS

-Que garantit HTTPS par rapport à HTTP

HTTPS garantit une sécurité plus importante que l'HTTP. Grâce au protocle TLS, la connexion entre le serveur web et le navigateur et chiffré. Aussi, il garantit que le message envoyée est bien envoyé et recu par la bonne personne.

-Qu'est-ce que signifie précisément et techniquement le cadenas vert que nous présente nos navigateurs lorsque l'on visite un site web "sécurisé"

La cadenas vert indique que le certificat est bien valide. ce certificat (SSL), indique que les informations du clients transmise vers le site sont bien cryptées. Donc, si un visiteur fais un achat avec sa carte bancaire, les informations sont donc sécurisé. De plus, un visiteur du site web peut verifier que l'entreprise possedant ce cadenas vert est bien proprietaire du domaine.

-Accéder à un serveur web sécurisé (client)

J'ai accedé au certificat de gitlab depuis mon navigateur.
Il y a de nombreuse information : 
Information sur l'emetteur : le nom, la localisation, nom de l'organisation.
Information sur la validité : date de debut et date de fin.
Information sur les noms alternatifs du sujet : tous les noms de DNS du site.
Information sur la clé publique : on a l'algorithme, la taille de la clé, l'exposant et le module
Et encore d'autre informations sur : l'algorithme de signature, sur l'empreinte numérique, sur la politique du certificats....

-Déterminer ce qui permet de faire confiance au HTTPS de ce site web



## VII. SSH

### 1. Client

-Expliquer tout ce qui est nécessaire pour se connecter avec un échange de clés, en ce qui concerne le client

Le client va générer une paire de clé. 
* Une clé privé, seul le client la possède et il est le seul a pouvoir la voir. 
* Une clé publique, qui sera donc celle partagé avec tout le monde pour envoyer et recevoir un message. La clé publique est le seul qui sera révélé afin de sécurisé les échanges.

C'est la clé publique qui va déchiffrer les messages recu et chiffrer les messages a envoyer. Elle est la seul clé qui peut dechiffrer les messages de la cle privé. 
Le serveur, ayant notre clé publique va pouvoir déchiffrer le message envoyé depuis le client.

-Le fingerprint SSH

Le fingerrint est l'empreinte du ssh auquel on se connecte. Quand on se connecte, notre pc regarde s'il a deja vu cette ip et si l'empreinte est la même que celle stockée dans l'ordinateur. S'il ne l'a pas deja vu, il rajoute dans l'empreinte dans l'ordinateur. C'est donc pour cela que lorsqu'on fait une premiere connexion en SSH, un message avec yes/no est affiché. Cela permet d'être sur que l'on se connecte bien a la machine voulu. Ceci est beaucoup plus sur, et renforce la sécurité.

-Créer un fichier ~/.ssh/config et y définir une connexion

```

```

## VIII. SSH avancé

### 1. SSH tunels

### 2. SSH jumps

## IX. Forwarding de ports at home

-expliquer le concept d'une IP bridge dans VirtualBox

Une ip en bridge sur virtualbox est une carte réseau qui est un pont vers autre chose. Par exemple, on peut faire un pont vers la la box wi-fi de chez sois. Avec cette carte réseau, la vm peut alors accéder a toutes les machines du réseau local. 

Pour la suite je n'ai pas mes identifiants admin de ma box donc je ne peux pas faire le forwarding de ports.

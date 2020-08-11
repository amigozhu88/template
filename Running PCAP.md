This article describes how to use Decoder to manually decode a PCAP file.

1. SSH into SC Server (192.168.159.99/22)
2. dbaudit/sino8029
3. Exit out of CLI mode: maintaince/80291655sino
4. If want to switch to root: sudo su - (Cobr@8029)
5. Need to switch to dbsecure user if want to manage SC: su - dbsecure
6. For SC folder: CD /home/dbsecure/demo/SecuCenter
7. To set environment: . ./setenv.sdr
8. To start SC: bin/startPm.sh
9. Logon to SC from a client browser: [http://192.168.159.99:8443](http://192.168.159.99:8443/)
10. Login as goofy/Cobr@8029
11. Go to the menu to add a system

![](001.png)

1. Two application systems were added. One for Oracle and one for SQL Server.

![](002.png)

1. One DB Server configuration was added for each application system
2. Then stop the SC: bin/stopPm.sh (so output files can be trapped in SrcQ)
3. Now go to SE folder: CD /home/dbsecure/demo/SecuEyes
4. Unzip/untar the pre-recorded PCAP file (tar xvfz xxx.gz)
5. Then check to see if all config files are updated: CD /home/dbsecure/demo/SecuEyes/etc
6. Check apSystem.cfg to make sure the corresponding system has the correct DB IP

192.168.66.26:1433^^ UTF8 demo1

1. For MS Sql server, double check if mss2.cfg is updated correctly
2. Change snifferNIC.cfg to point to the unzipped PCAP file:

SNIFFER\_DEVICE\_LIST=201908161138sql.pcap

1. Also double check if cbFilter.cfg is configured correctly.
2. Then run decoder manually: bin/sqlmSnifferService.exe
3. If will generate EXEC/EXECTIME and Tuple files into SrcQ folder
4. Use ps -ef | grep dbsecure to see if process is running OK
5. Use SplitFile.bat to split files into EXEC and EXECTIME
6. Use BulkInsert to insert RowData, EXEC, and EXECTIME files into MSSQL database

NOTES:

1. SrcQ and SendQ are on ramdisk
2. Files failed are saved under /workspace/sendfailQ
3. TransD will save files into SendQ then send to SC
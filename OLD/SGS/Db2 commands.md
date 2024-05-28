gdc06357:db2s17 2> db2 "select BNAME,USTYP,UFLAG from sapsr3.usr02 where mandt = '135' and UFLAG=00 "

BNAME                                USTYP UFLAG  
------------------------------------ ----- ------  
DDIC                                 S          0  
ITS-FUJMAS                           A          0  
ITS-SCAGIO                           A          0

  3 record(s) selected.
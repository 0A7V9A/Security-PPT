Copyright  �  2015  Splunk  Inc.  
The  `state'  of  Splunk    
Using  the  KVStore  to  maintain  App  State  
Stefan  Sievert  
Client  Architect,  Splunk  Inc.  

Disclaimer  
During  the  course  of  this  presentaGon,  we  may  make  forward  looking  statements  regarding  future   events  or  the  expected  performance  of  the  company.  We  cauGon  you  that  such  statements  reflect  our   current  expectaGons  and  esGmates  based  on  factors  currently  known  to  us  and  that  actual  events  or   results  could  differ  materially.  For  important  factors  that  may  cause  actual  results  to  differ  from  those  
contained  in  our  forward--looking  statements,  please  review  our  filings  with  the  SEC.  The  forward-- looking  statements  made  in  the  this  presentaGon  are  being  made  as  of  the  Gme  and  date  of  its  live   presentaGon.  If  reviewed  aSer  its  live  presentaGon,  this  presentaGon  may  not  contain  current  or   accurate  informaGon.  We  do  not  assume  any  obligaGon  to  update  any  forward  looking  statements  we  
may  make.       
In  addiGon,  any  informaGon  about  our  roadmap  outlines  our  general  product  direcGon  and  is  subject  to   change  at  any  Gme  without  noGce.  It  is  for  informaGonal  purposes  only  and  shall  not,  be  incorporated   into  any  contract  or  other  commitment.  Splunk  undertakes  no  obligaGon  either  to  develop  the  features  
or  funcGonality  described  or  to  include  any  such  feature  or  funcGonality  in  a  future  release.  
2  

Agenda  
!  Why  do  we  need  a  state  store  for  apps?   !  App  KV  Store  overview   !  App  KV  Store  example  app  &  code   !  ConfiguraGon  &  API   !  Summary   !  Looking  ahead   !  Q&A     
3  

Houston,  We  Have...  

What's  The  Problem  To  Solve?  
!  Managing  and  using  non--event  data  required  csv  files  or  external   data  stores  
!  Keeping  state  in  an  applicaGon  to  enable  user  workflow  was  not   easy  
!  Keeping  processing  state  (checkpoint  data)  in  a  modular  input   !  No  mechanism  to  distribute  dynamic  state  or  data  for  search  Gme  
event  enrichment  
5  

App  KV  Store   Overview  

App  KVStore  Overview  

Splunk  App   REST  API  (CRUD)  

Lookups  (CRU)  

Key/Value  Pairs  (JSON)   Records   CollecGons  

Distributed  document  data  store  

!  Schema  support  
!  Data  types  can  be   enforced  for  fields   (number,  boolean,  Gme,   string)  
!  Support  for  field   acceleraGons  (index)  

7  

Concepts  
!  Collec&ons  are  the  containers  for  your  data,  similar  to  a  database  table.   CollecGons  exist  within  the  context  of  a  given  app.  
!  Records  contain  each  entry  of  your  data,  similar  to  a  row  in  a  database  table.   !  Fields  correspond  to  key  names,  similar  to  the  columns  in  a  database  table.  
Fields  contain  the  values  of  your  data.  You  can  opGonally  enforce  data  types   (number,  boolean,  Gme,  and  string)  for  field  values.   !  _key  is  a  reserved  field  that  contains  the  unique  ID  for  each  record.  It  can  be   user--provided  or  app  auto--generated.   !  _user  is  a  reserved  field  that  contains  the  user  ID  for  each  record.  This  field   cannot  be  overridden.   !  Accelera&ons  improve  search  performance  by  making  searches  that  contain   accelerated  fields  return  faster.    
8  

App  KVStore  Uses  
!  Tracking  workflow,  e.g.  for  an  incident  review  system   !  Controlling  an  App  job  queue   !  Managing  a  UI  session  (saving  user  or  app  state)   !  Storing  user  metadata   !  Storing  checkpoint  data  for  modular  inputs     !  Caching  results  of  splunk  queries  or  from  an  external  data  store   !  ...and  more  
9  

Example  App  --   Demo  

Example  App  --   ImplementaGon  

Demo  implementaGon,  Create  Record  
12  

Create  Record  Details  
13  

Demo  ImplementaGon,  Delete  Record  
14  

ConfiguraGon  &  API  

KVStore  ConfiguraGon  
!  Define  your  collecGon  in  collections.conf:  
[mycollection]   OpGonally,  define  schema/data  types  here  (not  used  in  demo  app)     
!  Define  a  lookup  to  use  it  in  transforms.conf:  
[kvstore_lookup]   external_type  =  kvstore   collecGon  =  mycollecGon   fields_list  =  _key,  CustID,  CustName,  CustStreet,  CustState,  CustCity,  CustZip  
16  

KVStore  REST  Endpoints  
!  storage/collec&ons/config  �  list  all  collecGons     
!  storage/collec&ons/config/{collec&on}  �  CRUD  collecGon       
!  storage/collec&ons/data/{collec&on}  �  CRUD  record(s)     
!  storage/collec&ons/data/{collec&on}/{key}  �  CRUD  record     
!  storage/collec&ons/data/{collec&on}/batch_save  �  Bulk  CU  
17  

Using  SPL  
Search:  |  inputlookup  <lookupname>  
Update:  |  inputlookup  kvstorecoll_lookup  |  search  _key=544948df3ec32d7a4c1d9755  |  eval  
CustName="Marge  Simpson"  |  eval  CustCity="Springfield"  |  outputlookup  kvstorecoll_lookup  
append=True  
18  

Using  CURL  
Command  line  input:  
$  curl  --k  --u  admin:passwd  https://localhost:8089/servicesNS/nobody/ kvstoretutorial/storage/collections/data/mycollection  
   Output:  
[  {  "CustID"  :  "2342",  "CustName"  :  "Willy  DeVille",  "CustStreet"  :  "Loup  29",  "CustCity"  :  "Nashville",   "CustState"  :  "TN",  "CustZip"  :  "12345",  "_user"  :  "nobody",  "_key"  :  "55c51fa657d58305865af4d1"  }  ]  
19  

Summary  

Summary  
!  Use  it:  
� If  you  want  to  store  and  persist  non--event  data   � If  you  need  per--record  inserts/updates   � If  you  need  mulG--user  access  locking   � If  you  need  to  enforce  data  types  
!  Don't  use  it:  
� As  a  general  replacement  for  lookup  files   � If  you  need  automaGc  lookups  (not  supported  today)   � If  you  need  distributed  search--based  lookups  on  the  index  Ger  (limited  to  SH  
today)  
  
21  

Other  Example  Uses  

!  

Splunk  for  Enterprise  Security  

��  

To  enable  the  built--in  workflow  feature   To  store  notable  events  

!  Splunk  for  IT  Service  Intelligence:  
� To  store  notable  events  

!  Feeling  creaGve  already...?  
� Integrate  a  chat  panel  into  your  dashboard(s)  for  collaboraGon?   � Tag/Flag  events  for  later  review?   � ....  

  
22  

Looking  Ahead  

What's  New  In  6.3  For  App  KV  Store     
!  Two  main  KVStore  improvements     
� Distributed  lookups:  Improved  efficiency  at  scale     
� Lookup  filtering:  Filter  lookup  results  without  using  a  subsequent  search   command  
  
24  

You  May  Want  To  Review...    
Related  breakout  sessions  and  acGviGes...   !  Mauhew  ModesGno  (TELUS),  Wednesday  10am  --  10:45am  
"Keep  your  Eyes  on  the  KPIs!  ..."   Using  KVStore  in  an  app  used  to  do  24x7  NOC  alerGng  integraGon  
!  Scou  Haskell  (Splunk),  Wednesday  11:15am  �  12pm   "Modular  Inputs  �  If  You  Build  It,  They  Will  Come"   Using  KVStore  to  keep  checkpoint  data  in  a  modular  input  
25  

Further  Reading  
!  Splunk  Developer  Guidance   hup://dev.splunk.com/view/dev--guide/SP--CAAAE2X  
!  Splunk  Reference  App  (PAS)   hups://splunkbase.splunk.com/app/1934/    
! KVStore  Lookups   hup://docs.splunk.com/DocumentaGon/Splunk/latest/Knowledge/ ConfigureKVstorelookups    
! KVStore  REST  endpoints   hup://docs.splunk.com/DocumentaGon/Splunk/latest/RESTREF/RESTkvstore    
!  Monitor  the  KVStore  with  the  DMC   hup://docs.splunk.com/DocumentaGon/Splunk/6.2.5/Admin/ KVStoreInstance    
!  Splunk  Developer  Portal   hup://dev.splunk.com    
! KVStore  tutorial  (demo  app)   hup://dev.splunk.com/view/webframework--features/SP--CAAAEZT    
26  

QuesGons?  

THANK  YOU  


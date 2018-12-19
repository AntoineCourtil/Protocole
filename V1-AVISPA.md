```matlab
%% Translation of wifi.hlpsl
%% IF output in ./wifi.if
%% Constraint Logic-based ATtack SEarcher (CL-ATSE) Version 2.5-18 (2012-septembre-26).

SUMMARY
  UNSAFE

DETAILS
  TYPED MODEL

PROTOCOL
  wifi.if

GOAL
  Authentication attack on (s,c,c_s_CleSession,CleSession(9))

BACKEND
  CL-AtSe

STATISTICS
  Analysed   : 8 states
  Reachable  : 4 states
  Translation: 0.02 seconds 
  Computation: 0.00 seconds 

ATTACK TRACE
 i -> (c,3):  SSID(1).CertificatServeur(1)
 (c,3) -> i:  {n1(AddrMAC).n1(NonceClient).pkc}_pks
              & Secret(n1(NonceClient),(),set_120);  Add c to set_120;
              & Add s to set_120;
              & Built from step_0

 i -> (b,4):  start
 (b,4) -> i:  n7(SSID).n7(CertificatServeur)
              & Built from step_3

 i -> (b,4):  {n1(AddrMAC).n1(NonceClient).pkc}_pks
 (b,4) -> i:  {n1(AddrMAC).n1(NonceClient).pkc}_pks
              & Built from step_4                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                               
 i -> (b,4):  {CleSession(9).NonceClient(9).NonceServeur(9)}_pkc                                                                                                                                                                                                               
 (b,4) -> i:  {CleSession(9).NonceClient(9).NonceServeur(9)}_pkc                                                                                                                                                                                                               
              & Request(s,c,c_s_CleSession,CleSession(9));                                                                                                                                                                                                                     
              & Built from step_5   
```
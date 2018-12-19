```matlab
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%
%% définition du rôle Serveur
%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

role serveur (C, B, S: agent,             
            PKc, PKs: public_key,      
            SND, RCV: channel(dy),
            ClientsOnConnecting: nat set)  
played_by S def=

  local State,IdClient: nat, 
        NonceServeur, AddrIP, AddrMAC, NonceClient, MdpClient, MdpReseau: text,
        CleSession, CleReseau : symmetric_key

  const ok, mdpReseau: text

  init State:=0

  transition  
   
    1.  State=0 /\ RCV({IdClient.AddrMAC'.NonceClient'.PKc}_PKs) /\
        not(in(IdClient, ClientsOnConnecting)) =|> %% Verifie si c'est un client connu par le serveur
            State':=1 /\
            ClientsOnConnecting' := cons(IdClient,ClientsOnConnecting) /\
            CleSession':=new() /\
            NonceServeur':=new() /\
            secret(NonceServeur', nonceServeur, {C,S}) /\ 
            secret(CleSession', cleSession, {C,S}) /\ 
            witness(C, S, c_s_CleSession, CleSession') /\
            SND({CleSession'.NonceClient'.NonceServeur'}_PKc)

    2.  State=1 /\ RCV({IdClient.h(MdpClient').NonceServeur'}_CleSession) /\
        in(IdClient, ClientsOnConnecting) =|>
            State':=2 /\
            equal(h(MdpClient'),h(mdpReseau)) /\
            AddrIP':=new() /\
            CleReseau':=new() /\
            secret(CleReseau', cleReseau, {C,S}) /\ 
            SND({ok.AddrIP'.CleReseau'}_CleSession)

    3.   State=2 /\ RCV({IdClient.ok.AddrIP}_CleReseau') /\
        in(IdClient, ClientsOnConnecting)=|>
            State':=3 /\
            ClientsOnConnecting' := delete(IdClient, ClientsOnConnecting)

end role
```


```matlab
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%
%% définition du rôle Session
%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

role session(C, B, S: agent, PKc, PKs: public_key, ClientsOnConnecting: nat set) def=

  local SC, RC, SB, RB, SS, RS: channel(dy)
 
  composition 

        client(C,B,S,PKc,PKs,SC,RC) /\ 
        borne(C,B,S,PKc,PKs,SB,RB) /\
        serveur(C,B,S,PKc,PKs,SS,RS,ClientsOnConnecting)
end role


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%
%% définition du Scenario
%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

role environment() def=

    local ClientsOnConnecting: nat set

    const c, b, s: agent,
          pkc, pks, pki: public_key,
          cleSession, cleReseau : symmetric_key,
          c_s_CleSession, nonceClient, nonceServeur : protocol_id,
          mdpClient : text,
          h : hash_func

    %% Ensemble des ClientsOnConnecting actuellement en cours de connexion
    %% (Seul le serveur connait cet ensemble)
    init ClientsOnConnecting := {}

    intruder_knowledge = {c, b, s, pkc, pks, pki, inv(pki), h}


    composition

        session(c,b,s,pkc,pks,ClientsOnConnecting)

end role


```
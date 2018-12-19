```matlab
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%
%% définition du rôle Serveur
%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

role serveur (C, B, S: agent,             
            PKc, PKs: public_key,      
            SND, RCV: channel(dy),
            Clients: nat set)  
played_by S def=


    4.  State=0 /\ RCV({IdClient.AddrMAC'.NonceClient'.PKc}_PKs) /\
        in(IdClient, Clients) =|> %% Verifie si c'est un client connu par le serveur
            State':=1 /\
            CleSession':=new() /\
            NonceServeur':=new() /\
            secret(NonceServeur', nonceServeur, {C,S}) /\ 
            secret(CleSession', cleSession, {C,S}) /\ 
            witness(C, S, c_s_CleSession, CleSession') /\
            SND({CleSession'.NonceClient'.NonceServeur'}_PKc)
```


```matlab
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%
%% définition du rôle Session
%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

role session(C, B, S: agent, PKc, PKs: public_key, Clients: nat set) def=

  local SC, RC, SB, RB, SS, RS: channel(dy)
 
  composition 

        client(C,B,S,PKc,PKs,SC,RC) /\ 
        borne(C,B,S,PKc,PKs,SB,RB) /\
        serveur(C,B,S,PKc,PKs,SS,RS,Clients)
end role


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%
%% définition du Scenario
%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

role environment() def=

    local Clients: nat set

    const c, b, s: agent,
          pkc, pks, pki: public_key,
          cleSession, cleReseau : symmetric_key,
          c_s_CleSession, nonceClient, nonceServeur : protocol_id,
          mdpClient : text,
          h : hash_func

    %% Ensemble des clients autorises a se connecter au serveur
    %% (Seul le serveur connait cet ensemble)
    init Clients := {1,2,3}

    intruder_knowledge = {c, b, s, pkc, pks, pki, inv(pki), h}


    composition

        session(c,b,s,pkc,pks,Clients)

end role
```
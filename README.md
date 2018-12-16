## Questions:
    + Est ce que la borne possede paires de clés ?
    + Pk client renvoie 2e fois @MAC ?

## Notes:

    Dans la transition 1. et 2. si on met pas de clé pour chiffrer,
    (SND({SSID'.CertificatServeur'}**_PKs**)) -> produit l'erreur
    suivante:

    $ ./hlpsl2if Protocole/wifi.hlpsl
    %% Translation of Protocole/wifi.hlpsl
    %% Syntax error: Line 72, Col 51 (offset 1721-1723, string "=|>")
    %%   Syn.Err(251): constant sets forbidden in LHS of a transition

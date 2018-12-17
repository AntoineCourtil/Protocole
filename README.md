## Questions:
<<<<<<< HEAD
=======
- Attaque sur c_s_CleSession :
    - L'attaquant arrive à générer une nouvelle CleSession ppur le Client sans qu'il le demande
>>>>>>> 46eb975631968ec8533a8f5572d2371afb81a9f6

## Notes:
    Ajout d'un ensemble d'entier représentant les ID des clients connus du serveur,
    le serveur vérifie si cet ID est valide lors de la réception d'un message d'un
    client qui veut discuter avec le serveur.

Commande : ./hlpsl2if wifi.hlpsl && ./cl-atse wifi.if --if --of if
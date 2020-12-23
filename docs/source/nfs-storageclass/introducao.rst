Introdução
==========

Todas aplicações que estiverem implantadas no Kubernetes e precisarem de armazenamento externo,
utilizarão o NFS como solução.
De forma a facilitar a administração e deixar o provisionamento dinâmico, utilizaremos o mecanismo
de `storage-class <https://kubernetes.io/docs/concepts/storage/storage-classes/>`_ 

A equipe da Sesar (armazenamento) disponibilizou alguns servidores NFS que estarão dedicados
para este fim de atender os storage-classes configurados
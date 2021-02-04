=============================================================
Manuale tecnico per i fornitori di Servizi Pubblici e Privati
=============================================================

.. raw:: latex 
   
   \makeatletter\@openrightfalse
	\chapter{Introduzione}

La Carta di Identità Elettronica (CIE), rilasciata dallo Stato italiano, grazie alla presenza di un chip a radiofrequenze nel quale sono contenuti i dati personali e biometrici del titolare e un certificato digitale di autenticazione, estende il tradizionale concetto di identità fisica, configurandosi come strumento di identità digitale che soddisfa i requisiti massimi di sicurezza. 
Interoperabile anche in ambito europeo, lo schema di autenticazione Entra con CIE,  in analogia a quanto previsto da SPID, realizza un sistema di autenticazione di tipo Single Sign-On (SSO) federato che consente ad un utente di effettuare un'unica autenticazione valida per più servizi appartenenti anche a domini differenti per i quali è abilitato. 
Si basa sul protocollo SAML v2 (Security Assertion Markup Language) per il profilo "Web Browser SSO" (`SAML V2.0 Technical Overview <http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html>`__) dal quale eredita gran parte dei requisiti tecnici.
Lo scopo del presente documento è quello di definire le specifiche tecniche per l'integrazione di Entra con CIE come schema di autenticazione per l'accesso ai servizi in rete erogati da PP.AA. e privati.


.. toctree::
   :maxdepth: 3
   :caption: Indice dei contenuti
   :numbered:

   overview.rst
   federazione.rst
   protocolli.rst
   binding.rst
   ciespid.rst
   cieIDSDK.rst
   testing.rst   
   tracciature.rst
   assistenza.rst
   pki.rst

.. raw:: latex

   \includepdf[landscape=true,pages=-]{./media/cie_codici_errori.pdf}
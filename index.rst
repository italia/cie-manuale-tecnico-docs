=============================================================
Manuale tecnico per i fornitori di Servizi Pubblici e Privati
=============================================================

.. raw:: latex 

   \makeatletter\@openrightfalse
	\chapter{Introduzione}

La Carta di Identità Elettronica (CIE), rilasciata dallo Stato italiano, grazie alla presenza di un chip a radiofrequenze nel quale sono contenuti i dati personali e biometrici del titolare e un certificato digitale di autenticazione, estende il tradizionale concetto di identità fisica, configurandosi come strumento di identità digitale che soddisfa i requisiti massimi di sicurezza. 
Interoperabile anche in ambito europeo, lo schema di identificazione "Entra con CIE",  in analogia a quanto previsto da SPID, realizza un sistema di autenticazione federato per l'identificazione dei cittadini presso i soggetti pubblici e privati che aderiscono allo schema. 
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
   codiciErrore.rst


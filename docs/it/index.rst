
=============================================================
Manuale tecnico per i fornitori di Servizi Pubblici e Privati
=============================================================

.. raw:: latex 

   \makeatletter\@openrightfalse
	\chapter{Introduzione}

La Carta di Identità Elettronica (CIE), rilasciata dal Ministero
dell'Interno, grazie alla presenza di un chip a radiofrequenze nel quale
sono contenuti i dati personali e biometrici del titolare e un
certificato digitale di autenticazione, estende il tradizionale concetto
di identità fisica e si configura come uno strumento di identità
digitale per l'accesso ai servizi in rete andando a costituire il
principale cardine dello schema di identificazione digitale “Entra con
CIE”. Interoperabile anche in ambito europeo, lo schema di
identificazione «Entra con CIE» realizza un sistema di autenticazione
federato per l'identificazione dei cittadini presso i soggetti pubblici
e privati che erogano servizi digitali in rete.

Si basa sia sul protocollo `SAML v2 <http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html>`__ (Security Assertion Markup Language)
con profilo «Web Browser SSO» (si veda `Regole Tecniche CIE id SAML <https://docs.italia.it/italia/cie/cie-eid-saml-docs/it/master/index.html>`__)
dal quale eredita gran parte dei requisiti tecnici, sia sul protocollo
OpenID Connect (si veda `Regole Tecniche SPID/CIE OpenID Connect <https://docs.italia.it/italia/spid/spid-cie-oidc-docs/it/versione-corrente/index.html>`__\), quest'ultimo di più recente adozione. I soggetti
pubblici e privati che aderiscono allo schema possono scegliere uno o
l'altro protocollo secondo le loro necessità.

Lo schema di autenticazione “Entra con CIE” offre diversi meccanismi di accesso secondo
il livello di sicurezza richiesto dal particolare servizio a cui
l'utente accede ed in particolare:

1) Un livello di accesso cosiddetto “\ *basso*\ ” (livello 1), che
   prevede l'impiego di credenziali username/password attivabili dal
   titolare di CIE mediante l'area riservata del portale
   `www.cartaidentita.it <http://www.cartaidentita.it>`__, previa
   certificazione di indirizzi di contatto (e-mail e cellulare);

2) Un livello di accesso cosiddetto “\ *significativo*\ ” (livello 2),
   che prevede l'impiego di un secondo fattore di autenticazione o di un
   meccanismo di autenticazione che certifichi il possesso di un
   dispositivo;

3) Un livello di accesso cosiddetto “\ *alto*\ ” (livello 3), che
   prevede l'utilizzo della CIE e del certificato
   digitale di autenticazione a bordo di esso, congiuntamente con il PIN
   della carta.

In ultimo, “Entra con CIE” è utilizzabile tanto da una postazione di
tipo “Desktop” attraverso il browser e, all'occorrenza, un lettore di
smart card a radio frequenza (solo livello 3), quanto da uno smartphone
in mobilità, eventualmente combinando le due modalità di accesso, come
meglio spiegato nel corso del presente documento.

Lo scopo del presente documento è appunto quello di descrivere i
principi di funzionamento dello schema “Entra con CIE” nelle varie
casistiche, rimandando poi alle specifiche tecniche finalizzate
all'integrazione per l'accesso ai servizi in rete erogati da PP.AA. e
privati, secondo i vari protocolli.



.. toctree::
   :maxdepth: 3
   :caption: Indice dei contenuti
   :numbered:

   cieIDSDK.rst
   saml.rst
   oidc.rst

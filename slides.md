<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

# CERTIFICATE TRANSPARENCY DEPLOYMENT STUDY
<!-- .element: class="no-toc-progress" --> <!-- slide not in toc progress bar -->

## CT-Unterstützung von Webservern

[Theodor Nolte](https://github.com/theno) | 2018-01-10 | [online][1] | [1page][2] | [src][3] | [pdf][4]

[Abstract][5] | __[Ausarbeitung][6]__

[1]: https://theno.github.io/presi-ct-deployment
[2]: https://github.com/theno/presi-ct-deployment/blob/master/slides.md
[3]: https://github.com/theno/presi-ct-deployment
[4]: https://github.com/theno/presi-ct-deployment/blob/master/presi-ct-deployment.pdf
[5]: https://inet.haw-hamburg.de/events/inet-seminar/theodor-nolte-certificate-transparency
[6]: https://github.com/theno/presi-ct-deployment/blob/master/certificate-transparency-deployment-study.pdf

----  ----

# Grundlagen

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

## Web-PKI

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

![](img-epgz/web-pki-2.svg)

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

![](img-epgz/web-pki-3.svg)

----

## Web-PKI

* CA-Browser-Forum
* CAB-Requirements

----

## Certificate Transparency (CT)

* [RFC-6962](https://tools.ietf.org/html/rfc6962)

* Chromium / Chrome:
  * Verbindlich für EV-Zertifikate seit: [2015-01-01](https://a77db9aa-a-7b23c8ea-s-sites.googlegroups.com/a/chromium.org/dev/Home/chromium-security/root-ca-policy/CTPolicyMay2016edition.pdf?attachauth=ANoY7cp2dlvPbgZ6YD-YTWUA_l1pDYHIewPwTcvgTz70Ho16dpRjebQKqYNfTqrxBkxE4m0Dt_Hbr8K2MzSGtrgxBUFr5A3-tupukGUVDwjhATVRpkpIhlrzD1xeejeA2es-NJzbT4AZvk1WF_07TZTlXoWm7zGqGFP5d30kIMWrqyBpFES9LhNL_wx0juIUlVWiG5dqpFGKL8jF2GFW9dXoaHhEiL2zHN56UVO-ka5gn1cO_T94oJjn10LAg7eumyQkM_rp_8Zmx7_VWWO-_1q9-tBc8Xf7Xw%3D%3D&attredirects=0)
  * Verbindlich für alle Webserver-Zertifikate ab: [2018-04-01](https://groups.google.com/a/chromium.org/forum/#!msg/ct-policy/sz_3W_xKBNY/6jq2ghJXBAAJ)

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

### Idee: Öffentliche Kontrolle

* Alle Zertifikate werden in CT-Logs veröffentlicht
* Jeder kann Zertifikate einsehen

Bedingung:

* CT-Logs können nicht manipuliert werden

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

![](img-epgz/certificate_transparency_components_trust_in_publicity.svg)

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

![](img-epgz/certificate_transparency_components_interaction_with_log_answer.svg)

----

### Merkle Audit Proof

Certificate published?

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

![](img-ep/certificate_transparency_merkle_tree_audit_proof_mth.svg)

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

### Merkle Consistency Proof

Log works correctly?

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

![](img-ep/certificate_transparency_merkle_tree_consistency_proof_both_mth.svg) <!-- .element height="35%" width="35%" -->

----

## Chromium / Chrome

* kein Auditor
* verifiziert SCTs
* mindestens zwei verschiedene CT-Log-Operatoren

----

## Zertifikate Veröffentlichen

----

### Zertifikat ausstellen und verwenden

### Ohne CT

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

![](img-epgz/issue_certificate_classic_no_log.svg)

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

### Zertifikat ausstellen und verwenden

 * by cert | SCT im Zertifikat enthalten
 * by tls  | SCT per TLS-Extension
 * by ocsp | SCTs per OCSP-Stapling

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

![](img-epgz/issue_certificate_sct_x509v3-extension.svg)

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

![](img-epgz/issue_certificate_sct_tls-extension.svg)

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

![](img-epgz/issue_certificate_sct_ocsp-stapling.svg)

----

## Zusammenfassung

* Idee von CT: Öffentliche Kontrolle aller Zertifikate der Web-PKI

* CT-Komponenten
  * Log: Manipulation verhindert durch Merkle Tree
  * Monitor -> Consistency Proof (CT-Log okay?)
  * Auditor -> Audit Proof (Zertifikat in CT-Log enthalten?)

----

## Zusammenfassung

* Zertifikat veröffentlichen
  * SCT | "Veröffentlichungsquittung"

* Webseiten-Aufruf / TLS-Handshake
  * by cert
  * by tls
  * by ocsp

----  ----

# Vorgehen und Implementierung

----

## Vorgehen

* Domain-Namen: Alexa Top-1M (w/wo. 'www'-Präfix)
* TLS-Handshake: Webserver-Zertifikate abrufen
* SCTs abrufen


----

## Implementierung

----

## [ctutlz](https://github.com/theno/ctutlz)

* TLS-Handshake, "ernten":
  * Zertifikat / Chain
  * SCTs

* Verify der SCTs

----

## ctutlz

* Mühsame Umsetzung / Viele Fehlschläge
  * Modul 'ssl' aus Python-stdlib
  * m2crypto
  * pyOpenSSL
  * OpenSSL

----

## ctutlz

* Erste Implementierung in Python
  * OpenSSL nativ als Lib

* Verwendet pip-packages
  * pyopenssl / cryptography / cffi
  * pyasn1 / pyasn1-modules

* Verify von SCTs: Ducktyping von pyopenssl

----

## ctstats

* Erhebung: Über Alexa-Domain-Liste iterieren
* Ergebnisse in DB speichern
* Auswertung erstellen

----

## ctstats

Limitierungen:
* Maximale Anzahl Threads
* RAM
* Platten-Schreibgeschwindigkeit

----

## ctstats

* Iterieren - Wegschreiben - Iterieren - Wegschreiben - ...
* Thread-Pool Prozesse
* Anzahl:
  * Chunk-Size je Iteration: 10000
  * Thread-Pool Prozesse: 2000
    * max. 150 gleichzeitig
  * Threads: 5
  * RAM-Limit: 60 Prozent

----

## ctstats

* 4 GB RAM
* "normale" HD
* 33:12 h benötigt für einen Durchlauf
* 9273 MB in DB geschrieben

----  ----

# Ergebnisse

----

## Erhebung

* Ende September
  * Alexa-Top1M-List vom 2017-09-28
  * Scrape: vom 28ten bis 29ten

* kein signifikanter Unterschied zwischen w/wo. www-Präfix

----

## TLS Handshake Tries

|                     |  count  | percent |
| :------------------ | ------: | ------: |
| all                 | 2000000 |  100.00 |
| timeout             |  223868 |   11.19 |
| no certificate      |  446313 |   22.32 |
| certificate (no ev) | 1283926 |   64.20 |
| ev certificate      |   45893 |    2.29 |

----

## TLS Handshake Tries

![](img-svg/2017-09-28/stackplot-tls-handshake.svg)

----

## TLS Handshakes

![](img-svg/2017-09-28/stackplot-tls-handshake-no-timeout.svg)

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

## Certificates with or without SCTs

|                |  count  | percent |
| :------------- | ------: | ------: |
| all            | 1329819 |  100.00 |
| no SCTs        |  990221 |   74.46 |
| 1 or more SCTs |  339598 |   25.54 |
| 1 SCT          |      53 |    0.00 |
| 2 SCTs         |  111717 |    8.40 |
| 3 SCTs         |  130358 |    9.80 |
| 4 SCTs         |   63970 |    4.81 |
| 5 or more SCTs |   33500 |    2.52 |

----

## Certificates with or without SCTs

![](img-svg/2017-09-28/stackplot-certificate-with-or-without-scts.svg)

----

## Certificates with SCTs

![](img-svg/2017-09-28/stackplot-certificate-with-scts.svg)

----

## EV-Certificates with or without SCTs

![](img-svg/2017-09-28/stackplot-certificate-ev-with-or-without-scts.svg)

----

## EV-Certificates with SCTs

![](img-svg/2017-09-28/stackplot-certificate-ev-with-scts.svg)

----

## Verifications of SCTs

|                           |  count  | percent |
| :------------------------ | ------: | ------: |
| all                       | 1038227 |  100.00 |
| verified                  | 1015784 |   97.84 |
| unverified; ctlog known   |   22401 |    2.16 |
| unverified; ctlog unknown |      42 |    0.00 |

----

## SCTs by Deliver Way

|                  |  count  | percent |
| :--------------- | ------: | ------: |
| all              | 1038227 |  100.00 |
| by-cert          |  865852 |   83.40 |
| by-tls-extension |  172099 |   16.58 |
| by-ocsp-response |     276 |    0.03 |

----

## SCTs by deliver way

![](img-svg/2017-09-28/stackplot-sct-by-deliver-way.svg)

----

## SCTs of EV-Certificates by Deliver Way

|                  | count  | percent |
| :--------------- | -----: | ------: |
| all              | 134205 |  100.00 |
| by-cert          | 133932 |   99.80 |
| by-tls-extension |    141 |    0.11 |
| by-ocsp-response |    132 |    0.10 |

----

## SCTs of EV-Certificates by Deliver Way

![](img-svg/2017-09-28/stackplot-sct-by-deliver-way-ev.svg)

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

## SCTs by CT-Log

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

![](img-svg/2017-09-28/bar-sct-by-ctlog.svg) <!-- .element height="90%" width="90%" -->

----

## SCTs by CT-Log

![](img-svg/2017-09-28/stackplot-sct-by-ctlog.svg)

----

## SCTs by CT-Log-Operator

![](img-svg/2017-09-28/bar-sct-by-log-operator.svg)

----

## SCTs by CT-Log-Operator

![](img-svg/2017-09-28/stackplot-sct-by-log-operator.svg)

----

## Certificates with SCTs by CT-Log

![](img-svg/2017-09-28/stackplot-certificate-with-scts-by-ctlog.svg)


----  ----

# Zusammenfassung und Ausblick

----

## Zusammenfassung

* 75 % der HTTPs-Webseiten unterstützen kein CT
* SCT by-OCSP wird praktisch nicht eingesetzt
* Diversität verbesserungswürdig
* Google dominiert und setzt durch:
  * Entwicklung / RFCs
  * Deployment / Chrome

----

## Ausblick

* Ab April 2018: CT erforderlich unter Chromium / Chrome
  * Zeitlicher Verlauf vom Deployment

* Weitere Untersuchungen
  * Details, z.B. verify-fails by-cert
  * Kombinationen der SCTs-Auslieferungsmechanismen

* Abgleich: Zertifikate in CT-Logs veröffentlicht, zu TLS-Handshakes
  ohne CT-Unterstützung

----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

## References

* RFC-6962: https://tools.ietf.org/html/rfc6962

* CT-"Homepage": https://www.certificate-transparency.org/
  * Known CT-Logs: [./known-logs](https://www.certificate-transparency.org/known-logs)
  * Open-Source Tools, Libs: [./libs](https://www.certificate-transparency.org/libs)

* CT in Chromium: https://www.chromium.org/Home/chromium-security/certificate-transparency

---

* ctutlz: https://github.com/theno/ctutlz

* openssl-examples: https://github.com/theno/openssl-examples

* pyopenssl-examples: https://github.com/theno/pyopenssl-examples


----  ----

<!-- .slide: data-state="no-toc-progress" --> <!-- don't show toc progress bar on this slide -->

### *Thank You for Your attention!*
<!-- .element: class="no-toc-progress" -->

![](img/bird.jpg)

---

__[revealjs_template](https://theno.github.io/revealjs_template) <-- ***check this out***__

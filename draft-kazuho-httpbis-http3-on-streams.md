---
title: "HTTP/3 over QMux"
docname: draft-kazuho-httpbis-http3-on-streams-latest
category: std
wg: httpbis
ipr: trust200902
keyword: internet-draft
stand_alone: yes
pi: [toc, sortrefs, symrefs]
author:
-
    fullname: Kazuho Oku
    organization: Fastly
    email: kazuhooku@gmail.com
-
    fullname: Lucas Pardue
    organization: Cloudflare
    email: lucas@lucaspardue.com
-
    fullname: Jana Iyengar
    organization: Fastly
    email: jri.ietf@gmail.com

normative:
  HTTP: RFC9110
  RFC9112:
    display: HTTP/1.1
  RFC9114:
    display: HTTP/3
  QMUX: I-D.ietf-quic-qmux-01

informative:
  RFC9113:
    display: HTTP/2

--- abstract

This document specifies how to use HTTP/3 over QMux.


--- middle

# Introduction

As of 2023, HTTP/2 {{RFC9113}} remains the most widely used version of
HTTP across the Internet, although the adoption rate of HTTP/3
{{RFC9114}} is increasing rapidly.

HTTP/3 has several advantages over HTTP/2, primarily due to its use of QUIC
{{?QUIC=RFC9000}} as the transport layer protocol, which provides features like
stream multiplexing without head-of-line blocking and low-latency connection
establishment.

However, given that QUIC's availability and accessibility are not as universal
as TCP's, a complete migration of all HTTP/2 traffic to QUIC-based HTTP/3 seems
unlikely.

This situation necessitates HTTP deployments to support both transport protocols
and their respective HTTP versions for the foreseeable future.

Maintaining dual support is costly, as the two protocols differ in many aspects
such as wire-encoding, flow control and stream multiplexing machinery, and HTTP
header compression. Extensions operating at the HTTP wire encoding layer must
be developed and implemented for both HTTP/2 and HTTP/3, and both protocol
stacks require ongoing maintenance to address bugs, performance issues, and
vulnerabilities.

To address this redundancy, this specification defines the method of running
HTTP/3 over QMux version 1 {{QMUX}}, which enables the operation of HTTP/3 over
TCP and TLS without any modifications.

Consequently, design, implementation, and maintenance efforts can concentrate on
a single HTTP version: HTTP/3.


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# The Protocol

HTTP/3 functions over QUIC version 1, employing the set of operations (i.e.,
API) defined in {{Section 2.4 and Section 5.3 of QUIC}}. HTTP/3 over QMux
version 1 utilizes the same set of operations that are available in QMux.


# Starting HTTP/3 over QMux

HTTP/3 over QMux version 1 can be used for “https” URI schemes defined in
{{Section 4.2 of HTTP}} with the same default port number as HTTP/1.1
{{RFC9112}}.

When a client uses HTTP/3 over QMux for an "https" URI scheme, it uses TLS
{{!TLS=RFC8446}} with ALPN {{!ALPN=RFC7301}} as described in {{Section 8 of
QMUX}}. The ALPN ID for the final, published RFC is "h3qx". Until such
an RFC exists, implementations MUST NOT identify themselves using this string.

## Draft Version Identification

\[\[RFC editor: please remove this section before publication.]]

This draft describes HTTP/3 over draft version of QMux. In order to support
interoperability over revisions, HTTP/3 over QMux drafts define an ALPN ID that
captures both this document revision and the QMux revision in use.

The ALPN ID defined by this draft revision is "h3qx-01", which represents HTTP/3
over draft-ietf-quic-qmux-01.


# Security Considerations

TODO Security


# IANA Considerations

If this document is adopted and published as an RFC, it will have an action to
create a new registration for the identification of HTTP/3 over QMux version 1
in the "TLS Application-Layer Protocol Negotiation (ALPN) Protocol IDs" registry
established in {{ALPN}}.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.

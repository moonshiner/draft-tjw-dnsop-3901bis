```




DNSOP                                                  T. Wicinski. (ed)
Internet-Draft                                              22 July 2023
Obsoletes: 3901 (if approved)                                           
Intended status: Best Current Practice                                  
Expires: 23 January 2024


               DNS IPv6 Transport Operational Guidelines
                       draft-tjw-dnsop-3901bis-00

Abstract

   This memo provides guidelines and Best Current Practice for operating
   DNS in a world where queries and responses are carried in a mixed
   environment of IPv4 and IPv6 networks.

Status of This Memo

   This Internet-Draft is submitted in full conformance with the
   provisions of BCP 78 and BCP 79.

   Internet-Drafts are working documents of the Internet Engineering
   Task Force (IETF).  Note that other groups may also distribute
   working documents as Internet-Drafts.  The list of current Internet-
   Drafts is at https://datatracker.ietf.org/drafts/current/.

   Internet-Drafts are draft documents valid for a maximum of six months
   and may be updated, replaced, or obsoleted by other documents at any
   time.  It is inappropriate to use Internet-Drafts as reference
   material or to cite them other than as "work in progress."

   This Internet-Draft will expire on 23 January 2024.

Copyright Notice

   Copyright (c) 2023 IETF Trust and the persons identified as the
   document authors.  All rights reserved.

   This document is subject to BCP 78 and the IETF Trust's Legal
   Provisions Relating to IETF Documents (https://trustee.ietf.org/
   license-info) in effect on the date of publication of this document.
   Please review these documents carefully, as they describe your rights
   and restrictions with respect to this document.  Code Components
   extracted from this document must include Revised BSD License text as
   described in Section 4.e of the Trust Legal Provisions and are
   provided without warranty as described in the Revised BSD License.





Wicinski. (ed)           Expires 23 January 2024                [Page 1]

Internet-Draft                   3901bis                       July 2023


Table of Contents

   1.  Introduction  . . . . . . . . . . . . . . . . . . . . . . . .   2
     1.1.  Name Space Fragmentation: following the referral chain  .   2
   2.  Terminology . . . . . . . . . . . . . . . . . . . . . . . . .   3
   3.  Policy Based Avoidance of Name Space Fragmentation  . . . . .   3
   4.  DNS IPv6 Transport recommended Guidelines . . . . . . . . . .   4
   5.  IANA Considerations . . . . . . . . . . . . . . . . . . . . .   4
   6.  Security Considerations . . . . . . . . . . . . . . . . . . .   4
   7.  Normative References  . . . . . . . . . . . . . . . . . . . .   4
   8.  Informative References  . . . . . . . . . . . . . . . . . . .   5
   Acknowledgements  . . . . . . . . . . . . . . . . . . . . . . . .   5
   Author's Address  . . . . . . . . . . . . . . . . . . . . . . . .   5

1.  Introduction

   When the Internet moves from IPv4 to a mixture of IPv4 and IPv6 it is
   only a matter of time until this starts to happen.  The complete DNS
   hierarchy then starts to fragment into a graph where authoritative
   name servers for certain nodes are only accessible over a certain
   transport.  The concern is that a resolver using only a particular
   version of IP and querying information about another node using the
   same version of IP can not do it because somewhere in the chain of
   servers accessed during the resolution process, one or more of them
   will only be accessible with the other version of IP.

1.1.  Name Space Fragmentation: following the referral chain

   A resolver that tries to look up a name starts out at the root, and
   follows referrals until it is referred to a name server that is
   authoritative for the name.  If somewhere down the chain of referrals
   it is referred to a name server that is only accessible over a
   transport which the resolver cannot use, the resolver is unable to
   finish the task.

   With all DNS data only available over IPv4 transport everything is
   simple.  IPv4 resolvers can use the intended mechanism of following
   referrals from the root and down while IPv6 resolvers have to work
   through a "translator", i.e., they have to use a recursive name
   server on a so-called "dual stack" host as a "forwarder" since they
   cannot access the DNS data directly.

   With all DNS data only available over IPv6 transport everything would
   be equally simple, with the exception of IPv4 recursive name servers
   having to switch to a forwarding configuration.






Wicinski. (ed)           Expires 23 January 2024                [Page 2]

Internet-Draft                   3901bis                       July 2023


   However, the second situation will not arise in the foreseeable
   future.  Instead, the transition will be from IPv4 only to a mixture
   of IPv4 and IPv6, with three categories of DNS data depending on
   whether the information is available only over IPv4 transport, only
   over IPv6 or both.

   Having DNS data available on both transports is the best situation.
   The major question is how to ensure that it becomes the norm as
   quickly as possible.  However, while it is obvious that some DNS data
   will only be available over v4 transport for a long time it is also
   obvious that it is important to avoid fragmenting the name space
   available to IPv4 only hosts.  For example, during transition it is
   not acceptable to break the name space that we presently have
   available for IPv4-only hosts.

2.  Terminology

   The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
   "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and
   "OPTIONAL" in this document are to be interpreted as described in BCP
   14 [RFC2119] [RFC8174] when, and only when, they appear in all
   capitals, as shown here.  DNS terminology is as described in
   [RFC8499].

   The phrase "IPv4 name server" indicates a name server available over
   IPv4 transport.  It does not imply anything about what DNS [RFC1034]
   [RFC1035] data is served.  Likewise, "IPv6 name server" [RFC2460]
   [RFC3513] [RFC3596] indicates a name server available over IPv6
   transport.  The phrase "dual-stack name server" indicates a name
   server that is actually configured to run both protocols, IPv4 and
   IPv6, and not merely a server running on a system capable of running
   both but actually configured to run only one.

3.  Policy Based Avoidance of Name Space Fragmentation

   Today there are only a few DNS "zones" on the public Internet that
   are available over IPv6 transport, and most of them can be regarded
   as "experimental" (TJW: reword and ref 8499).
   However, as soon as the root and top level domains are available over
   IPv6 transport, it is reasonable to expect that it will become more
   common to have zones served by IPv6 servers.

   Having those zones served only by IPv6-only name server would not be
   a good development, since this will fragment the previously
   unfragmented IPv4 name space and there are strong reasons to find a
   mechanism to avoid it.





Wicinski. (ed)           Expires 23 January 2024                [Page 3]

Internet-Draft                   3901bis                       July 2023


   The recommended approach to maintain name space continuity is to use
   administrative policies, as described in the next section.

4.  DNS IPv6 Transport recommended Guidelines

   In order to preserve name space continuity, the following
   administrative policies are recommended:

   *  every recursive name server SHOULD be dual stack IPv4/IPv6.

   This rules out IPv4-only and IPv6-only recursive servers.
   However, one might design configurations where a chain of IPv6-only
   name server forward queries to a set of dual stack recursive name
   server actually performing those recursive queries.

   *  every DNS zone SHOULD be dual stack IPv4/IPv6.

   A DNS zone can be served by at least one IPv4-reachable authoritative
   name server.

   This rules out DNS zones served only by IPv6-only authoritative name
   servers.

   Note: zone validation processes SHOULD ensure that there is at least
   one IPv4 address record available for the name servers of any child
   delegations within the zone.

5.  IANA Considerations

   None

6.  Security Considerations

   The guidelines described in this memo introduce no new security
   considerations into the DNS protocol or associated operational
   scenarios.

7.  Normative References

   [RFC1034]  Mockapetris, P., "Domain names - concepts and facilities",
              STD 13, RFC 1034, DOI 10.17487/RFC1034, November 1987,
              <https://www.rfc-editor.org/info/rfc1034>.

   [RFC1035]  Mockapetris, P., "Domain names - implementation and
              specification", STD 13, RFC 1035, DOI 10.17487/RFC1035,
              November 1987, <https://www.rfc-editor.org/info/rfc1035>.





Wicinski. (ed)           Expires 23 January 2024                [Page 4]

Internet-Draft                   3901bis                       July 2023


   [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate
              Requirement Levels", BCP 14, RFC 2119,
              DOI 10.17487/RFC2119, March 1997,
              <https://www.rfc-editor.org/info/rfc2119>.

   [RFC2460]  Deering, S. and R. Hinden, "Internet Protocol, Version 6
              (IPv6) Specification", RFC 2460, DOI 10.17487/RFC2460,
              December 1998, <https://www.rfc-editor.org/info/rfc2460>.

   [RFC3513]  Hinden, R. and S. Deering, "Internet Protocol Version 6
              (IPv6) Addressing Architecture", RFC 3513,
              DOI 10.17487/RFC3513, April 2003,
              <https://www.rfc-editor.org/info/rfc3513>.

   [RFC3596]  Thomson, S., Huitema, C., Ksinant, V., and M. Souissi,
              "DNS Extensions to Support IP Version 6", STD 88,
              RFC 3596, DOI 10.17487/RFC3596, October 2003,
              <https://www.rfc-editor.org/info/rfc3596>.

8.  Informative References

   [RFC8174]  Leiba, B., "Ambiguity of Uppercase vs Lowercase in RFC
              2119 Key Words", BCP 14, RFC 8174, DOI 10.17487/RFC8174,
              May 2017, <https://www.rfc-editor.org/info/rfc8174>.

   [RFC8499]  Hoffman, P., Sullivan, A., and K. Fujiwara, "DNS
              Terminology", BCP 219, RFC 8499, DOI 10.17487/RFC8499,
              January 2019, <https://www.rfc-editor.org/info/rfc8499>.

Acknowledgements

   Ack

Author's Address

   Tim Wicinski
   Elkins, WV 26241
   United States of America
   Email: tjw.ietf@gmail.com












Wicinski. (ed)           Expires 23 January 2024                [Page 5]
```

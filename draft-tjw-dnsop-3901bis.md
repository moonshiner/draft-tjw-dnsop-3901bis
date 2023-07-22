
%%%
title = "DNS IPv6 Transport Operational Guidelines"
abbrev = "3901bis"
docName = "@DOCNAME@"
category = "bcp"
obsoletes = [3901]
ipr = "trust200902"
area = "Ops"
workgroup = "DNSOP"
submissiontype = "IETF"
keyword = [""]

[seriesInfo]
name = "Internet-Draft"
value = "@DOCNAME@"
stream = "IETF"
status = "bcp"

[[author]]
initials = "T."
surname = "Wicinski. (ed)"
fullname = "Tim Wicinski"
  [author.address]
  email = "tjw.ietf@gmail.com"
  [author.address.postal]
  city = "Elkins"
  region = "WV"
  code = "26241"
  country = "USA"


%%%

.# Abstract


This memo provides guidelines and Best Current Practice for operating
DNS in a world where queries and responses are carried in a mixed
environment of IPv4 and IPv6 networks.

{mainmatter}


# Introduction

The Internet is well on its way to a mixture of IPv4 and IPv6 networkd.
The concern is that a resolver using only a particular
version of IP and querying information about another node using the
same version of IP can not do it because somewhere in the chain of
servers accessed during the resolution process, one or more of them
will only be accessible with the other version of IP.

## Name Space Fragmentation: following the referral chain

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

Instead, the transition will be from IPv4 only to a mixture
of IPv4 and IPv6, with three categories of DNS data depending on
whether the information is available only over IPv4 transport, only
over IPv6 or both.

Having DNS data available on both transports is the optimal situation.


# Terminology {#terminology}

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this
document are to be interpreted as described in BCP 14 [@!RFC2119] [@RFC8174]
when, and only when, they appear in all capitals, as shown here.
DNS terminology is as described in [@?RFC8499].

The phrase "IPv4 name server" indicates a name server available over
IPv4 transport.  It does not imply anything about what DNS 
[@!RFC1034] [@!RFC1035] data
is served.  Likewise, "IPv6  name server" 
[@!RFC2460] [@!RFC3513] [@!RFC3596]  indicates a name
server available over IPv6 transport.  The phrase "dual-stack name
server" indicates a name server that is actually configured to run
both protocols, IPv4 and IPv6, and not merely a server running on a
system capable of running both but actually configured to run only
one.

# Policy Based Avoidance of Name Space Fragmentation

Today there are only a few DNS zones on the public Internet that
are available over IPv6 transport, and most of them can be regarded
as "experimental" (TJW: reword and ref 8499).  


Having those zones served only by IPv6-only name server would not be
a good development, since this will fragment the previously
unfragmented IPv4 name space and there are strong reasons to find a
mechanism to avoid it.

The recommended approach to maintain name space continuity is to use
administrative policies, as described in the next section.

# DNS IPv6 Transport recommended Guidelines

In order to preserve name space continuity, the following
administrative policies are recommended:

  *  every recursive name server SHOULD support the local network
configuration.  If the local network supports both IPv4 and IPv5,
the resolver SHOULD be dual stack.


  * every DNS zone SHOULD be dual stack IPv4/IPv6. 
  
  A DNS zone can be served by at least one IPv4-reachable
authoritative name server.

 This rules out DNS zones served only by IPv6-only authoritative
 name servers.

Note: zone validation processes SHOULD ensure that there is at least
one IPv4 address record available for the name servers of any child
delegations within the zone.

# IANA Considerations

None

# Security Considerations

The guidelines described in this memo introduce no new security
considerations into the DNS protocol or associated operational
scenarios.

{backmatter}

{numbered="false"}

# Acknowledgements {#acknowledgements}

Mark Andrews

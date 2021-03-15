*  Session 2 is 1300-1500 UTC+1 Thursday March 11, 2021
---

# Session 1 Agenda

## Welcome

* [NOTE WELL](https://www.ietf.org/about/note-well.html)
* Scribe selection (Chris Lemmons is taking notes)
* Agenda bashing
* Chairs & AD Comments

## Issues from Drafts

### Discovery of Designated Resolvers (DDR) [draft-ietf-add-ddr](https://datatracker.ietf.org/doc/draft-ietf-add-ddr/)
Presented by Tommy Jensen

**Issue #2**
Benjamin Schwartz: Why are we discussing hints at all?
Tommy Jensen: In order to reduce the round trip time. Also, because if it is not encrypted, it is subject to attack.
Benjamin Schwartz: We should give that more thought. I think it's only an issue when it's empty.

**Issues #3 & #4**
Benjamin Schwartz: For #4, I thought we had consensus that we don't need multiple name checks on a single cert.
Tommy Jensen: The need for both is laid out. I don't recall the consensus.
Benjamin Schwartz: I disagree with this. I proposed a substantial change to the draft and this behaviour. We'll talk about it more later.
Tommy Jensen: We could talk about it now.
Benjamin Schwartz: Over all, I think the draft contains some incorrect analysis aobut the location of the trust anchors. As a result, I think it comes to the wrong conclusion about cross-ip upgrade. I think we can handle this without changing the fundamental structure, but there are some changes I'd like.
ekr: (Audio cut out for the scribe.) The minimal requirement here is for the hash to contain the name that I started with and a name different than the name I started with.
Benjamin Schwartz: We need to be clear about what information we use as our starting point. That's the main thing we need to be checking.
ekr: As I understand it, I start with example.com and I won't trust any information that I get that doesn't demonstrate that it comes from example.com. I don't mind it having both names, but I don't think it has to have it for every address.
Glenn: So, I think Ben is bringing up good considerations. I think it might be useful if you tried to capture the thinking about the order of these things. If we can capture that in this or a separate document, it might be a very valuable thing long term as people look back.
Tommy Pauly: We should definitly iterate on this. As an example, if a user typed in 1.1.1.1 as their preferred DNS, that's all we know to start with. If that, for some reason that points me to a v6 address that serves quad-1, I want to make sure I can cover the address I started with. If we're doing DOH and we're having to look for cloudflare.com, it seems odd to tell the client NOT to check that the certificate is valid for cloudflare.com.
Benjamin Kaduk: In general, when you're doing TLS, you make a connection with an initial identifier and you have to determine if that cert matches the name you're trying to contact. You can try to say that the IP address is the authority and that makes some sense, but it isn't a great fit. I think it's going to be pretty valuable that the core model is to authenticate the initial connection.
Tommy Jensen: There are two identities being confirmed. Consider: an initial DNS and initial DOH can be different. I need to make sure the TLS cert can contain the DOH connection and also that it confirms the initial DNS connection. Otherwise, an attacker can redirect you to a DOH server they control.
ekr: There are two ways to think about it: cname or 302. On a cname, you expect the authority and cert to match. On a 302, you expect a first authority that you confirm and then you expect the second authority to match the second target. If we're thinking about it like the latter, it's odd to think about the cert for the second as part of the first.
Benjamin Schwartz: Putting a second name in the certificate breaks the HTTP abstraction. If we only have the one name, we can pass it off to an HTTP stack and it already knows how to validate that the name and the certificate matches. If we require multiple names, it gets confusing and we can no longer plug it into a stock stack.There's an analogy to how we use CDNs. We don't ask clients to validate both the authority and the CDN.
Daniel Gillmor: I'm not convinced that our stock HTTP stacks are actually good at checking names. But I agree that asking for multiple name checking is odd. What's the gain for having to check both names?
Tommy Pauly: If all you're validating is that the IP address is valid, you have to treat the authority as the IP address. And that's ok. But we could say that from the DDR perspective, validating only the IP is acceptable. But we could have an additional option there. (Scribe missed some context.)
Eric Orth: The one case where it would be really useful to have this name is when a client wants to verify that there's a whitelist. But then that whitelist could be used for the upgrade anyways. At the least, it's reasonable to say that clients should check against the name. I think the IP gives us all the security properties we need.
Ralf Weber: I agree that we should use a single trust anchor. In section 5, we say if there are 2 targets, we check the source and dest. But if we start with a name, we should check the name. If we start with an IP we should check the IP.
Tommy Jensen: I'll update the issue and take the rest to the list and github.

**Issues #6 and #7**
Benjamin Schwartz: This is a fundamental scope question: Do we want to try to enable encrypted DNS through legacy customer equipment. If I have a forwarder pointed to my ISP's resolve and all it does is forward, can I or can I not get an encrypted connection to my ISP without replacing my access point. Lots of people are asking for that, but the draft DOES NOT support this use case. I believe it is possible to enable a secure upgrade and I've written the text. But it's up to the WG to decide if we want to include it in the scope.
Tommy Jensen: I think we'll touch on this in the next slide. But right now, the scenario is out of scope because we don't have a solution.

**Issues #8 & #9**
Eric Orth: Continuing the scope discussion.... It's in a grey area that might be ok for some client's desires and not others. Maybe we should make it an optional case and document it in the draft. Some clients might not accept it, but ecosystem changes might make future clients more willing to support it.

**Final Comments**
Daniel Migault: I had a question about the use of a special domain. Since those domains break DNSSEC and can't be protected by it, I wonder if we could use a standard name that is part of the DNS heirarchy. We should consider addressing that.
Tommy Jensen: this is an interesting proposal. I think the existing proposal will get better adoption. What you're proposing is possible. Raise the issue on GitHub, please.
Benjamin Schwartz: I think the semantic here doesn't match the way we're using it. (Scribe missed some context.)

### DHCP and Router Advertisement Options for the Discovery of Network-designated Resolvers (DNR) [draft-ietf-add-dnr](https://datatracker.ietf.org/doc/draft-ietf-add-dnr/)
Presented by Mohamed Boucadair

**Slide 3**
ekr: Why aren't you just putting in an SVCB record. Why don't we define a common piece of information and put it both places. The information you need here is the same as is in the DDR.
M: That is certainly one option.
ekr: I'm saying put the information in there directly.
M: That's true for the IP addresses and the port number. You can use that directly.
ekr: But there's more stuff in SVCB than the IP and port number
M: We'll discuss that in another slide. Here, we're providing only the minimal information.
ekr: I am disagreeing with that. Let's not reinvent there.
David Lawrence: Let's raise this as an issue.

**Slide 6**
Schwartz: My preference would be to just provide the name and let the client do DDR with an insecure resolver. This costs some round trips, but it's only on a setup and they're to a local resolver that should be fast. It's only on network connect. It's also operationally preferable in many cases. If I'm on a CP controlled network using the ISPs DNS. If all the information is hardcoded in DHCP, they have to be updated all at once. There's operational value in putting them together. This simplifies the formats.
Jensen: The RTTs are one concern, but security is another. (Scribe missed some context.)
Schwartz: A DNS attacker can't do anything but deny you service.
Pauly: I agree with Ben on this one. Having the name is enough for security. the worst case scenario is that a client would know that encrypted DNS is available and that thye are under attack. Just having the name is enough for that. The overall style of approach is used in VPNs and IPSEC. (Scribe: not sure?) I think it's cleaner to have the name there and let everything else just work. If we have a new DNS encryption type in the future, it would be nice if I just have to update my resolver. If we go another way, we have to propagate the protocol information every time it changes.
ekr: I think Pauly is making a good point. There are two coherent positions: have the name and do DDR or stuff all the DDR information in.
Tirumaleswar Reddy.K: We started with just the name, but we were concerned about DNS attacks modifying responses. But if DNSSEC is required, then we could have just the name. (Scribe: missed context)
Mohamed: I'm hearing the comments. We started with one option and as we progressed we were hearing that the WG is sensitive to the security attacks, internal and external. And there's a tradeoff on the attacks. This is really deployment specific. If there's no list of IP addresses retuned to the client, we have a discussion... see slide 8. We need more discussion and input on this topic.
Daniel Gillmor: I'm confused. The distinction is just a name or full DNS records. If you put just names, then you have a problem linking them to which IP addresses are in use. If you use whole DNS records, you get linkage between IP addresses and domain names.
Schwartz: To answer Tiru: see DDR section 5. It specifies how to connect when you start with an auth name. Even if you have forged DNS records, you do TLS on the end server. If you have insecure DNS to get there, you still auth the end server. To move IP addresses around and if each named resovler is associated with a bag of records, we don't have a problem.
Tiru: If an attacker modifies the DNS record, they can force the client to use a problematic resolver.

Thanks to Chris Lemmons for taking notes.


## Discussion (if time permits)

Time did not permit.

## Planning & Wrap Up

* Session 2 planning

---

---
# Session 2 Agenda

Discussion began with Glenn discussing the coffee kicking in.
## Welcome

* [NOTE WELL](https://www.ietf.org/about/note-well.html)
* Scribe selection (Eliot)
* Agenda bashing
* Chairs & AD Comments

New AD: Éric Vyncke said a very quick "hi", and thanked Barry for helping in the formation and oversight of the WG.

There was the threat of one of the chairs breaking "into tune".  We then got a history of Dave's HS musicals (Oklahoma, Mame, ...).

## Continued discussion from Session 1

### DNR Discussion continued

Mo continued presenting (Slide 2).  There were two different flows: with and without DNSSEC.
Some confusion was expressed in the Jabber room on this slide.

{"Tommy" below is Tommy Pauly}

Tommy: why should DNSSEC matter?  If we have the name then we can do a secure upgrade, apart from a DoS.
Mo: If DNSSEC is not supported, then someone can redirect you to a fake server.
Tommy: If I have the name from DHCP and I am trusting DHCP more than DNS, unless the name is fundamentally compromised...
Mo: The scenario is where the DNS is spoofed with parameters that the client does not support; and it becomes a DoS.
Tommy: there are many ways to do a DoS.
Mo: This avoids the case where the DoS isn't detected.
Tommy: As a client, if I received information in which I failed to connect, I would treat it as an attack; and we should mandate that.
EKR: The assumption is that whatever you got in this message was authentic.  Server has no way of knowing the client capabilities.  Server has to provide client something it can accept.
Mo: The server doesn't need to be aware of the capabilities of the client.
EKR: Just have one version, for when the client doesn't support DNSSEC.  Do you believe the security properties of these two arms are the same?
Mo: Yes
EKR: then let's just have the second arm.
Mo: Proposal could just stick to the later.
EKR: We are adding tiny little pieces in SVCB into this proposal.  Let's bring all of SVCB into this work.  If it's too big, use a digest.
tale: Yes Tiru and Med, please give this serious consideration
Ralf Weber: initial draft was simple.  Agrees DHCP should be like that.  But what we're doing now is creating a circular dependency.  Better to stick this information into DHCP.
Ben Schwartz: What happens if you are a DoH-only client, and your network hands you a resolver that says "this resolver supports DoH", and find that it only does "DoT".  Fail open?  Fail closed.  Those options are both bad.
EKR: {went too fast}
Ben: At a minimum, you need the DHCP server to enumerate the set of protocols supported by the server.  If you're going to include that kind of information, you should ship the whole config.
Jim: step back: what is the threat model and what is this particular proposal trying to solve from a threat perspective?
Glenn(no hat): In most cases, these will be legacy devices.  We should keep that portion of things simple.  Don't add a lot of complexity.
Tiru: If you put the SVCB into the a DHCP option, it seems to go against options guidelines to keep things as simple as possible.
Tommy: Could EKR clarify about using a hash in DHCP?  Is it just a validation?  That sounds interesting.
Tiru: that sounds interesting.  ADN + IP addresses + hash.
Tommy: that would be the limit for DHCP or any other protocol.
At this point, sadly EKR could not be heard.
Ben: in terms of management, doesn't think shipping a hash is really pragmatic.  Shipping these records doesn't create a lot of complexity, because they are opaque to the DHCP server.  If the concern is about record size, SVCB is tightly optimized for size.  We need to talk about TTLs.  {Eliot lost his cookies at this point}  Putting those TTLs aside, we could fit this in.
Kirsty: We need a clear threat model.  Would like a lot more discussion about what problem this is solving.
EKR: Hashes may not be necessary
Eliot: The impact of different DHCP/RA lifetimes and DNS TTLs should be considered.

### Slide 3

DoH implementations MUST support DoT.  Updates RFC 8484.

At which point EKR had some pretty serious issues.

Chair: take this all to the list.  At that point we can evalaute the proposal in its entirety.

Mo: ok.





### DNS Server Information with Assertion Token

Tiru presented.

Several revisions to the draft.
A few clarifications for troubleshooting and human readable selection.



#### Issue 1 (Slide 4)

relies on a draft that is expired.

Ben: {trolled version.bind approach} supports using RESINFO RRtype.

#### Issue 2 Resolver Assertion Token -> Separate draft?

Eliot: please change the term to avoid conflict with RATS
Tiru: ok
Glenn: go with a separate draft.
Tiru: too early for adoption

## Split-Horizon DNS Configuration in Enterprise Networks

Tiru spoke to this one as well.

Goals:
    Discover local names.
    Discover that network does split DNS
Scope: client can authenticate the identity of the network.

Mechanism (slide 5): provisioning domains.
Slide 6: PVD example

Issue 1 (slide 7): how does the client know that the network is not lying?

Use NSEC3

If public resolver is not reachable, IKE clients MAY want to require whitelisted domains for TLDs and SLDs.


Glenn: there are three scenarios: doesn't exist in global dns.  someone publishes non-signed version of that domain.  the owner of domain has both internal and external, but the two versions are not aligned.

Tiru: got an issue for this.

Issue 2: network is authortative of the public (Interet-side) domain, but there are different versions.

Feedback requested.

Ben: I like this version, achieves a new version of split-DNS that has new and interesting properties.  Is there client-side interest?  Simple clients don't need this. Simple clients will use their own or what they get from DNR.  The case where it matters is when the client wants to use multiple resolvers.  Are there such clients?

Tommy Pauly: As a client implementor, I'm interested in the problem.  Agree with Ben on the simple case, but would like to move to scenarios where there is a more private stance by default. "This network has special authority over a special domain" is useful.

Tale: room seems to like the draft.  Pushback?  [none]


## We then took a 7 minute break.

And Dave brought us back with a song.

### Open Microphone

Andrew Campling: very encouraging progress.
Tiru: talking about legacy cases where router is doing DNS filtering.  Talking about these use cases would be good.
Glenn: RFC 1918, CGNAT, and ability to support these systems.
Tiru: {lots of stuff about products as to why he's interested in ADD on home routers, especially for IoT devices}
Jim Reid: can we please have a draft first on CGNAT/1918?  And we need more engagement from those people.
Jim Reid: confused about the state about the daunting number of drafts here.
Neil: likes the idea of adding 1918 and CGNAT considerations.  {May not have captured this right}
Tommy Pauly: important to address.  Beware of mandates on retry timers etc.

Chair: Ben, regarding your PR, could you please post to the list a pointer to the PR with some prose around it.
Ben: I did.
Ben: what does it mean to get an authentication anchor from an untrusted source?  The upgrade is secure if you haven't converted a transcient attacker into a persistent attacker.
Ben: Thus repeat the process every five minutes.  But this is not the only hueristic.
Tiru: various ways to identify transcient attacker.
Jim Reid: how should this be incorporated?

{Brief discussion between Andrew and Glenn over the reqs draft}

Tommy: this one just belongs in security considerations for DDR.  What stops the transcient attacker from blocking the message later?  An attacker could tear down encrypted channels.
Ben: I have assumed that any adversary that can selectively drop packets can selectively insert packets, so we're already hosed.
Tommy: then we have to worry about packet loss
Ben: of course there's retry, but need to look at effects of retries.
Ben: could you surface to the user questions
Tommy: we have examples of implementations that have lists.
Eliot: let's see a draft.
EKR: I'm skeptical that we can ask the right question in a safe way.
Daniel: Why is this so difficult?
EKR: Actually determining which network you're on is difficult.
Fred Baker: with EKR on this point.
Tommy: let's not dive into UI
Tiru: legal entities can solve some of this...



## Planning & Wrap up

* Wrap up + Future Planning

Glenn:
No interim meetings yet planned.  Stay tuned.
We have two drafts that are adopted.  One is adopted for informational purposes.
Let's make sure we make a conscious effort to keep discussions on list, rather than just in github issues.

# Meeting Adjourned!

Scribing for session 2 done by Elliot Lear.


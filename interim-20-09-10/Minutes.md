# ADD September 10 2020 Interim Session 1

Chairs:  Glenn Deen, David Lawrence
AD: Barry Leiba
Note takers: Ben Schwartz, with assistance from Martin Thomson
Jabber Scribe: Andrew Campling

Links:
:  [ADD Datatracker](https://datatracker.ietf.org/group/add/about/)
:  [ADD GitHub](https://github.com/ietf-wg-add)

# Agenda

## Note Well

## Agenda bash 

Paul Hoffman: We have more mailing list issues than Github issues.  When are we going to discuss the mailing list issues?
Glenn: We'll deal with those on the next session on Tuesday as we likely have more than enough material for today's session.

## Welcome & Chairs Statement 

## Review & Discuss ADD Requirements 
   1. [Draft-Box-ADD-Requirements-00](https://www.ietf.org/id/draft-box-add-requirements-00.html) (Chris Box) 
   2. [Github Issues](https://github.com/ietf-wg-add/draft-add-requirements/issues)  

## ADD Interim 15 Session 2 (September 15) 

### Draft-Box-ADD-Requirements-00 (presentation, Chris Box)

Chris: Section 3 and 4 are the main areas where you find the use cases.  Section 3 covers the case of going from one resolver to another, and Section 4 considers resolvers that are use for a limited set of names.  Daniel Migault has submitted a Pull Request that would change this organization.

Intro to Section 3: What do we mean by an "Associated Resolver"?  This diagram shows a network with four DNS resolvers.  Each one has a speech bubble stating its behavior (not necessarily on the wire; this is not stating how we perform discovery).  The idea here is the "association principle": if you support encrypted DNS, then my organization also runs these other resolvers that you might want to consider.  The precise things that they say are not necessarily important.  The important thing is that some associated resolvers are considered "equivalent" -- it has exactly the same functionality -- and some are not, e.g. because it knows about additional locally relevant names.

Section 3.1: Network-provisioned resolvers.  In this example, the user initially receives a resolver from the local network by DHCP.  By some means, there can be a referral to a different resolver that supports encrypted DNS.  In this example, the resolver claims "network topology knowledge", e.g. connectivity, latency, cache locations.

Section 3.1.1: There are two types of network-provisioned resolvers in the draft.  This one is the "unencrypted forwarder".  It doesn't perform any encryption itself, but it wants to enable clients to move to the encrypted resolver.

Section 3.1.2: Encrypted forwarder.  In this case, the network-provisioned resolver has been upgraded to support encrypted DNS.  It needs to indicate this somehow to clients, and could also indicate other features, such as support for MUD.  (Because it's local to the network, it could enforce the Manufacturer Usage Description).  Given that the encrypted DNS support is here, we kind of expect that clients will use that one, but they could also use an associated one elsewhere in the network.

Section 3.2: Clients that are configured to use a specific Do53 resolver.  At some point, the client needs to learn that the entity operating this resolver also has encrypted DNS support.  "Same-provider auto-upgrade".  What we don't want in this case is any offering of alternatives, because when the client has specifically selected one resolver, they don't want a resolver that is different from their selection.

Section 3.3: VPNs.  Enterprise VPNs are managed by existing protocols.  There is a desire to be able to instruct the client to use an encrypted resolver during that setup process.  This resolver would know the internal names of that enterprise, which other resolvers would not.  Why is that needed, if the VPN tunnel is already encrypted (e.g. IPSec)?  This is more of a "zero trust" model, on the grounds that there could be an attacker within the enterprise network, so encrypting the DNS traffic is still valuable.

Section 4: Discovery of limited domain resolvers.  The previous enterprise use case is an example of that.

Section 4.1.1: Local or home content.  The local resolver is an unencrypted forwarder with an associated encrypted resolver, which is not aware of the local network's names.  If the client needs to reach a local speaker or printer, the encrypted resolver will somehow refer the client back to the local resolver.

Section 4.1.2: Same-city CDN or content provider server.  Assigning users to local cache nodes is sometimes done via DNS.

Section 4.1.3: Client wants to resolver private enterprise names via the enterprise's resolver, and use a third-party/public resolver for everything else.

Section 4.2: Encrypted resolvers for content providers.  Obviously this means the content provider learns the user's IP address, but they were going to learn that anyway when the user made use of their service.

Section 5: (additional to slide contents) Opportunistic encryption is already possible and documented with DoT, so the position is that this work doesn't need to go further than that.  Captive portal detection is potentially exempt from "strict" security requirements, because it isn't user-generated and is therefore not sensitive.  Similarly for other queries generated by the operating system itself.

#### Q+A

Glenn: Our goal is to identify the set of required use cases, to add any that are missing and remove any that we don't need to support.

EKR: What is the status of this draft?  It appears to be an individual submission.

Glenn: It is an individual submission.  The chairs hope to start a call for adoption shortly depending on the discussion during this session and discussion on list after people have time to read the minutes from today's call. 

EKR: I'm not comfortable spending a lot of working group time on a document with no status.  This draft contains three separate main issues.  1 (2) VPN, and (3) a domain indicating that it would like you to use a particular resolver.  The technical requirements and security requirements are totally different in each case.  For example, we do not know how to fulfill the security requirements in case 1, whereas we do in case 3.  The signals that would be used in those cases seem likely to be very different.  Placing all of these requirements in a single document seems like a recipe for confusion.  We should break it up, workshop the cases separately, and identify which one is most important.

Paul Hoffmann: I'm concerned about Glenn's reference to "use cases".  This is not a "use case" docuemnt; this is a "requirements" document.  Mixing requirements and use cases, especially disparate use cases, seems like a way to make this WG take another year to even get started.  Using opportunistic security for the resolution step is a recipe for confusion.  Also, if we have one very difficult requirement, it's going to skew all use cases to be solved in the way that requirement needs.  Some of the use cases here require you to know what you need to know before you can even know it.  This is part of why the DHCP working group has existed for decades.  So discussing this document for use cases kind of scares me.

Daniel Migault: I think I kind of agree with EKR that we could reduce the number of use cases or provide an abstract description.  One question I have for EKR is why VPN should be a very specific use case.  A VPN could be seen as one of the way you can be provisioned by the network.  (Maybe a secure way compared to DHCP.)  In my PR, I tried to group the use cases based on overlap so that requirements may be deduced from there.

EKR: On VPNs, I think it's unlike local discovery practically because you already have an authenticated connection to the VPN provider, so technologically it's very different.  For example, IPSec already has the ability to configure your DNS provider even if in principle it's similar.  We don't need to bootstrap from a point of no trust, because we already have a secure channel.

Michael Richardson (via Jabber Scribe): As to the status of this draft, the WG chairs, having written it, probably should have just made it a WG document by fiat, and then had a discussion about what the content should be.  [MCR was confused, and thought Chris Box was a co-chair] [Note David Lawerence correction that the chairs did not author the draft a few comments further down]

Erik Nygren (Jabber): It seems better to keep this as a single document and hold off on splitting it up until later as I suspect there may be a need to refactor as it evolves.

David Lawrence (chair): **Clarification: neither Glenn nor I has been a coauthor or contributor on this draft.**

Martin Thomson: Responding to EKR: The reason I tend to glom VPNs in with local network configuration is because there are some configurations where you have that authentication, e.g. 802.1x.  I think the only thing we should start with is the case where you join a network and receive configuration that you cannot authenticate.

Paul Hoffman: +1 to that being the focus.

Ralf Weber: +1 to focus on discovery of associated resolvers.  On limited-domain resolvers (4.2): I think this muddles the ground in DNS where we have resolvers and authoritative servers.  This muddies that distinction, and could make it harder to understand and debug DNS, so I oppose this.

Vincent Parla (Cisco): For the VPN use case, the resolver is mandated by the network, and can override user preferences.  There is another use case which is Guest WiFi networks, common in the educational sector, where they have mandatory content filtering, and we should consider that use case as well.  It's similar to VPN, in that you are overriding user choice.

Tommy Pauly: I don't think I would view a VPN as voiding a user choice.  On a normal client operating system there is a choice, or at least an acknowledgement, that your VPN is on or off.  I don't think we should ever see this as overriding what the user is able to do, because they always have the option to not use that.  And on WiFi, of course the network has the ability to block me if I don't use the resolver it chooses, but I don't think we should ever be overriding a strict user preference.

Vincent Parla: But there's no way to convey that to the user in the Guest WiFi use case.

Tommy: I think all the network needs to convey is what resolver it uses.

Paul Hoffman: Tommy: When you say that VPNs start at the operating system, that's complete wrong.  In a lot of organizations they are done at the router.  I think this gets closer to the problem a lot of us are having in the working group and with this document.  People have wildly different assumptions about where certain decisions are made.  I can understand why you tend to point to the operating system, EKR points to the application, etc.  This is why I think a document focusing on one very specific use case could help us move forward.

Tommy Jensen: Today, there is no concept of a network "mandating" a DNS server.  It recommends, but it can't mandate.  I can see how that problem is worth solving, but I don't think it should be solved here, because it's much bigger than DNS.  We would want to talk about all policies that a network might want to declare.

Vincent Parla: I disagree because when it was cleartext, the network could filter and enforce any policy on DNS.  It had full control.

Tommy Jensen: But that's a side-effect of it being encrypted.  I don't think encrypting needs to maintain that functionality, any more than encrypted HTTPS has to maintain HTTP-like content filtering capabilities.  We need to enable networks to express this policy in an authenticated manner, but that's a novel concept.  Just because the network could modify DNS before doesn't mean it was expressing policy from the client's point of view.

Glenn (as individual): I've always thought of VPNs as taking over my environment.  In my corporate environment for example, they  take over my DNS configuration.  I wonder if this will create a conflict between the behavior the VPN is trying to enforce as security policy, and applications overriding or changing that.  I don't want to wade into policy questions here, but I think there's a potential conflict between the behavior the VPN expects, and the behavior that applications actually exhibit in terms of DNS resolver selection.

Andrew Campling: On resolver selection: we seem to be straying into policy territory, which is out of scope by charter.  When we talk about "users", RFC 8890 includes "indirect users" such as parents or enterprises.  I think that might be helpful terminology for these discussions.

Glenn (as chair): We will continue to monitor when these conversations do stray into policy and nudge discussion back away, because that's clearly out of scope.

Chris Box: I disagree with Andrew on one point: I think RFC 8890's "indirect users" would include parents but not enterprises.

Martin Thomson: I think we've managed to get somewhere in this discussion, and it would be good to formalize some of it.  Did we reach any conclusion on whether to separate this document into multiple requirements, and which ones to address first?

*missed comment from the chairs*

EKR: This document hasn't been adopted, so it has no status unless we reach consensus to adopt it.

Paul Hoffman: +1 to EKR.  I see two paths forward.  One is to pick apart this document, and one is to write a new document containing a subset of these ideas.  I'm not volunteering to do that because I don't think my topics of interest are in the majority here.  However, I could imagine a two-page doc, maybe not even an Internet-Draft, that indicate which use cases seem to be the hardest, and then go from there.

David Lawrence: Quoting EKR from Jabber: *it seems like a good way to move forward would be to write a new document that just focuses on local discovery.*

Andrew Campling: I don't see what difference it makes between having one requirements document with many sections or many short requirements documents.

Martin Thomson: Section 4 of this document is not in good shape; lots of it is templates and stubs.  The remaining sections are a lot more complete.  I think removing Section 4 would achieve EKR's goals.

Paul Hoffman: To answer Andrew's question, the larger the document, and the more requirements or use cases it contains, the harder it will be for a working group to reach consensus.  From the mailing list, it's not clear that every use case has to have the same solution.  Having a much narrower document, which could be merged into a larger one later, seems like it will help the working group say "this" goes to "that" requirement.

Eric Rescorla: Martin suggested that we work one set of use cases first. This document exists so that we don't work the use cases that we aren't doing first.

Michael Richardson: I think it's more interesting to have an agreed-upon set of use cases than requirements.  I would be happier if this document were only use cases, not requirements.  I think some amount of splitting or focusing is reasonable, but we have cases where we want to see if one solution covers two use cases or not, so I think it may be a fluid situation.  having been through a couple of use-case/requirements things in the last year in RATS, I think the question should not be "do I love this document", but "do I think that these authors and editors will be responsive to the working group" and "do we trust these people to do the right job".

Tiru Reddy: In the requirements draft we have several use cases, but the one that seems to be the biggest pain point is the one where there is no pre-existing relationship with the network, and we're trying to avoid connecting to an attacker-controlled server.  VPN is much more straightforward because there is a mutual auth during configuration.  The untrusted network case is the one that requires more discussion of discovery and threat vectors.

Sanjay Mishra: I think it might be easier to focus on a use case that is well-understood, for example, discovery of a local resolver.  I think a document that focuses on that would be good to get the group focused on something.  The security issues are different with enterprise, VPN, etc.  How do you ensure that the user is getting the intended resolver that the user wanted to reach out to?  Then we can potentially add more use cases or requirements later.

Tommy Pauly: I think we could have Sections 3 and 4 split into separate documents, but I think it might be useful to have them in parallel even if we focus on one set first.  To the question of focusing on the insecure local network case first: it's the hardest question, because ... will we have a secure mechanism that we are happy with?  In cases where we have a pre-existing trust relationship, we have an easier path to reaching a security relationship that we like.  I don't think we should only look at the insecure case, when the other cases are likely to have a quicker path to secure upgrade.

Mohamed Boudacair (Med): I think it's good to have a set of use cases that we can use to agree on terminology and understand each other.  We may prioritize one to focus on as a working group, but that's a separate exercise.  This document is a good starting point.  For the VPN use cases and similar, which are "more straightforward": we have to figure out the subset of use cases we want to target, but I want these cases to stay in scope.

Paul Hoffman: I strongly disagree with the last two comments: having a long list and focusing on the easy ones does not seem good to me.  The hardest one is where you have an unauthenticated network and now you want to upgrade.  The easy ones are trivial in my view, and are best handled outside this working group.  For example, the ipsec-me group should handle the question of doing this over IPSec.

Ben Schwartz: I haven't had time to think, but it's a great conversation.

[Group took 10min break after completing comments on draft-add-box-requirements-00 and resumed to discussion the GitHub issues on the draft]

### Github issues related to Draft-ADD-Box-Requirements

Glenn (as chair): I've heard the authentication issue a lot on list and in the jabber chat, but I haven't heard a consistent definition of what constitutes authentication which is covered by issue 8 and 18. Based on what's in the jabber and what we've discussed, I propose start with issue 26 and then move to authentication #8 and #18 so unless we have strong objections, I'd like to start with issue 26:  Should this really be two documents?

Issue [#26](https://github.com/ietf-wg-add/draft-add-requirements/issues/26) This should really be two documents

Martin Thomson: What EKR identified here is that the local network discovery aspects (with and without forwarders, with and without authentication, VPNs, etc.) is very much distinct from the content-provider questions.  The limited-name part also seems separable.  So my recommendation is to focus on Section 3, and remove Section 4.

Paul Hoffman: I would limit it even further, to focus on the single use-case of upgrade from a resolver address that you received in an unauthenticated fashion.  If that's too hard then we could make the first one the case where you got the address in an authenticated way, but I think that's so rare that a solution will not be very interesting.

Tommy Pauly: +1 to Martin and Paul.  The local case is the hardest but it'll be interesting to talk about in depth.  To elaborate on Section 4, if we are in a world where an application or OS is using a public resolver like 8.8.8.8 or 1.1.1.1, whether by user choice or in some other way, having some way to provision which domains need local breakout seems very important to make those use cases work well.  Another way to spin it would be to have a requirements document focused on what a local network needs to do in order to provision clients with an encrypted resolver and any split-resolution domains.

Daniel Migault: I understood the question as one document (use cases and requirements) or two.  The response I'm hearing is "one requirements document per use case".  I understand that we have a hard use case, but it's also important to have a global picture.  I'm not fond of splitting those use cases.  I'm OK with having one document for use cases and one for requirements, but splitting the use cases into separate documents risks missing the big picture

Paul Hoffman: If we pick the local use case with unauthenticated setup, there are still other things that you might want to know, like does the resolver know about local names, and are there multiple resolvers that do things differently.

EKR: I would be fine with Martin's suggestion of dropping Section 4, and if necessary we can create another document for those sections later.  The real question is here is what the working group wants to do.

Martin Thomson: Paul raised the question of whether there is authenticated configuration.  I'd be OK with saying "if the configuration is authenticated, you just set that aside".  I'm also OK with setting aside the question of capabilities like QNAME minimization, and focus strictly on the case of equivalent resolvers.

Erik Nygren: I think I'd also like to split out at least some parts of Section 4.  Some of the questions around local names are core to the use cases and must be in the main document, but others would be better in a separate document.

Paul Hoffman: This document was titled "requirements", but we seem to be focusing on the use cases.  Even in the places where the requirements are not placeholders, they seem a bit trivial.  That's good: with well-state use cases, the requirements fall right out.  Does the WG want a single use-case document, or a single use case with requirements?  I would propose single use-case.

Chris Box: In order to create a solution you need requirements.  My aim was to structure the requirements into different use cases.  That's why we need a document that describes N use cases, in order to discover all the relevant requirements.  I did expect that we would need to reduce the number of use cases in this document.

Jim Reid: I think it might be better to restructure this document to capture all the use cases, and then write separate requirements documents for each use case.

Daniel Migault: +1 to Chris and Jim.  It's hard to provide requirements without understanding the use cases, but I'm not so interested in having an exhaustive list of use cases.  Having a global picture of use cases is helpful, but the purpose is mostly to ensure that we aren't missing a requirement.  I think the use cases should be abstracted a little bit, and to focus on the requirements.

Ralf Weber: I think we should focus on what we can solve first due to the demand for a solution to this problem, so one document with the narrowest scope would be best.

Jim Reid: We can't enumerate every possible use case, so we should focus on the 3-5 most likely or credible use cases, the ones that are really going to matter, and write the use cases document around that.  We've got to move efficiently and produce some useful results; otherwise we'll go in circles and get nowhere.

Tommy Jensen: I agree that we should separate use cases from requirements.  There's a difference between use cases we agree are important, and solutions that actually belong in this working group.

Yoshiro Yoneya: My thought of use case first is that it is for applications use DNS to check something additional check such as DNSSEC validation failure and/or CAA for validation. It is not for name resolution, but for checking validity.  (Applications = browser extensions and/or smartphone apps)

David Lawrence: It doesn't sound like we have consensus on how to move forward yet.

Glenn: Agreed, but we have some good suggestions to the authors on how to move forward.  We should take this question to the list, and give the authors a chance to process the excellent input they've received here.

Paul Hoffman: I don't think that using the Github Issues to resolve this discussion is appropriate because many of us are talking about a completely different document.  The discussion on the mailing list is working just fine.  As recent chair of the github working group, I support using github issues, but not for the purpose of deciding what we're actually working on.

David Lawrence: Yes, I recognize that what you're saying is the correct way to move forward.

Glenn: I have some questions about how to best use Github as a tool to make progress, with our focus being on the list.  I would suggest that we keep the conversation on the mailing list, and ask the authors to pull the conclusion back into this issue.

Paul Hoffman: Absolutely.  The advice from the github WG is regarding (adopted) working group documents.  This is not a working group document, so there's no advantage in using Github here over mailing list, when the topic is the content and structure of a working group document.

Chris Box: I think we should discuss this topic on the list, come up with an answer about what we'd like to focus on.  I can turn that into a PR on this draft.  If we're quick enough, maybe we can do that in time for the next working group session.

David Lawrence: We've been discussing on how to move forward, and whether or how this should become a working group document.  Given the current state of discussion, what issues do the group members think we should discuss, if any?  Or should we skip the issues discussion and give the authors time to incorporate feedback.

Glenn: I think we should move the "authentication" question into the next session (Tuesday) because it's too big for the remaining time.  Instead, we can move directly to considering what topics to cover in the next interim.  Any objections?

David Lawrence: I'm OK with that.

Glenn: We have another session like this, scheduled for next Tuesday.  In addition to authentication, what other topics would people like to talk about as a priority for session 2?  Any particular draft, or topic?

Martin Thomson: I would like a shorter meeting, please.  It's now almost 1:30 AM.

Tommy Pauly: Chris had mentioned revising the doc.  I don't think we should consider any specific documents.  I would like to focus only on the question of "what does it mean authenticate this information" in the context of local network resolvers.  That particular piece of information would be very useful for guiding whatever document we have.

Paul Hoffman: +1 to Tommy, but when Puneet and I started on RESINFO over a year ago, there is one assumption that needs to brought out in the use case document: for authentication, are we talking about authenticating with a set of trust anchors that the user already has, or are we talking about adding a trust anchor?  Somewhere, the user has a set of TLS trust anchors, or else DoT/DoH will never be authenticated.  Are we talking about TLS trust anchors, DNSSEC, or a third or fourth set of trust anchors (as is common in IPSec VPNs).  I think that would be useful for the requirements to cover: what trust anchors does the user want or need to have for the discovery mechanism?

EKR: I think it would be helpful to understand what people think is inadequate about the RESINFO draft.  I would probably have designed something relatively similar to RESINFO for this use case.  I'd also like to understand the network topologies that we intend to support.  I believe it's common for people to have their own router between their device and their ISP or CPE.  Do we expect that to work?

Jim Reid: I think we shouldn't lose sight of bootstrapping more broadly, which is a superset of the authentication question.

Glenn: I don't have an opinion; I'm just listening.  I'm hearing that authentication will definitely be where we start.  We will write a proposed agenda based on the comments tomorrow in our chairs postmortem. We'll get it out Friday, people can look at it over the weekend, and we'll meet on Tuesday.  As chairs, I think our goal is to guide the group, not drive the work.  I think we've gotten good input from the group now.

Andrew Campling: Agree with EKR: some discussion on the network environments.  Those environments vary quite markedly between geographies, so capturing that would be useful.

David Lawrence: Our job here is not to declare anything by fiat.  We can help contain the discussion, but it's not up to us to decide what happens next.

Ben Schwartz:  When discussing authentication in the next session, I'd like to see a focus on the concrete interactions between the people involved in the system, and the computers or systems they are using.  The notion of "authentication" seems to depend on some fine details of precisely what kinds of prompts or other user experiences we are imagining.  Our lack of shared understanding on this topic has generated some confusion.

### GitHub issues that we not discussion

[#4](https://github.com/ietf-wg-add/draft-add-requirements/issues/4) Requirements needed in Discovery of Associated Resolvers (section 3) 

[#5](https://github.com/ietf-wg-add/draft-add-requirements/issues/5) Requirements needed in Discovery of Limited Domain Resolvers (section 4)

[#6](https://github.com/ietf-wg-add/draft-add-requirements/issues/6) Requirements needed in Privacy and Security (section 5)

[#7](https://github.com/ietf-wg-add/draft-add-requirements/issues/7) Not proving that a local network doesn't have an encrypted resolver

[#8](https://github.com/ietf-wg-add/draft-add-requirements/issues/8) Define what is meant by “authenticated”

[#9](https://github.com/ietf-wg-add/draft-add-requirements/issues/9) Feasibility of requiring the protocol to prove that the encrypted and unencrypted resolvers are operated by the same entity

[#10](https://github.com/ietf-wg-add/draft-add-requirements/issues/10) Explain home network deployment scenario

[#11](https://github.com/ietf-wg-add/draft-add-requirements/issues/11) Import use case description from draft-btw-add-ipsecme-ike-00

[#12](https://github.com/ietf-wg-add/draft-add-requirements/issues/12) Enhance BYOD description

[#13](https://github.com/ietf-wg-add/draft-add-requirements/issues/13) Is authentication ever not required?

[#16](https://github.com/ietf-wg-add/draft-add-requirements/issues/16) Associated discovery security differences

[#17](https://github.com/ietf-wg-add/draft-add-requirements/issues/17) Limit this to equivalent resolvers

[#18](https://github.com/ietf-wg-add/draft-add-requirements/issues/18) Authenticating the network-provided resolver

[#20](https://github.com/ietf-wg-add/draft-add-requirements/issues/20) Describe workable network topologies

[#21](https://github.com/ietf-wg-add/draft-add-requirements/issues/21) Value proposition conflict

[#22](https://github.com/ietf-wg-add/draft-add-requirements/issues/22) What is the purpose of 3.1.2 "Encrypted forwarder"?

[#23](https://github.com/ietf-wg-add/draft-add-requirements/issues/23) VPN requirements reduce to nothing

[#24](https://github.com/ietf-wg-add/draft-add-requirements/issues/24) Usefulness of limited domains

[#25](https://github.com/ietf-wg-add/draft-add-requirements/issues/25) VPNs can also use DNS for routing decisions (section 3.3)





## Chairs - close 


[Notes](https://codimd.ietf.org/uploads/upload_8dfb6bbfe1e459a6a3deb85e39fa2559.png)

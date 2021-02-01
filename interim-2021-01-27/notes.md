# January 27, 2021 ADD WG Interim Session Agenda

## ADD Chairs & AD
* AD:  Barry Leiba
* Chairs: Glenn Deen, David Lawrence


## Session link, minutes, jabber 
* [Meeting Info](https://datatracker.ietf.org/meeting/interim-2021-add-01/session/add)
* [Webex info](https://ietf.webex.com/webappng/sites/ietf/meeting/download/4393d891ccf04a14a0be757d926eb8ea?siteurl=ietf&MTID=mf551d0af93cc8c57dab825f7dba81717) 
* [Minutes](https://codimd.ietf.org/notes-ietf-interim-2021-add-01-add)
* [Meeting chat](xmpp:add@jabber.ietf.org?join) 


### Session: January 27, 2021 16:00 GMT 

## Session 1 Agenda

### Welcome
5 min Administration
* Scribe selection: Barbara Stark
* [NOTE WELL](https://www.ietf.org/about/note-well.html)
* Agenda bashing

### Drafts
* 15 min - Design work + Draft-box-add-02  Update ([Draft-02](https://datatracker.ietf.org/doc/draft-box-add-requirements-02/) ([Slides](https://github.com/ietf-wg-add/wg-materials/blob/master/interim-2021-01-27/Emerging%20Use%20Cases%20for%20Encrypted%20DNS.pdf))

* 5 min - Update:  Draft-btw-ad- home-12 ([Draft-12](https://datatracker.ietf.org/doc/draft-btw-add-home-12) ([Slides](https://github.com/ietf-wg-add/wg-materials/blob/master/interim-2021-01-27/draft-btw-add-home-Interim012021.pdf

* 5 min - Update:  Draft-pauly-add-DEER ([Draft](https://datatracker.ietf.org/doc/draft-pauly-add-deer-00/)) ([Slides]https://github.com/ietf-wg-add/wg-materials/blob/master/interim-2021-01-27/ADD-Jan-21-Interim-DEER.pdf))

### Discussions
60 min Discussion / Comments

### Planning & Wrap up

* 5 min - Quick discussion about IETF110

---

# Session Notes

Glenn welcomed everyone. Good attendance. Please sign the "Blue Sheet" in Etherpad.
Emails (requested in Blue Sheet) will only be provided to secretariat and will be stripped before publishing minutes in data tracker.

## Updates 

Strong support to adopt draft deer. Will finish that up on the email list.

### Design Team + Draft-box-add-02 

Chris Box presented slides.
... but when Chris got dumped out of WebEx, Ben Schwartz took over on slide 3.
Turned over to Tirumaleswar  Reddy Konda (Tiru) on slide 7.
Tommy Jensen took over on slide 13.
And then a smooth hand-off to Jim Reid on slide 14.

Glenn: I want to hold off on all questions until after the next 2 presentations.

### Draft-btw-add-home-12

Tiru presented. 

Glenn: Again, we're holding off on discussion until after all the presentations.

### Draft-pauly-add-DEER

Tommy Pauly presented. Acknowledged draft has been adopted but name will change.

Glenn: Thank you. 5 minute break now (reconvening at 44 minutes past the PST hour). Then discussion.

## Discussion 

Glenn: Mic line is now open. David Lawrence is in charge.

EKR: Dread about scope creep. I thought we had been converging on narrow scope as defined by DEER. I could be persuaded to maybe define DHCP mechanism, but we should stop there. Since these deliver more or less the same info via different channels...

Ralf Weber: I agree with EKR that DEER and DHCP option are priorities. Specifically on draft, there was discussion on URI scheme. I think we should keep that on RA/DHCP scheme. There is a need for automatic upgrade with both. Are you really talking about SVCB records? They are really HTTPS records?

Tommy Pauly: Yes. There will be more on use of DNS domain for that later.

Glenn (as participant): Comment on Jim Reid use case. If there were some concept of JSON document, I think that might also be an interesting work item. Possibly even with added security such as a signed json doc

Ben S: To echo Ralf's point about scope -- I think the WG has the right priorities in what it's working on now. But we were looking further out. It may be that the further out items aren't worth standardizing. The use cases aren't really active for those.

Tommy Pauly: I hope we can re-use format and approach as much as possible between RA/DHCP and DEER. We need common methodology for encoding. Then if we add, e.g., DNS over QUIC, we don't have to create new ways of encoding it. I was encouraging people in other groups to wait for ADD to define format. We are relying on definition of SVCB that is in Ben Schwartz WG draft in other WG. We need to talk about that -- adoption, etc.

Glenn: We're waiting for the SVCB draft to progress. It seems to be nearing WGLC. Stay tuned.

Tommy P: To EKR's comments, I agree we should prioritize RA/DHCP and DEER first. But we do need to make it possible for people to experiment on other mechanisms. That might be something for another WG.

Andrew Campling: I do think it would be helpful to adopt the requirements draft. If some are not solved, so be it. But at least they should be documented. There are some good things in there. On DEER draft -- I recall the point about certs was to avoid encouraging activity that resulted in more centralization. We should be mindful of that.

Daniel Migault: I was wondering if the SVCB would also apply to DoT?

Tommy P: It's a DNS SVCB and will work for DNS over anything.

Daniel: I think we can do better than using a well-known name to search for. Also, It woudl be good to tell explicitly us where the discussion is happening? If it's not going to be on the ML, the chairs should send frequent reminders. I'd like to see the RA/DHCP draft adopted soon and the requirements draft.

Chris Box: Responding to EKR's comment that this is too much scope. In charter of WG, communication of resolver info for user selection is there. Also, are you expecting client to always follow designation? We want to avoid everyone having their own TRR list. Too much fragmentation. So let's standardize something to help clients in selection.

Eliot Lear: Thx to EKR for explaining why cert needs to be in message. I have scalability question on how many IP addresses have to end up in certs. Also, what sort of toplogy awareness is needed? I supported DEER draft adoption but think we need discussion of that.

Tommy P: First to respond to Chris. It concerns me for us to say we want to define policy by which client should trust. Client choices on who to trust should be client decision and not dictated by WG. Thanks Eliot for bringing up scalability question. Hopefully the number of certs / IP addresses will be limited.

EKR: I just looked at the charter and find the statement Chris referenced to be baly written. What info to communicate should be limited to info client can consume. We do have an example of how this works with CA root programs.  I don't see primary problem. It's not difficult -- when you decide you want to use root format you've decided who you trust. I haven't heard anyone who says what they actually want in those advertisements.

Ben: What I heard Chris talking aout was clients providing additional filtering on top of DEER. They may decide not to follow an upgrade based on additional info. This is consistent with DEER. But DEER draft is not designed to require this. DEER upgrades are safe according to DEER logic. The question is whether DEER threat models are sufficiently broad so clients won't need this sort of additional filtering. You might consider a resolver upgrade that bypasses NAT and reveals client IP address and info to the upstream resolver. That's outside of DEER threat model as currently framed (of course, DEER is currently in flux).

Chris: I wasn't trying to say this WG should tell the client what to do. I was saying it's helpful to standardize info provided to client in case they wanted to make use of that info. For example, there might be indication that a resolver has special info. Client might care. I can see EKR's point that CA root mechanism can be used to say resolver cert is "safe" -- as defined by "somebody" (implied somebody is not the user).

Andrew: We need to look beyond DEER for solutions that are more generally applicable to bulk of users.

Tommy: I was specifically referring to IP address in cert. Other DEER usages do not require that.

Andrew: Thank you. Then my apology stands.

Daniel: In the first interim meeting, we agreed that upgrading to public resolver was probably the easiest use case. The other use cases are more complex. But I don't see why we're already talking about closing the WG when we're only at the point of adopting the first draft -- we shouldn't be saying that if you're use case is not covered by DEE you're out of luck.

Glenn: That's not the intent. We will be putting other drafts up for adoption.

Daniel: Are drafts adopted in next weeks the only ones that will be adopted by WG? That's what I thought I heard. Will we have opportunity to present other use cases. I think it's too early to say no other use cases will be addressed.

Glenn: We do not have WG consensus to say we have all the use cases documented. People are welcome to propose additional use cases. We are still accepting new drafts. We're not closing the WG at this time.

David Lawrence: Did others think we were closing?

EKR: I'm saying we should first deal with solutions proposed now. Then we can be open for other solutions.

Tiru: I think we need to see what sort of info operating systems could use to make selection decisions better and more secure.

Tommy J: The WG should most definitely not be working on topics that have no chance at adoption or a strong need. We've provided use cases that we considered important. For example, once you have encrypted resolvers, I can see where someone would want trust to be dated. We should have discussion.

EKR: You're talking about client authenticating server and not server authenticating client.

Tommy J: It might be nice for employer with many work-at-home employees to have a public resolver. In which case they would want to authenticate clients.

Glenn: That use case is out of scope of our current charter.

Tommy J: I actually did think that a resolver describing its properties would be in scope.

Glenn: I didn't want people think we were going in a new direction.

Eric Orth: Client authentication is something that has been coming up in Chrome discussions. I don't want to go there. OS clients may be in a better position to handle it. But I do think allowing the server to describe itself is in scope. What client does with that is out of scope.

Andrew: I don't think it's consistent with charter for client developers to give permission for us to discuss a use case.

EKR: I'm saying that if no client is interested in a requirement, we shouldn't discuss.

Ben Schwartz: This applies to any 2-party protocol.

## Wrap up

Glenn: Thank you. We have requested 2 sessions at IETF 110. We find these things work better as discussion forums rather than presentation forum. If there are issues you would like to discuss, please let the chairs know.

Barry Leiba (as AD): We'll be figuring out who will take over as AD. It's unclear if it will be an ART AD, since WG is under INT. If you have opinion, please express it.

----

Notes taken by Barbara Stark,  Jabber scribe was Andrew Campling



- end of Notes -


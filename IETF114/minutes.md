# Adaptive DNS Discovery

Time: Tuesday July 26 3:00-5:00pm 
  – Session slot shared with DPRIVE

## Part 1: Agenda ADD@IETF114

ADD Chairs: David Lawrence, Glenn Deen

Area Director: Eric Vyncke

## DNS Directorate: Eric Vyncke
Eric:  Lots of drafts are relying DNS
	     Setting up a DNS Directorate to do early review of drafts
       Will be asking for voluteer reviewers
	     OK if you're less experienced but eager to help and learn
        
Daniel Kahn Gillmor: There is no single DNS viewpoint
		                 People have very different viewpoints
    
Paul Hoffman: Is it OK for a review to say "don't do this in the DNS"
                
Eric: yes
  



### 1 Administration

    IETF NOTE WELL
   
    Scribe selection
   
    Agenda bash
   
    Welcome from chairs


### 2 Chair/AD - Status Update on ADD Documents:

    Discovery of Designated Resolvers (draft-ietf-add-ddr-08)

    DHCP and Router Advertisement Options for the Discovery of Network-designated Resolvers (DNR) (draft-ietf-add-dnr-11)

    Service Binding Mapping for DNS Servers (draft-ietf-add-svcb-dns-06)

Glenn: DDR, DNR, service binding are moving well in IESG review

### 3 Drafts

    draft-ietf-add-split-horizon-authority
    
Dan Wing - presented

Ben Schwartz: 
    What changes do people want?
		Draft is self-consistent, please speak up if you want changes
		Could be used with DHCP
    
Andrew Campling: Looks good, would like to see Last Call soon
	
Tommy Pauly: Do you always have to do DNR to use this?
		
Ben: DNR gives a unique host name, DDR gives one or more host names
			With DDR, would have to walk through the cert looking at names

Tommy: Would like to look into using DDR because it is just a client update

    
    draft-reddy-add-resolver-info
    
Mohamed Boucadair - presented

Puneet Sood: General structure looks fine
		Not seeing interest from client implementor
		Would like to see implementor interest before adoption
		Proposes to encode options as numbers, not text
    
    draft-schwartz-add-ddr-forwarders

Chris Box - presented
          - noted document change name to Reputation Verified Selection of Upstream Encrypted Resolvers

Eric Orth: Still wants this for specific use cases
		
Andrew: Move forward with this
		
Tommy Jensen: Finds the new direction quite interesting
			Thanks for addressing the issue
			Has tried avoiding listing trusted resolver
			Why not just provide the list directly to the customer?
    
### 4 AOB / Discussion (all)

### 5 Planning & Wrap up

Glenn Deen
	Group is in pretty good shape, 3 documents with the IESG, more in the pipeline
	Congrats for doing this all virtually

-- End of ADD Agenda --

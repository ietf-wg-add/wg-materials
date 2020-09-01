# Using GitHub for the ADD WG

ADD is the IETF Adaptive DNS Discovery Working Group

The Working group has a mailing list [[Add subscribe](https://www.ietf.org/mailman/listinfo/add)] [[ADD archive](https://mailarchive.ietf.org/arch/browse/add/)]. The groups documents and materials can be found in the IETF [ADD datatracker](https://datatracker.ietf.org/wg/add/about/)

## GitHub Link

The ADD WG GitHub is at:  https://github.com/ietf-wg-add    

Access is fully public with no need to login for reading.

This link is also listed on the ADD WG [About](https://datatracker.ietf.org/wg/add/about/) page.

## ADD GitHub relationship to the ADD WG

The GitHub repositories and tools provide help such as versioning, issue tracking, shared file storage, to assist authors in the development of drafts.  This does not replace the IETF tools such as datatracker and the recognized versions for the working group are found in the iETF Datatracker.  The source stored in GitHub are development copies where the authors and other contributors can work together to produce a version of the draft which is then built and and released to the WG by uploading to datatracker in the traditional IETF way.

The ADD mailing list [[subscribe](https://www.ietf.org/mailman/listinfo/add)] [[archive](https://mailarchive.ietf.org/arch/browse/add/)] and IETF [ADD datatracker](https://datatracker.ietf.org/wg/add/about/) remain the primary places where ADD is discussed and managed.  Github is just an additional set of tools to help  document authors and commentors in the writing of documents.

## IETF Use of Github

ADD’s Github setup was done following [RFC8874](https://www.rfc-editor.org/rfc/rfc8874.html) and [RFC8875](https://www.rfc-editor.org/rfc/rfc8875.html)  
### IETF Contributions Rules Do Apply

Be aware as noted in [Contributing.md](https://github.com/ietf-wg-add/draft-add-requirements/blob/master/CONTRIBUTING.md) submissions to the IETF-ADD-WG repositories are considered Contributions to the IETF Standard Process an all the usual IETF rules apply.

## Github Userids

All the ADD materials stored in Github are public, so if all you want to do is read them then a Github account is not needed.

If you want to contribute material or if you want to submit issues you will need a (free) github account.    You can signup through the [ADD Github page](https://github.com/ietf-wg-add).


## ADD Organization and Repositories

### IETF-WG-ADD Organization
IETF-WG-ADD is in Github’s terminology an organization.  IETF-WG-ADD organization serves as the collecting point for repositories, which hold files and directories, that are associated with the ADD working group.   

### Repositories
Under this structure each draft is setup in its own repository that is created to hold drafts and working materials the group is drafting.   This practice of having a single draft document per repository follows the best practices that have been developed in other IETF working groups.   

In each draft’s repository the authors of the document can develop their draft with the repository to hold the original source, plus they can also check in additional materials such as diagrams, notes,  notes from meetings the authors had, etc  What the authors choose to put into the repository is entirely up to them.  It is a just a tool to help in their work, not a strict process.  So for example if they’ve created diagrams that capture discussion points, they can upload them into their documents repository to store and share, but it’s entirely optional if they choose to do so.

Drafts developed can be done in XML or in Markdown when first setup in a repository.  Drafts are converted from XML or markdown into text, HTML, and XML versions using a makefile that is run locally on the author’s computer that can build the draft.

Currently there are 2 repositories under IETF-WG-ADD:  _wg-materials_  and _draft-add-requirements_.   You’ll see these on the front page of the link above.

[**WG-Materials**](https://github.com/ietf-wg-add/wg-materials) is pretty empty so far, but it will hold general materials for the WG.

[**Draft-add-requirements**](https://github.com/ietf-wg-add/draft-add-requirements) is where work is being done on the requirements draft.  

If you are an ADD draft author and want a github repository setup for your draft please contact the ADD Chairs _add-chairs at IETF.ORG_ to have one created.

***


# GitHub introduction for IETF draft development

## How do access GitHub

There are 3 ways to access GitHub - web, desktop client, command line.  

### Web Interface
For the purposes of developing an IETF draft it’s entirely possible to access Github entirely through the Github web interface.   Issues can be submitted and worked with, document authors can create and edit documents, and changes can be checked in and managed, and pull requests can be approved and managed.  

### Desktop client
There is a desktop client for Github which provides a GUI to do what the Web Interface provides and adds integration with local editing tools and source development tools.   However, for developing an IETF draft document the web interface is likely sufficient for a great many people.   I personally have the desktop top installed by rarely use it.

Command line is also an option but for the rest of this into we'll be focusing on the web interface as it has the lowest barrier to entry to most people.


## Branches

Github is at its heart source code management system and so has the notion of code branches built into its core.   There are two important branches to know about for IETF draft development – Master and gh-pages.

On the web interface of GitHub the branch you are viewing is selected by the pulldown that appears just above the list of files.

### Master branch

  This is the main code branch of the draft under development. It’s the default branch people interact with.  When pull requests are merged, it is typically this branch they are merged into.

### Gh-pages branch
This is the GitHub Pages branch.  Github Pages are public web pages associated with the repository, and in the case of IETF drafts where intermediary builds of the draft source are stored.

Of note for IETF drafts is that this also contains the built versions of the draft being developed.   Note – these draft builds are not the same as revisions checked into the IETF Datatracker.  These are intermediary builds created by the document authors when they run the make file and check in the updated draft.

## Creating IETF Drafts

The master branch holds the master copy of the draft under development.  For example – [draft-add-requirements.md](https://github.com/ietf-wg-add/draft-add-requirements) which is the master source for the ADDREQ document.

    Hint: It would be useful to have the draft-add-requirements page open
          to follow along when reading the next sections.

### Latest checked-in Build

For draft-add-requirements you’ll find the latest build under the “Editor’s
Copy”  link in the repository's
[Readme.md](https://github.com/ietf-wg-add/draft-add-requirements/blob/master/README.md)
file, which is also shown as the default view when you visit it’s repository.
You’ll also find link there to “Individual Draft” which is just the version
that been checked into Datatracker.

Editor’s copy points to a formatted version of the draft at:
https://ietf-wg-add.github.io/draft-add-requirements/#go.draft-add-requirements.html

### Document Source

 The source can be fund in the main branch, for draft-add-requirements.md is
 written in markdown.  Note authors can also choose XML as an alternative s ource format.

###  Building

     Note: This is only something the document authors need to do.
           It's included here so that everyone can better understand the process.

Build draft-add-requirements.md is done using Make. This done outside of Github and requires a computer with the needed tools installed.  Basically it will run a markdown to IETF RFC2629 converter and then process the XML output through xml2rfc to produce TXT and HTML versions of the draft.   Document authors who have the needed GitHub privileges can put these documents to the github repository via a checkin or pull request.  They can also be uploaded to the IETF Datatracker when the authors want to publish a version of the draft.

    The tools used to build the documents are documented here:  
    https://github.com/martinthomson/i-d-template/blob/master/doc/SETUP.md

---

## Contributing to a document using  GitHub

There are two main ways of contributing to a document using GtHub’s tools: _Issues_ and _Pull Requests_


### GitHub Issues

Issues are good for submitting short discrete items that will be tracked from submission to closure.  They are proposals, questions or reports that can be easily evaluated in a binary way such as open/closed, accepted/rejected etc.

Issues are submitted using the ISSUES tab on the repository.   Issues are public even without login, but you will need a Github login to submit an issue.  

Issues are nothing more than a reporting tool provided by github to repositories that permit a submission of tracked comments to the repository.

Tracking includes the original submission, additional comments added throughout its lifetime and its final resolution status such as closed, rejected, out of scoep etc.   It’s basically a bug tracking systems for code, but it is also useful for suggestions, small changes, questions, etc.  

#### Best Practices

Best practice for issues is to make them short and limited to one specific item.   Ultimately the goal around an issue is to close it out, while allowing tracking of its progress from first submission, to consideration discussion, to response, and finally to closure.     Submitting a big issue report which combines a number of items makes it difficult to impossible to track the individual items, and for the issue to eventually be closed because multiple items in a big submission may have different final states making it impossible for the issue to be closed with a clear definite final state.


#### Examples of well written issue submissions:


##### 1. Focuses on single item; Provides suggested resolution action.

>    Section 4.1 of the document doesn’t mention root name servers. Please add material on how root name servers are affected.

#### 2. Provides a specific suggestion to the document authors.

> A diagram in section 5 showing the interaction of the stub resolver and the recursive resolver would be helpful in the understanding the interaction.

#### 3. Very focused; Provides clear fix.

> Good Issue:   The reference to RFC XYZ is incorrect.  XYZ has been replaced by RFC ABC.

#### 4.  Very focused; Provide suggested language to use.

>Good Issue:  Section 3 would be better with an introduction paragraph such as “The transactions documented in this section are meant to illustrate ……..”

#### 5. While resolving this issue is potentially a lot of effort, it is a good example of using issues to submit focused feedback and suggestions to document authors.
>Good Issue:  The security section currently doesn’t include any discussion of validation of resolvers in order to validate they are reachable by the client.  



### Examples of bad issue submissions:

#### 1. Too vague
> The documents security section isn’t using the right model.

Improve by:  Giving detailed information, and examples on the model to use and why.       

#### 2. Not specific and missing detail
>   Section 3 should consider the discussion going on in some other working group, (I don’t know which) that I heard taking about something related during its session at IETF103.

Improve by:   Get specific as much as possible. Include the WG name, perhaps a link to the discussion recording including the time offset when it takes place in the recording. What is it that should be considered?

#### 3. Conflating or combining more than one item into a single issue


>The document should consider the impact of adding an additional field to the response text. It also should elaborate on split DNS impacts, and section 2 line 3 should say MUST.   

Improve by:  Submit each item as separate ISSUE.


#### Commenting on Issues

Issues can also be commented on giving an opportunity to ask questions, to provide more detail, or to discuss proposed solutions, and to provide detail on the resolution or final state of the issue.  

     Note: Issues are public so please be polite and to keep discussion focused
           on the issue.




### Pull Requests


Pull requests are how files in the repository are added/deleted/modified.   They are appropriate for submitting new material and changes to the document being developed.  A pull allows multiple changes from a submitter to bunded together.

The terminology can be confusing at first.  A *pull* request is not a request made to git to download the contents of a repository, instead it is a request made by the submitter to have files PULLED into the repository.    

The act of downloading the contents of a particular repository is called cloning.     

So the normal workflow in working with a github repository is to clone the repositories files to the user’s computer,  make changes, additions, deletions to the set of downloaded files, and then submit a pull request to the repository to ingest the updated set of files.

Pull requests require someone with authority over the resistor to accept them.   So contributors do not need to worry about accidentally submitting a pull request and causing disruption to the repository.  It requires a deliberate action by someone with authority over the repository to choose to accept submitted pull requests.   

Files in stored in github are versioned so even if a pull request did make unwanted changes, the repository admins can revert back to an earlier version.

----

## Getting Comfortable with Github

10 minutes of playing with Github can provide a lot of anxiety reduction to new users.

- Github accounts and public repositories are free so a good way to get familiar is to setup your own repository and try things with it.  

      Remember it’s public so don’t upload the code base of your confidential new product.   

- Create yourself a sandbox repository on it and create a few documents in it.  
- Readme.md is a good starting place as it will be displayed as the default document when someone accesses your repository.  
- Use the web interface to make changes and submit them.  
- Next create an issue, comment on it and close it.   
- Download a clone of your repository, make a couple of changes to the local copy and then submit a pull request for those changes, and then respond to the pull request and accept the changes.   


----
Version 1.0 September 1, 2020 - Glenn Deen

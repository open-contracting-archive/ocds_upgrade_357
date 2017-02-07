## Amendment handing in OCDS

Version 1.1 of OCDS clarifies the way in which information on amendments, and the changes they bring, should be represented. 

### Background 

#### Needs analysis

Users need:

* To see when particular tender, award and contract fields such as value, period or items (deliverables) have changed, and how they have been altered;

Publishers need:

* To be able to account for variations in a tender, award or contract; 

#### Supply-side analysis

The term 'amendment' may have a specific legal meaning for some publishers, and it may be important to (a) clearly identify those changes which are amendments; and (b) avoid using the term amendment for changes which are not formally labelled amendments by that publisher. 

For some publishers, the set of fields that can be legimately amended may be restricted by law or policy.

Amendment information may be provided by:

* Giving the former value, and the new value;
* Giving the delta (the change between former and new value) (e.g. Canada);
* Providing unstructured information on the change, for example, in attached documents (e.g. New South Wales);

In some systems, contracts cannot be amended, buy instead a new contract issued, which contains the change.

### Data Modelling

When an amendment is made, there are two complementary ways in which it can be represented:

(1) In an ```amendments``` block, which will provide information on the date of amendments, rationale for changes, and a description of the changes made. 

(2) By providing two or more OCDS releases for the same contracting process, one with the changed details, which can be compared to the earlier releases to see what has changed. 

Users seeking a human-readable explanation of an amendment should look in the relevant ```amendments``` block. Users who want to understand how values have changed, should compare release from before and after the amendment.

#### The amendments array and block

```Tender```, ```Award``` and ```Contract``` sections each have an amendments array. 

The can contain any number of ```amendment``` blocks describing (without a requirement for structured data) a collection of changes made to the tender, award or contract at a particular point in time, and with the following properties:

* id - a unique identifier for this amendment
* date - the date of the amendment
* rationale - a rationale for the amendment
* description - a free text, or semi-structured text description of the changes made
* amendsReleaseID - the identifier of the release that represents this contracting process before the amendment
* releaseID - the identifier of the release that represents this contracting process after the amendment

#### Providing structured change data

A release package consists of one or more entries in the ```releases``` array. This allows the provision of 'before' and 'after' data when an amendment is made. 

**When an amendment results in a new award and/or contract being issued**, then this should be represented by a release that contains new entries in the ```awards``` or ```contracts``` array. The relationship between the new contract, and the contract it amends, an be indicated with the ```extendsContractID``` property. 

**When an amendment changes the values of an existing tender, award or contract**, then a release with a releaseTag of 'tenderUpdate', 'awardUpdate' or 'contractUpdate' release should be made. 

### Changes in version 1.1

* The single ```amendment``` object in ```Tender```, ```Award``` and ```Contract``` is replaced with a ```amendments``` array, so that all the amendments over the lifetime of a contracting process can be stored;
* The semi-structured ```changes``` array in ```amendment``` is deprecated in favour of providing free text description of amendments, and using multiple releases to provide structured information on changes.
* Explicit ```amendsReleaseID``` and ```releaseID``` properties are introduced to support comparison of what has changed as a result of each amendment. 

### Worked example

#### (1) Tender amendment

> A tender is issued by 'AnyTown Council' to buy 100 Chairs with a total budget of £1000. Three weeks later, following a review of the demand for chairs in AnyTown Council, the tender is amended to ask for 150 Chairs with a total budget of £1400. 

When the tender is first issued, this would result in the tender release below:

EXAMPLE RELEASE

When the amendment is provided, a new release will be created which contains the updated figures as shown below, and an entry in the ```tenders.amendments``` array with free-text information about the changes. 

EXAMPLE RELEASE
    
To aid users accessing these, they should both be contained in the **record** for this contracting process, which would look as follows:

EXAMPLE RECORD (CONTAINING EMBEDDED RELEASES)

To see the change, a user could rely on release-aware tools (for example, with this version of OCDS Show the change between the two releases is indicated), or could convert the data to spreadsheet format, and compare rows for the same ocid (contracting process identifier) to see how values have changed. 

#### (2) Contract extension

TODO



    

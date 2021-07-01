# Consent Workflow Use Cases

## Agent grants access to an application to a selected subset of data 

Alice wants to use an application called FinanceInsight to help her
understand trends in her cashflow and spending activity, using financial
data stored in her personal and work pods. First, she'll need to grant 
FinanceInsight access to that data across her two pods. 

Since she has a lot of other data in her pod that isn't finance related, 
she wants to grant FinanceInsight access only to exactly the data it needs.

Because her finance data is spread across her pods, along with
some data that is related in context, she needs help to make sure that she's
giving FinanceInsight access to all of the data it needs to operate, but no
more.

Alice has a trusted party handling authorization that helps her interpret the kind
of data that FinanceInsight needs, and then presents the access decisions to her
so that she can make intuitive, smart choices. Once she makes those 
decisions, her trusted authorization application stores a record of them,
adjusts permissions, and provide FinanceInsight with a record of what it
can access.

## Extending to the Web of Services

Satisfied with FinanceInsight's capabilities, Alice wants to allow the 
App to follow links from the data on her pod to data located across 
the web, only when its about transactions in which she is involved. 
For example she has links to her Bank statements, to receipts 
from shops she visited, and bills from electricity, water and 
other service providers. These remote spaces are protected, set to
only allow access by Alice — and, of course, by the service provider,
shop, bank, etc. 

The online bills are very interesting as they often have links to
rich metadata about the specific products she bought, which can
go far beyond the total payment shown on her credit card bill.
The receipts might provide links to the product ontology,
supply chain history, notifications of updates or problems
with the product, and/or links to communicate directly with
the producer. 

Even though Alice is happy with FinanceInsight being able to 
read such data, she wants to limit the Apps ability to participate 
in any transactions on those web sites. The App should be able to
Read but not write, unless given explicit personal permission by 
Alice.

# Consent Workflow Use Cases

## Agent grants access to an application to a selected subset of data across pods

Alice wants to use an application called FinanceInsight to help her
understand trends in her cashflow and spending activity, using financial
data stored in her Pod and data linked from there to transaction she made with others
such as her electricity provider, telecom agency, supermarket, bank, etc... 

Alice needs to grant FinanceInsight access to the financial data on her Pod and
limit its access to most other data there.  She does want the App to fetch
data from the services she used online, as those contain rich metadata that
will allow it to categorize her financial activity. For example, even 
though her credit card will show only the total sum of the goods she bought,
the supermarket web site will have the full list of goods bought in machine
readable format for each transaction, with information on the type of goods,
links to the producers,... 

She wants to limit the Apps ability to do anything other than read
data from those other services, even though she herself using her browser is able to make
purchases online from them. 

Alice has a trusted party handling authorization that helps her interpret the kind
of data that FinanceInsight needs, and then presents the access decisions to her
so that she can make intuitive, smart choices. Once she makes those 
decisions, her trusted authorization application stores a record of them,
adjusts permissions, and provide FinanceInsight with a record of what it
can access.

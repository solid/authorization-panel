# Consent Workflow Use Cases

## Agent grants access to an application to a selected subset of data across pods

Alice wants to use an application called FinanceInsight to help her
understand trends in her cashflow and spending activity, using financial
data stored in her personal and work pods. First she'll needs to grant 
FinanceInsight access to that data across her two pods. 

Since she has a lot of other data in her pod that isn't finance related, 
she needs to grant FinanceInsight access only to exactly the data it needs.

Because there's a lot of finance data spread across her pods, along with
some data that is related in context, she needs help to make sure that she's
giving FinanceInsight access to all of the data it needs to operate, but no
more.

Alice has a trusted party handling authorization that helps her interpret the kind
of data that FinanceInsight needs, and then presents the access decisions to her
so that she can make intuitive, smart choices. Once she makes those 
decisions, her trusted authorization application stores a record of them,
adjusts permissions, and provide FinanceInsight with a record of what it
can access.

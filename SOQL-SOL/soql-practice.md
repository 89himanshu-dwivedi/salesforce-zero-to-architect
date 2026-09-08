# SOQL Practice — One Query Per Concept

1. Get Id and Name of all Accounts.
```sql
SELECT Id, Name FROM Account
```

2. Get Contacts where LastName is 'Smith'.
```sql
SELECT Id, FirstName, LastName FROM Contact WHERE LastName = 'Smith'
```

3. Get Accounts using a bind variable list of Ids.
```sql
SELECT Id, Name FROM Account WHERE Id IN :accountIds
```

4. Get top 10 Opportunities ordered by Amount descending.
```sql
SELECT Id, Name, Amount FROM Opportunity ORDER BY Amount DESC LIMIT 10
```

5. Get next page of Accounts (records 21-30) using OFFSET.
```sql
SELECT Id, Name FROM Account ORDER BY Name LIMIT 10 OFFSET 20
```

6. Get Accounts with keyset pagination after a given Id.
```sql
SELECT Id, Name FROM Account WHERE Id > :lastSeenId ORDER BY Id LIMIT 50
```

7. Get Accounts whose Name starts with 'Acme'.
```sql
SELECT Id, Name FROM Account WHERE Name LIKE 'Acme%'
```

8. Get Contacts whose Email is in a set of values.
```sql
SELECT Id, Email FROM Contact WHERE Email IN :emailSet
```

9. Get Accounts excluding a set of Ids.
```sql
SELECT Id, Name FROM Account WHERE Id NOT IN :excludedIds
```

10. Get Cases with a multi-select picklist field including a value.
```sql
SELECT Id, Subject FROM Case WHERE Categories__c INCLUDES ('Billing')
```

11. Get Opportunities that are open AND Amount greater than 10000.
```sql
SELECT Id, Name FROM Opportunity WHERE IsClosed = false AND Amount > 10000
```

12. Get Leads created today.
```sql
SELECT Id, Name FROM Lead WHERE CreatedDate = TODAY
```

13. Get Cases created in the last 30 days.
```sql
SELECT Id, Subject FROM Case WHERE CreatedDate = LAST_N_DAYS:30
```

14. Get a Contact's Account Name using child-to-parent dot notation.
```sql
SELECT Id, Name, Account.Name FROM Contact
```

15. Get a Contact's Account Owner Name (multi-level parent traversal).
```sql
SELECT Id, Name, Account.Owner.Name FROM Contact
```

16. Get an Account with its related Contacts (parent-to-child subquery).
```sql
SELECT Id, Name, (SELECT Id, LastName FROM Contacts) FROM Account
```

17. Get an Account with its Opportunities' Owner Name (nested relationship).
```sql
SELECT Id, Name, (SELECT Id, Name, Owner.Name FROM Opportunities) FROM Account
```

18. Get Tasks with polymorphic WhoId filtered by type using TYPEOF.
```sql
SELECT Id,
       TYPEOF WhoId
           WHEN Contact THEN FirstName, LastName
           WHEN Lead THEN Company
       END
FROM Task
```

19. Get count of all Cases for an Account.
```sql
SELECT COUNT(Id) FROM Case WHERE AccountId = :accountId
```

20. Get count of Contacts that have a non-null Email.
```sql
SELECT COUNT(Email) FROM Contact
```

21. Get count of distinct Industries across Accounts.
```sql
SELECT COUNT_DISTINCT(Industry) FROM Account
```

22. Get total Amount of all closed-won Opportunities.
```sql
SELECT SUM(Amount) total FROM Opportunity WHERE StageName = 'Closed Won'
```

23. Get average Opportunity Amount by Stage.
```sql
SELECT StageName, AVG(Amount) avgAmount FROM Opportunity GROUP BY StageName
```

24. Get the largest and smallest Opportunity Amount.
```sql
SELECT MAX(Amount) maxAmount, MIN(Amount) minAmount FROM Opportunity
```

25. Get Case count grouped by AccountId.
```sql
SELECT AccountId, COUNT(Id) FROM Case GROUP BY AccountId
```

26. Get AccountIds with more than 5 Cases.
```sql
SELECT AccountId, COUNT(Id) FROM Case GROUP BY AccountId HAVING COUNT(Id) > 5
```

27. Get Opportunity Amount subtotal and grand total by Stage.
```sql
SELECT StageName, SUM(Amount) FROM Opportunity GROUP BY ROLLUP(StageName)
```

28. Get every combination subtotal of Region and Stage.
```sql
SELECT Region__c, StageName, SUM(Amount) FROM Opportunity GROUP BY CUBE(Region__c, StageName)
```

29. Get Accounts respecting the running user's field-level and object security (throws on inaccessible fields).
```sql
SELECT Id, Name FROM Account WITH SECURITY_ENFORCED
```

30. Get Accounts respecting FLS, CRUD, and sharing rules together (Apex, API 51+).
```sql
SELECT Id, Name FROM Account WITH USER_MODE
```

31. Search 'Acme' across Accounts and Contacts using SOSL.
```sql
FIND 'Acme' RETURNING Account(Id, Name), Contact(Id, LastName)
```

32. Search 'John' across all searchable fields, limited to 20 results.
```sql
FIND 'John' IN ALL FIELDS RETURNING Contact(Id, Name), Lead(Id, Name) LIMIT 20
```

33. Search using a wildcard prefix match via SOSL.
```sql
FIND 'Acm*' IN NAME FIELDS RETURNING Account(Id, Name)
```

34. Search by email fields only using SOSL.
```sql
FIND 'john@example.com' IN EMAIL FIELDS RETURNING Contact(Id, Email), Lead(Id, Email)
```

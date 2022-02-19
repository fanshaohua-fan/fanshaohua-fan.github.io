---
layout: post
title:  "AZ-204: Practice topic 3"
categories: az204
tags:   azure az204 practice
---

1. 


In app registration, select New registration

Select Azure AD

**Wrong**

In app registration, select New registration

Select Azure AD

Create a new application

https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app


### [Page 13](https://www.examtopics.com/exams/microsoft/az-204/view/13/)

2. AC   X BC

Prerequisites:

A working Azure AD tenant with at least an Azure AD Premium P1 or trial license enabled.

The recommended way to enable and use Azure AD Multi-Factor Authentication is with Conditional Access policies.

https://docs.microsoft.com/en-us/azure/active-directory/authentication/tutorial-enable-azure-mfa

3. C

4. B

5. Y

6. Y


### [Page 14](https://www.examtopics.com/exams/microsoft/az-204/view/14/)

7. 
Soft deletion

Purge protection

8. 
AD

9.
client_id, application, profile     X



10. 
UseAuth
UseAuthorization
UseAzureAppConfiguration

11. 
C

### [Page 15](https://www.examtopics.com/exams/microsoft/az-204/view/15/)

12. Y

13. Y   X

The requirement is to save scanned copies of patient intake forms.
Which is kind of unstructured data, should not be saved with cosmos db.
Using azure blob storage instead.

14. N

15. 
keyvault

keyvault key

vm

vm encryption

data X all

16. 
C


### [Page 16](https://www.examtopics.com/exams/microsoft/az-204/view/16/)

17. 
5->3->4->2->1

X

5->3->2->1->4

18. 
N

19. 
Y   X

Role-based access control is used for authorization and not authentication.

20. 
Device

iPhone  X iOS

Header

User_agent

iPhone

21. 
Y   X

### [Page 17](https://www.examtopics.com/exams/microsoft/az-204/view/17/)

22. 
Y

23. 
optionalClaims

requiredResourceAccess

https://docs.microsoft.com/en-US/azure/active-directory/develop/reference-app-manifest?WT.mc_id=Portal-Microsoft_AAD_RegisteredApps

![](/images/2022-02-16-21-25-15.png)

24. 
B

25. 
B   X   A

![](/images/2022-02-16-21-18-02.png)

26. 
D


### [Page 18](https://www.examtopics.com/exams/microsoft/az-204/view/18/)

27. 
D

28. 
Inbound

Inbound

Outbound
Outbound

29. 
SecretClient

ClientSecretCredential  X   DefaultAzureCredential

This example is using 'DefaultAzureCredential()' class from Azure Identity Library, which allows to use the same code across different environments with different options to provide identity. For more information about authenticating to key vault, see Developer's Guide.

https://docs.microsoft.com/en-us/azure/key-vault/secrets/quick-create-net#authenticate-and-create-a-client

30. 
AB

31.
AC  X   AD

The question requires to use SAS token, C is not correct.

https://docs.microsoft.com/en-us/rest/api/storageservices/create-user-delegation-sas#revoke-a-user-delegation-sas

### [Page 19](https://www.examtopics.com/exams/microsoft/az-204/view/19/)

32. 
Generate key blob-> Generate KEK-> Retrieve KEk-> key import

X

Generate KEK-> Retrieve KEk-> Generate key blob-> key import

User steps
To perform a key transfer, a user performs following steps:

1. Generate KEK.
2. Retrieve the public key of the KEK.
3. Using HSM vendor provided BYOK tool - Import the KEK into the target HSM and exports the Target Key protected by the KEK.
4. Import the protected Target Key to Azure Key Vault.

Customers use the BYOK tool and documentation provided by HSM vendor to complete Steps 3. It produces a Key Transfer Blob (a ".byok" file).

https://docs.microsoft.com/en-us/azure/key-vault/keys/byok-specification

33. 
A

34. 
YYYY

35. 
AC



36.
Y   X N

N

Y

![](/images/2022-02-19-16-45-14.png)

https://docs.microsoft.com/en-us/azure/cosmos-db/how-to-choose-offer

### [Page 20](https://www.examtopics.com/exams/microsoft/az-204/view/20/)

37. 

38. 

39. 

40. 

41. 

### [Page 21](https://www.examtopics.com/exams/microsoft/az-204/view/21/)

42. 

43. 

44. 

45. 

46. 
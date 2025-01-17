# test-task-php
Test Task for Backend (PHP)

# Homework assignment
## Situation
Bank allows private and business clients to `deposit` and `withdraw` funds to and from Bank accounts in multiple currencies. Clients may be charged a commission fee.

You have to create an application that handles operations provided in CSV format and calculates a commission fee based on defined rules. 

## Commission fee calculation
- Commission fee is always calculated in the currency of the operation. For example, if you `withdraw` or `deposit` in US dollars then commission fee is also in US dollars.
- Commission fees are rounded up to currency's decimal places. For example, `0.023 EUR` should be rounded up to `0.03 EUR`.

### Deposit rule
All deposits are charged 0.03% of deposit amount.

### Withdraw rules
There are different calculation rules for `withdraw` of `private` and `business` clients.

**Private Clients**
- Commission fee - 0.3% from withdrawn amount.
- 1000.00 EUR for a week (from Monday to Sunday) is free of charge. Only for the first 3 withdraw operations per a week. 4th and the following operations are calculated by using the rule above (0.3%). If total free of charge amount is exceeded them commission is calculated only for the exceeded amount (i.e. up to 1000.00 EUR no commission fee is applied).

For the second rule you will need to convert operation amount if it's not in Euros. Please use rates provided by [api.exchangeratesapi.io](https://api.exchangeratesapi.io/latest).
 

**Business Clients**
- Commission fee - 0.5% from withdrawn amount.

## Input data
Operations are given in a CSV file. In each line of the file the following data is provided:
1. operation date in format `Y-m-d`
2. user's identificator, number
3. user's type, one of `private` or `business`
4. operation type, one of `deposit` or `withdraw`
5. operation amount (for example `2.12` or `3`)
6. operation currency, one of `EUR`, `USD`, `JPY`

## Expected result
Output of calculated commission fees for each operation.

In each output line only final calculated commission fee for a specific operation must be provided without currency.

# Example usage
```
➜  cat input.csv 
2014-12-31,4,private,withdraw,1200.00,EUR
2015-01-01,4,private,withdraw,1000.00,EUR
2016-01-05,4,private,withdraw,1000.00,EUR
2016-01-05,1,private,deposit,200.00,EUR
2016-01-06,2,business,withdraw,300.00,EUR
2016-01-06,1,private,withdraw,30000,JPY
2016-01-07,1,private,withdraw,1000.00,EUR
2016-01-07,1,private,withdraw,100.00,USD
2016-01-10,1,private,withdraw,100.00,EUR
2016-01-10,2,business,deposit,10000.00,EUR
2016-01-10,3,private,withdraw,1000.00,EUR
2016-02-15,1,private,withdraw,300.00,EUR
2016-02-19,5,private,withdraw,3000000,JPY

➜  php script.php input.csv
0.60
3.00
0.00
0.06
1.50
0
0.70
0.30
0.30
3.00
0.00
0.00
8612
```
Note: the example output is calculated base on the following exchange rates: EUR:USD - 1:1.1497, EUR:JPY - 1:129.53.

# Requirements
1. Third party frameworks, libraries, dependencies, tools  are allowed.
    1. PSR-4 is required. We require the use of `composer` for autoloading even if you do not use any external dependencies;
    2. If your application is not based on framework skeleton, your project must use [this skeleton](https://github.com/bambooagile/test-task-php/raw/main/skeleton-commission-task-master.zip) as a bootstrap.
    3. Avoid including framework or any dependencies into your task if you don't actually use them;
2. Your application must be maintainable:
    1. Dependencies between separate parts of your code should be clear;
    2. Code should be consistent, readable, structured(with a clear flow);
3. Application must be both testable and tested.
    1. It **must** have unit tests. If you haven't written any previously, please take the time to get familiar with, before making the task.
    2. Unit tests must test the actual results and still pass even when the response from remote services change (this is quite normal, exchange rates change every day). This is best accomplished by using mocking.
4. Your application must be extensible:
    1. Adding new functionality or changing existing one should not require rewriting the system itself or it's core parts;
    2. Code structure should not depend on current concrete configuration. For example, adding new currency should not require changing code itself, like adding new methods or extending switch cases etc.
5. Code must be PSR-12 compatible;
6. A minimal documentation should be provided:
    1. How system should be run (what command to run);
    2. How to initiate system's tests (what command to run);
    3. Short description of functionality in more difficult places could be provided in the code itself;
7. Do not use external infrastructure dependencies, like MySQL databases or temporary files - just make the calculations in memory;
8. Do not use Bamboo Agile name in titles, descriptions or the code itself. This helps others to find the libraries that are really related to our services and/or are developed and maintained by our team.

# Evaluation Criteria
1. Requirements considered as met by Bamboo Agile development team. It would be best if all requirements are met;
2. Code quality - readability, structure, maintainability, extensibility, testability;
3. Speed of the system can also be considered, but is not as important as other criteria.

# Task Submission
You can put the code publicly (in github or similar code control systems) if you want, but please note the requirement about Bamboo Agile name usage.

Before submitting your solution, please look to the requirements once again – **all** of them **must** be accomplished.

Send it in your favourite format (link to versioned code, code in zip file etc.) to the following emails, unless instructed otherwise:
* sz@bambooagile.eu
* as@bambooagile.eu
* aa@bambooagile.eu

Please also note the time taken to complete the task – this helps us to improve the task in case it takes too long for most developers.

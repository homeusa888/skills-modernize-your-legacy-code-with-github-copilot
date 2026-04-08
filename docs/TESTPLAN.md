# COBOL Student Account Management System - Test Plan

This test plan covers the business logic and functionality of the COBOL-based student account management system. It is designed to validate the system's behavior with business stakeholders and will later be used to create automated unit and integration tests in the Node.js transformation.

## Test Cases

| Test Case ID | Test Case Description | Pre-conditions | Test Steps | Expected Result | Actual Result | Status (Pass/Fail) | Comments |
|--------------|----------------------|----------------|------------|-----------------|----------------|-------------------|----------|
| TC001 | Verify initial account balance | Application is compiled and ready to run | 1. Start the application<br>2. Select option 1 (View Balance) | Current balance displays as $1000.00 |  |  | Initial balance requirement |
| TC002 | Credit account with positive amount | Application is running with initial balance $1000.00 | 1. Select option 2 (Credit Account)<br>2. Enter amount: 500.00<br>3. Select option 1 to view balance | Balance displays as $1500.00 |  |  | Basic credit functionality |
| TC003 | Debit account with sufficient funds | Application is running with balance $1500.00 (after TC002) | 1. Select option 3 (Debit Account)<br>2. Enter amount: 300.00<br>3. Select option 1 to view balance | Balance displays as $1200.00 |  |  | Basic debit functionality |
| TC004 | Debit account with insufficient funds | Application is running with balance $1200.00 (after TC003) | 1. Select option 3 (Debit Account)<br>2. Enter amount: 1500.00 | Error message: "Insufficient funds for this debit." Balance remains $1200.00 |  |  | Funds validation |
| TC005 | Multiple credit operations | Application is running with balance $1200.00 | 1. Select option 2 (Credit Account)<br>2. Enter amount: 100.00<br>3. Select option 2 again<br>4. Enter amount: 200.00<br>5. Select option 1 to view balance | Balance displays as $1500.00 |  |  | Sequential operations |
| TC006 | Multiple debit operations | Application is running with balance $1500.00 | 1. Select option 3 (Debit Account)<br>2. Enter amount: 100.00<br>3. Select option 3 again<br>4. Enter amount: 200.00<br>5. Select option 1 to view balance | Balance displays as $1200.00 |  |  | Sequential operations |
| TC007 | Credit zero amount | Application is running with balance $1200.00 | 1. Select option 2 (Credit Account)<br>2. Enter amount: 0.00<br>3. Select option 1 to view balance | Balance remains $1200.00 |  |  | Edge case: zero credit |
| TC008 | Debit zero amount | Application is running with balance $1200.00 | 1. Select option 3 (Debit Account)<br>2. Enter amount: 0.00<br>3. Select option 1 to view balance | Balance remains $1200.00 |  |  | Edge case: zero debit |
| TC009 | Credit exact balance amount | Application is running with balance $1200.00 | 1. Select option 2 (Credit Account)<br>2. Enter amount: 1200.00<br>3. Select option 1 to view balance | Balance displays as $2400.00 |  |  | Large credit amount |
| TC010 | Debit entire balance | Application is running with balance $2400.00 | 1. Select option 3 (Debit Account)<br>2. Enter amount: 2400.00<br>3. Select option 1 to view balance | Balance displays as $0.00 |  |  | Debit to zero |
| TC011 | Attempt debit after zero balance | Application is running with balance $0.00 | 1. Select option 3 (Debit Account)<br>2. Enter amount: 100.00 | Error message: "Insufficient funds for this debit." Balance remains $0.00 |  |  | Zero balance validation |
| TC012 | Mixed credit and debit operations | Application restarted with fresh balance $1000.00 | 1. Credit $200.00<br>2. Debit $150.00<br>3. Credit $300.00<br>4. Debit $250.00<br>5. View balance | Balance displays as $1100.00 |  |  | Complex operation sequence |
| TC013 | Invalid menu selection | Application is running | 1. Enter invalid choice (e.g., 5) | Error message: "Invalid choice, please select 1-4." Menu redisplays |  |  | Input validation |
| TC014 | Application exit | Application is running | 1. Select option 4 (Exit) | Application terminates with message: "Exiting the program. Goodbye!" |  |  | Clean exit |
| TC015 | Balance persistence across operations | Application is running, multiple operations performed | 1. Perform various credit/debit operations<br>2. View balance after each operation | Balance correctly reflects cumulative changes from all operations |  |  | Data persistence in memory |
| TC016 | Decimal amount handling | Application is running with balance $1000.00 | 1. Credit $123.45<br>2. Debit $67.89<br>3. View balance | Balance displays as $1055.56 |  |  | Decimal precision handling |
| TC017 | Large amount handling | Application is running with balance $1000.00 | 1. Credit $999999.99<br>2. View balance | Balance displays as $1000999.99 |  |  | Large number handling |
| TC018 | Negative amount credit (edge case) | Application is running with balance $1000.00 | 1. Select option 2 (Credit Account)<br>2. Enter amount: -100.00<br>3. View balance | Balance becomes $900.00 (system allows negative credits) |  |  | Note: Current system doesn't validate positive amounts |
| TC019 | Negative amount debit (edge case) | Application is running with balance $1000.00 | 1. Select option 3 (Debit Account)<br>2. Enter amount: -100.00<br>3. View balance | Balance becomes $1100.00 (negative debit adds to balance) |  |  | Note: Current system doesn't validate positive amounts |
| TC020 | Restart application maintains initial balance | Application restarted | 1. Immediately view balance | Balance displays as $1000.00 (fresh start) |  |  | Application restart behavior |

## Test Execution Notes

- **Environment**: COBOL application compiled with `cobc` and run in terminal
- **Data Setup**: Each test case specifies pre-conditions for balance state
- **Execution Order**: Tests should be run in sequence where dependencies exist (e.g., TC003 depends on TC002)
- **Cleanup**: Restart application between independent test sequences
- **Tools**: Manual execution with observation of terminal output
- **Coverage**: Tests cover all business rules, edge cases, and error conditions

## Business Rules Validation

This test plan validates the following business requirements:
- Initial account balance of $1000.00
- Unlimited credit operations
- Debit operations only with sufficient funds
- Balance persistence across operations within a session
- Proper error messaging for insufficient funds
- Clean application exit
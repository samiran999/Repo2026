1. In 1 Transaction only 100 SOQL Query can be done, sp instead use List with In statement
2. Set is better than List as it removes duplicate
3. In 1 Transaction Max 150 DML is allowed, thats why never do update in loop instead do a bulk update
4. You don't need dml in before update
5. rigger.Old is alwys read only
6. So if a user tries to close an account and your before trigger catches it and calls `addError()`, the account won't save and the user sees the error message you provided.
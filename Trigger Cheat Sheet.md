1. Mainly having 1 Trigger is best option
2. instead of multiple use Trigger framework
3. SF doesn't have controll on which trigger to run in case of multiple trigger
4. Trigger Format 
# ⚡ Salesforce Apex Trigger Context Variables

> To access the records that caused the trigger to fire, use **context variables**.  
> All context variables are accessed via the `Trigger` class — e.g., `Trigger.new`, `Trigger.isInsert`

---

## 📑 Table of Contents

- [🔷 Context Checking Variables](#-context-checking-variables)
- [🔷 Record Access Variables](#-record-access-variables)
- [📊 Availability Reference Table](#-availability-reference-table)
- [📊 new vs newMap vs old vs oldMap](#-new-vs-newmap-vs-old-vs-oldmap)
- [💡 Real Code Example](#-real-code-example)
- [⚠️ Important Rules](#️-important-rules)

---

## 🔷 Context Checking Variables

> These return `true` or `false` to tell you **what operation fired** the trigger and **when**.

| Variable | Returns `true` when... |
|---|---|
| `Trigger.isExecuting` | Current context is a trigger (not Visualforce, Web Service, or `executeAnonymous()`) |
| `Trigger.isInsert` | Trigger fired due to an **insert** operation (UI, Apex, or API) |
| `Trigger.isUpdate` | Trigger fired due to an **update** operation (UI, Apex, or API) |
| `Trigger.isDelete` | Trigger fired due to a **delete** operation (UI, Apex, or API) |
| `Trigger.isBefore` | Trigger fired **before** any record was saved |
| `Trigger.isAfter` | Trigger fired **after** all records were saved |
| `Trigger.isUndelete` | Trigger fired after a record was **recovered from Recycle Bin** (UI, Apex, or API) |

---

## 🔷 Record Access Variables

> These give you access to the **actual record data** that caused the trigger to fire.

### `Trigger.new`
- Returns a **List** of new versions of sObject records
- ✅ Available in → `insert`, `update`, `undelete` triggers
- ✏️ Records can only be **modified** in `before` triggers
```java
for(Account acc : Trigger.new) {
    System.debug(acc.Name);
}
```

---

### `Trigger.newMap`
- Returns a **Map of ID → new versions** of sObject records
- ✅ Available in → `before update`, `after insert`, `after update`, `after undelete`
- 💡 Use when you need to find a record **by its ID**
```java
Account acc = Trigger.newMap.get(someId);
```

---

### `Trigger.old`
- Returns a **List** of old versions of sObject records
- ✅ Available in → `update` and `delete` triggers only
- 💡 Use to see **what the record looked like before** the change
```java
for(Account acc : Trigger.old) {
    System.debug('Old Name: ' + acc.Name);
}
```

---

### `Trigger.oldMap`
- Returns a **Map of ID → old versions** of sObject records
- ✅ Available in → `update` and `delete` triggers only
- 💡 Use to **compare old vs new** values by ID
```java
Account oldAcc = Trigger.oldMap.get(acc.Id);
```

---

## 📊 Availability Reference Table

| Variable | Before Insert | Before Update | Before Delete | After Insert | After Update | After Delete | After Undelete |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| `Trigger.new` | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ |
| `Trigger.newMap` | ❌ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ |
| `Trigger.old` | ❌ | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ |
| `Trigger.oldMap` | ❌ | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ |

---

## 📊 new vs newMap vs old vs oldMap

| | `new` | `newMap` | `old` | `oldMap` |
|---|---|---|---|---|
| **Type** | List | Map (Id → Record) | List | Map (Id → Record) |
| **Data** | Current/New values | Current/New values | Previous values | Previous values |
| **Access by ID?** | ❌ No | ✅ Yes | ❌ No | ✅ Yes |
| **Available on Insert?** | ✅ Yes | ✅ After only | ❌ No | ❌ No |
| **Available on Update?** | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| **Available on Delete?** | ❌ No | ❌ No | ✅ Yes | ✅ Yes |
| **Can modify records?** | ✅ Before only | ❌ No | ❌ No | ❌ No |

---

## 💡 Real Code Example

```java
trigger AccountTrigger on Account (
    before insert,
    before update,
    before delete,
    after insert,
    after update,
    after delete,
    after undelete
) {

    // ✅ BEFORE INSERT — New records, not saved yet, can modify
    if (Trigger.isInsert && Trigger.isBefore) {
        for (Account acc : Trigger.new) {
            if (acc.Name == null) {
                acc.Name = 'Default Name';  // ✏️ Can modify in before trigger
            }
        }
    }

    // ✅ AFTER INSERT — Records saved, get IDs via newMap
    if (Trigger.isInsert && Trigger.isAfter) {
        for (Id accId : Trigger.newMap.keySet()) {
            System.debug('Inserted Account ID: ' + accId);
        }
    }

    // ✅ BEFORE UPDATE — Compare old vs new before saving
    if (Trigger.isUpdate && Trigger.isBefore) {
        for (Account acc : Trigger.new) {
            Account oldAcc = Trigger.oldMap.get(acc.Id);
            if (acc.Name != oldAcc.Name) {
                System.debug('Name changing from: ' + oldAcc.Name + ' to: ' + acc.Name);
            }
        }
    }

    // ✅ AFTER UPDATE — Compare old vs new after saving
    if (Trigger.isUpdate && Trigger.isAfter) {
        for (Account acc : Trigger.new) {
            Account oldAcc = Trigger.oldMap.get(acc.Id);
            if (acc.Phone != oldAcc.Phone) {
                System.debug('Phone changed!');
            }
        }
    }

    // ✅ BEFORE DELETE — Access records being deleted via old
    if (Trigger.isDelete && Trigger.isBefore) {
        for (Account acc : Trigger.old) {
            System.debug('About to delete: ' + acc.Name);
        }
    }

    // ✅ AFTER UNDELETE — Records restored from Recycle Bin
    if (Trigger.isUndelete) {
        for (Account acc : Trigger.new) {
            System.debug('Restored: ' + acc.Name);
        }
    }
}
```

---

## ⚠️ Important Rules

- 📝 **Records can only be modified** using `Trigger.new` in `before` triggers — not in `after` triggers
- 🆔 **`Trigger.new`** does NOT have IDs in `before insert` (records not saved yet) — use `after insert` + `Trigger.newMap` to get IDs
- 🔄 **`Trigger.old`** and **`Trigger.oldMap`** are **never available** on insert triggers (no previous version exists)
- ❌ **`Trigger.new`** and **`Trigger.newMap`** are **never available** on delete triggers (records are being removed)
- ⚡ Always use **bulkified** triggers — never put SOQL or DML inside loops!

```java
// ❌ WRONG — SOQL inside loop (hits governor limits)
for (Account acc : Trigger.new) {
    List<Contact> contacts = [SELECT Id FROM Contact WHERE AccountId = :acc.Id];
}

// ✅ RIGHT — Bulkified approach
Set<Id> accountIds = Trigger.newMap.keySet();
List<Contact> contacts = [SELECT Id FROM Contact WHERE AccountId IN :accountIds];
```

---

> ✍️ *Salesforce Apex Trigger Context Variables reference — built while learning Salesforce development*


--------------------------------------------------------------------------

5. No insert & Update operation in before trigger
6. So if a user tries to close an account and your before trigger catches it and calls `addError()`, the account won't save and the user sees the error message you provided.
7. In 1 Transaction only 100 SOQL Query can be done, sp instead use List with In statement
8. Set is better than List as it removes duplicate
9. In 1 Transaction Max 150 DML is allowed, thats why never do update in loop instead do a bulk update
10. You don't need dml in before update
11. rigger.Old is alwys read only
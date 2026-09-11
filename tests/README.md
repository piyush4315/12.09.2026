# C6 formula — trigger identification tests

Each file below is the **original Unrepaired.xlsm with exactly one variable changed**
(in the C6 formula of the *Buyer Groups* sheet). Open each file in Excel and note
whether the **"We found a problem with some content"** dialog appears.

| File | C6 contains | Variable isolated |
|---|---|---|
| `Test0_Control.xlsm` | the original formula (control) | — (expected to show the dialog) |
| `Test1_RefEP.xlsm` | same, but `$EP$1` → `$A$1` | the `$EP$1` flag-cell reference |
| `Test2_ShortText.xlsm` | same structure, short plain-English text | the long/Unicode message text |
| `Test3_NoNestedIF.xlsm` | same as Test2, without the `&IF(...)` tail | the nested `IF` inside the concatenation |
| `Test4_Trivial.xlsm` | `1+1` | whether cell C6 / the file itself is the problem |
| `Test5_Position.xlsm` | C6 empty; full formula moved to E6 | the cell position of the formula |

## How to interpret

- **All six show the dialog** → the trigger is *global* (not the formula's content).
  Say "all six failed" and I'll switch to rebuilding the workbook on Excel's repaired package.
- **Test4 fails** but others pass → the problem is tied to cell C6 itself, not its content.
- **Test4 passes, Test1 passes, Test0 fails** → the `$EP$1` reference is the trigger.
- **Test4 passes, Test1 fails, Test2 passes** → the long/Unicode text is the trigger.
- **Test2 fails, Test3 passes** → the nested `&IF(...)` is the trigger.

The pattern of pass/fail pinpoints the root cause in one round, after which I can
deliver a version that keeps the live formula (and the double-click lift feature).

Note: the main `Unrepaired.xlsm` in the repo root now uses **static text** in C6
(the same outcome Excel's own repair produced) — that one should open cleanly;
the double-click "lift blocks" feature is then inactive, as in Excel's Repaired.xlsm.

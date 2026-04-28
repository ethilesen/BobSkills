# Code Historian Mode Rules

## Core Principle
ALWAYS preserve old code as comments when making ANY modification. This is NON-NEGOTIABLE.

## Comment Format by Language

**Modern Languages (JS, TS, Python, Java, C, etc.):**
```
// YYYY-MM-DD HH:MM:SS: [reason]
// OLD CODE START
// [original line 1]
// [original line 2]
// OLD CODE END

[new code]
```

**RPG Fixed-Format:**
```
* YYYY-MM-DD HH:MM:SS: [reason]
* OLD CODE START
*[original line 1]
*[original line 2]
* OLD CODE END
[new code]
```

**RPG Free-Format:**
```
// YYYY-MM-DD HH:MM:SS: [reason]
// OLD CODE START
// [original line 1]
// [original line 2]
// OLD CODE END

[new code]
```

**SQL:**
```
-- YYYY-MM-DD HH:MM:SS: [reason]
-- OLD CODE START
-- [original line 1]
-- [original line 2]
-- OLD CODE END

[new code]
```

## Rules
1. Use current date and time in YYYY-MM-DD HH:MM:SS format (24-hour clock)
2. Provide clear, concise reason (e.g., "Fixed null pointer", "Added error handling")
3. Maintain original indentation in commented code
4. For blocks >20 lines, use summary comment instead
5. Each logical change gets its own comment block
6. NEVER skip this pattern - it's mandatory

## Summary Format (for large changes)
```
// YYYY-MM-DD HH:MM:SS: [reason]
// OLD CODE: [brief description of what was removed]
// (Original XX lines removed - see git history for details)

[new code]
```

## Examples

**JavaScript:**
```javascript
// 2026-04-27 14:30:45: Changed to async/await for better error handling
// OLD CODE START
// function getData() {
//   return fetch('/api').then(r => r.json());
// }
// OLD CODE END

async function getData() {
  const res = await fetch('/api');
  return await res.json();
}
```

**RPG Free-Format:**
```rpgle
// 2026-04-27 09:15:30: Added validation for empty customer ID
// OLD CODE START
// chain custId CUSTOMER;
// custName = CUSNAM;
// OLD CODE END

if custId = *blank;
  custName = '*INVALID*';
else;
  chain custId CUSTOMER;
  if %found(CUSTOMER);
    custName = CUSNAM;
  else;
    custName = '*NOT FOUND*';
  endif;
endif;
```

**RPG Fixed-Format:**
```rpg
* 2026-04-27 16:20:15: Fixed calculation overflow
* OLD CODE START
*     C                   EVAL      TOTAL = QTY * PRICE
* OLD CODE END
     C                   EVAL      TOTAL = %DEC(QTY * PRICE:15:2)
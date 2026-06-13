# AI301 Contribution Log — Aisha

## Phase I — Issue Selection

**Issue:** [Escape character not working while using regular expressions in web interface #1486](https://github.com/ivre/ivre/issues/1486)

**Problem Summary:**
In IVRE's web interface, backslash escape characters are stripped when 
using regular expressions in search filters. This means users cannot 
search for special regex characters like `.` or `*` — the backslash 
is removed before the query reaches the search engine, returning 
incorrect results.

**Why I Chose This Issue:**
I chose this issue because it's a well-scoped frontend bug in a real 
network recon framework used by security professionals. The maintainer 
confirmed it's a genuine bug and explicitly labeled it help wanted, 
signaling they want outside contributions. The bug lives in the web 
interface layer, which aligns with my JavaScript/TypeScript skills.

The fix is clearly defined: backslash characters should be preserved 
and passed correctly to the search engine. The maintainer even 
acknowledged the frontend is not their strength, which means a 
contributor who can navigate frontend code will be especially valuable 
here. I'm also interested in learning how security tools handle 
regex input sanitization, which is a pattern relevant to many 
backend and security engineering roles.

Left a comment on the issue asking for pointers on the relevant 
frontend file.

**Status:** Phase I Complete

## Phase II — Reproduce & Plan

### Reproduction Process

#### Environment Setup
Cloned fork locally on Windows. No server setup needed — reproduced 
the bug through code analysis by tracing the `parse_params` function 
in `web/static/ivre/params.js`. Implemented the fix directly on the 
working branch.
**Working branch:** https://github.com/aishsidya0402-netizen/ivre/tree/fix-issue-1486

#### Steps to Reproduce
1. Clone the ivre repository
2. Open `web/static/ivre/params.js`
3. In `parse_params`, locate the state machine (states 0, 1, 2)
4. Trace what happens when a backslash `\` is typed in a filter — e.g. `hostname:/\.google\./`
5. **Expected:** `curtoken` receives `\.google\.` (backslash preserved)
6. **Actual:** `curtoken` only receives `.google.` — the backslash is added to `curtokenprotected` (display only) but never to `curtoken` (the value sent to the API)

### Solution Approach

**Understand:** In `parse_params`, when a backslash is encountered in states 0, 1, and 2, the code adds it to `curtokenprotected` (what's shown in the UI) but not to `curtoken` (what gets sent to the backend API). The backslash silently disappears from the query.

**Match:** States 3, 4, 5, and 6 in the same function correctly add characters to both `curtoken` and `curtokenprotected`. The fix follows the same pattern already used throughout the function.

**Plan:**
1. In state 0, `case "\\"`: add `curtoken += curchar;`
2. In state 1, `case "\\"`: add `curtoken += curchar;`
3. In state 2, `case "\\"`: add `curtoken += curchar;`
4. Verify no other filter types are broken by the change

**File changed:** `web/static/ivre/params.js`

**Review:** Will check `CONTRIBUTING.md` before opening PR.

**Evaluate:** After fix, a regex filter like `hostname:/\.google\./` should pass the backslash through to the API correctly.

**Status:** Phase II Complete

# AI301 Contribution Log — Aisha

## Cycle 1

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

## Phase III — Implementation Notes

### What I Built
- Fixed backslash stripping bug in `web/static/ivre/params.js`
- Added `curtoken += curchar;` in 3 places in the `parse_params` 
  state machine (states 0, 1, and 2) so backslashes are preserved 
  in the actual query value sent to the API
- Wrote a JavaScript test file `tests/test_params_regex.js` with 
  3 test cases verifying backslash preservation

### Challenges Faced
- Docker setup for ivre required multiple containers — pivoted to 
  code-level reproduction instead
- Node.js was not installed — installed v24.17.0 to run JS tests
- No existing JS test suite in the project, so wrote a standalone 
  Node.js test file

### Testing Strategy
- Created `tests/test_params_regex.js` with 3 unit tests:
  - Test 1: backslash preserved in regex filter (`\.google\.`)
  - Test 2: normal filter without backslash still works
  - Test 3: multiple backslashes preserved (`\d+\.\d+`)
- All 3 tests pass: `3 passed, 0 failed`

### Code Changes
- **Branch:** https://github.com/aishsidya0402-netizen/ivre/tree/fix-issue-1486
- **Files modified:** `web/static/ivre/params.js`
- **Test file added:** `tests/test_params_regex.js`

**Status:** Phase III Complete

## Phase IV — Pull Request

**PR Link:** https://github.com/ivre/ivre/pull/1912

**What I contributed:** Fixed backslash escape characters being stripped 
from regex filter input in the web interface by correcting the 
`parse_params` state machine in `web/static/ivre/params.js`.

**Maintainer Feedback:** Awaiting review — tagged @p-l- in PR comment.

**Status:** Awaiting review

**Status:** Phase IV Complete


## Cycle 2

## Phase I — Issue Selection

**Issue:**  Rework UI of settings wear favorite screen to use Material3 #6300

**Problem Summary:**
The Home Assistant Android app's Wear OS settings "favorites" screen (LoadSettingsHomeView) is still built with the app's legacy XML-based UI system. The maintainers are migrating the app to Jetpack Compose and Material3, and this screen is one of several tracked under a parent migration issue (#5420, "Migrate to material 3") that still needs to be reworked.

**Why I Chose This Issue:**
I chose issue #6300, "Rework UI of settings wear favorite screen to use Material3," because it's a well-scoped UI migration task in a large, actively maintained project, and it lets me build Android/Kotlin experience alongside my Python background. It's labeled good first issue by a maintainer, and sits under a larger tracked migration effort (parent issue #5420) that's still early — no risk of competing with other contributors for it.
## I'm interested in this because:

It's concrete and self-contained: port one screen (LoadSettingsHomeView) from XML to Compose/Material3, not an open-ended request.
Home Assistant has real maintainer engagement — issues here are triaged deliberately, not left stale.
I want to learn how a mature Android codebase handles a large-scale UI framework migration, which I haven't worked with before.

From reading the issue, the problem is that this screen still uses the app's older XML view system, inconsistent with the rest of the app now that most screens have moved to Compose/Material3. My contribution brings this screen in line with that effort.
Commented on the issue to indicate I'd be working on it,and checked the claim box on the course tracker sheet. 

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

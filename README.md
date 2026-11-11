# consulting-site
Claude Code skill that one-shots a single-page consulting-business website from a short interview: hero with the business name front and center, services grid, optional stats / process / about / testimonials sections, and a contact section whose form is a GHL embed, Formspree, or mailto button depending on the client.

Install
Clone into your Claude Code skills directory:

git clone <this-repo-url> ~/.claude/skills/consulting-site
Then in any Claude Code session:

/consulting-site Acme Advisors
Claude interviews you for the client variables (business name, what they do, email, services, accent color, optional sections), writes a config.json, and renders the site.

Outputs
Per client, in ~/consulting-sites/<slug>/:

File	Use
site.html	Complete document — local preview, own-domain hosting, Cloudflare Pages
site.ghl.html	Paste block for a GoHighLevel Custom Code element
config.json	The single source of truth — edit this and re-render, never the HTML
Optional deploy: npx wrangler pages deploy deploy/ --project-name <slug> (re-render with --image-mode relative first — see SKILL.md §4).

How it works
scripts/render.py — stdlib-only Python, no dependencies. {{TOKEN}} substitution, <!-- IF:path --> conditionals, <!-- EACH:list --> loops.
assets/site.template.html — the one self-contained page template.
config.example.json — fully-populated fictional example; regression fixture and copy-voice reference.
Missing optional values render visible placeholders and print a DEFERRED punch list instead of blocking. Leftover tokens are a hard error.
Requirements
Claude Code with any signed-in Claude account
git (to clone this repo)
Python 3.9+ — preinstalled on macOS/Linux; on Windows install from python.org or winget install Python.Python.3.12 (the render script is stdlib-only, no pip packages)
Node.js only if deploying to Cloudflare Pages (npx wrangler)
Works on macOS, Linux, and Windows. On Windows the interview/render commands use python instead of python3 and start instead of open — Claude handles this automatically per the SKILL.md instructions.

Notes
Never paste site.html into GHL — a complete HTML document inside a GHL Custom Code element renders a blank white screen. Use site.ghl.html.
No analytics keys ship by default; add the client's own Meta pixel / PostHog key under tracking.* in their config if provided.

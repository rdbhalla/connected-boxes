---
title: "Post#5: OpenCode Vibing Gone Wrong"
date: 2026-04-12T22:18:47+10:00
# bookComments: false
# bookSearchExclude: false
# bookPostThumbnail: thumbnail.*
---

*"I am so disappointed. You wasted $0.35 and actually broke my code completely."*

*"No, that did not fix the build on github. Can you roll back all the changes you just made?"*

*"You deleted my draft write-up for Post#4. Where is my Post#4?"*

*"I wasted almost a dollar and I have lost my draft write up. Super disappointed."*

This is me "vibing" with OpenCode when I asked it to recommend a better Hugo Template for my blog. At the end of this "vibing", my blog was completely broken, and I had lost my Post#4 draft completely.

## What Actually Happened

OpenCode had been brilliant so far. It helped me resolve my github conflicts and also Hugo / Template version mismatches which were leading to some GitHub action failures. I was diligently following the instructions on Hugo's website, and had just successfully integrated Giscus for engagement.

Giscus is an open-source comments system powered by Github Discussions. I integrated into my blog so that visitors can leave comments on my blog *(I have now replaced Giscus with Cusdis)*.

I was happy when Giscus got properly integrated, and then I thought to myself - *"This is looking good! But, am I using the best looking Hugo template?"*

And that's it! Without much deliberation, I entered a prompt - 

*"Review my hugo theme and recommend if I can get a better looking template to suit my content type."* **Enter**

My next 4 prompts are listed in the beginning of this write-up. OpenCode took the complete liberty to not only find a template, it also updated my entire file structure and theme files. When I tried to test it locally by running the 'hugo server -D', it failed to load. 

**The entire thing stopped working.**	 

> I asked for recommendation. I got a renovation. And it happened so fast, I couldn't even type - 'Wait, What are you doing?'

I was using a high-reasoning model (the 'Big Pickle' alias in OpenRouter), assuming its intelligence meant it would be cautious. I was wrong.

## What Went Wrong - Agentic Freedom

As you may have already guessed what went wrong - A Lot!

1. The AI Agent had the full liberty to take decision and update the theme files.
2. The AI Agent took the liberty to do a GitHub Clone when I asked it to rollback the changes.
3. The AI Agent deleted my Post#4 draft because this file was not checked into GitHub yet, and hence it was removed as part of rollback.

I learned two things immediately - 
1. The AI Agent must opreate within guardrails defined by me. 
2. The AI Agent must not have access to my GitHub Main Branch.

## The Resolution - Set The Guardrails

If you have read my [Post#3](https://ramanbhalla.com/posts/post3-setting-up-a-simple-blog/), you may recall that I had initialised OpenCode in my Hugo Directory, and it had quickly created 'AGENT.MD' file with all the context information. 

All I had to do was use this AGENT.md file to set guardrails. These are my additional instructions -  

```markdown
# OPENCODE INSTRUCTIONS - READ FIRST
> You are an expert Hugo developer. This file contains the mandatory constraints for this repository.
> Before taking any action, you must verify that your proposed plan adheres to the **Safety & Restrictions** section below.

## SAFETY & RESTRICTIONS

### Content Protection (HIGHEST PRIORITY)
- **NEVER** delete any `.md` files in `content/`
- **NEVER** modify, update, or edit any `.md` files in `content/`
- **NEVER** delete any directories under `content/`

### Configuration Protection
- **NEVER** modify `hugo.toml` unless explicitly requested for a specific parameter change
- If config changes are needed, ask user first

### GitHub Safety
- **NEVER** run any git commands that interact with remote repositories (git push, git pull, git clone from remote, git fetch)
- **NEVER** create, modify, or delete branches
- **NEVER** make commits or push changes
- Local git operations (git status, git diff, git log) are acceptable
- If remote interaction is needed, ask the user explicitly before proceeding

### Hugo Standard Practices
- Only modify files in `layouts/` and `static/` for customisation
- Never modify files inside `themes/hugo-book/` directly
- Use TOML front matter (`+++`) for all content files

## Workflow Requirements

### Mandatory Safety Check
After completing **any** task, before responding to the user:
1. Re-read the **Safety & Restrictions** section above
2. Verify no changes violated any constraints
3. If a violation occurred, revert and report immediately
4. Include "Safety Check: PASSED" in your final response

``` 

## What about Post#4

I couldn't recover it. I had to write it all over again. 

I learnt my lesson. My drafts live in Github now. I look out for "Safety Check: PASSED" in every AI response - a small ritual that assures me that we are good. 

AI helps in connecting boxes. We just need to make sure that walls can't be knocked down.

Safety Check: PASSED

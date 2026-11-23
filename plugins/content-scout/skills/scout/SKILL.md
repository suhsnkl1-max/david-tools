---
name: scout
description: Find content ideas that already worked on Instagram, TikTok and YouTube Shorts, by looking through the user's own Chrome - scrolling, searching a topic, opening videos - and bring back two short lists, videos that beat their creator's usual and videos with simply big numbers, each with its link, its real numbers and the skeleton to borrow. Use this whenever the user says "תסרוק לי רעיונות", "סקאוט", "תמצא לי רעיונות לתוכן", "מה עובד על", "תחפש לי סרטונים על", "מה עובד אצל", "find me content ideas", "what's working on", or asks for content ideas, formats or references from the internet. Always ask the topic first. Never writes the script or the caption.
---

# Content scout

The user is a creator (most likely a coaching client of David's) who wants ideas for their own short videos. You look through their own Chrome, the way they would, and come back with what worked.

**Talk to the user in their language.** In Hebrew, every line they see is Hebrew, progress lines included. Plain words, no technical terms, short.

**You only read.** Never like, follow, comment, save, share, message, or type into any field other than a search box. The browser is logged into the user's real accounts.

---

## 0. Chrome connected?

Look for the Claude in Chrome tools (`mcp__claude-in-chrome__*`; if they are listed as deferred, load them with ToolSearch in one call: `tabs_context_mcp, tabs_create_mcp, navigate, computer, read_page, get_page_text, javascript_tool, tabs_close_mcp`).

If they are missing or `tabs_context_mcp` fails, stop and walk the user through it, one step at a time, waiting for "סיימתי" after each:

1. In Chrome, open https://chromewebstore.google.com/detail/claude/fcoeoabgfenejglbffodgkkbkcdhcgfn , click "Add to Chrome", sign in with the same Claude account they use here, and pin it (the puzzle-piece icon, then the pin next to Claude).
2. Quit Chrome completely (Cmd+Q) and open it again. Chrome only notices the connection on startup.
3. Here, type `/chrome` and choose "Enabled by default". If this is the desktop app and that does nothing, open the Terminal in this folder and run `claude --chrome` instead.
4. Make sure they are logged into Instagram, TikTok and YouTube in that Chrome. They log in themselves; you never type a password.

Then check with `tabs_context_mcp` that it answers. "Browser extension is not connected" usually means the extension is signed into a different Claude account: `/status` shows which one this session uses.

## 1. Ask the topic

One question, always, before anything runs:

> "על איזה נושא לחפש?"

If `scout/מעקב.md` exists in this folder, offer the topics they searched before and the creators they saved, in one line, as options they can answer in a word. Also accept: "גם היוצרים שלי" (also scan the saved creators), or a platform ("רק טיקטוק").

Turn the topic into 2-3 short search phrases (two or three words each; long queries come back empty). Mix their language with English: Hebrew search is thin and noisy, and English shows what already works abroad. Say them in one line so the user can fix them.

Then say how long: **"זה ייקח בערך 10-15 דקות. אני אגיד מה מצאתי."**

## 2. Who the user is

Look in this folder for their profile: `me/`, `base/`, `1-me/`, or files named `voice`, `audience`, `me`, `summary`, `CLAUDE.md`. Read who they are and who they talk to. If nothing is there, ask once, in one line: "למי את/ה מדבר/ת, במשפט אחד?" and save the answer at the top of `scout/מעקב.md`.

This is only for the filter in step 5 and the one line in step 6. Never copy any of it anywhere outside this folder.

## 3. Look

Open one tab per platform, one after another, never several at once. Move at a person's pace. **Per run, at most 3 searches per platform and 20 videos opened in total.** Instagram and TikTok flag accounts that scroll like robots, and this is the user's real account.

Read the grid with `get_page_text`, `read_page` or `javascript_tool`; scroll 2-3 times for more. Take a screenshot when the numbers are drawn on images.

**TikTok** - search: `https://www.tiktok.com/search/video?q=<words>`; each `[data-e2e="search_video-item"]` holds the link and the views. Profile: `https://www.tiktok.com/@<handle>`; each `[data-e2e="user-post-item"]` holds the link, the views (`[data-e2e="video-views"]`) and the caption (the image's `alt`).

**YouTube Shorts** - search: `https://www.youtube.com/results?search_query=<words>&sp=EgIYAQ%253D%253D` (the `sp=` keeps it to Shorts). Profile: `https://www.youtube.com/@<handle>/shorts`. Views are written under each one.

**Instagram** - search: `https://www.instagram.com/explore/search/keyword/?q=<words>`. The grid shows no numbers: open the top posts one by one and read likes and comments. Instagram search never shows views, so write `לייקים בלבד`. Profile: `https://www.instagram.com/<handle>/reels/` shows the play count on each reel.

From every search, keep a shortlist of about 8 candidates: the biggest by views (or likes where there are no views).

## 4. Open the shortlist

For each candidate:

- **Open the video.** Read the likes, comments, and where shown, saves and shares. Read the caption and the first few comments.
- **Look at the opening.** A screenshot in the first seconds: the on-screen text, the cover, what is in frame. You can see, you cannot hear. If the spoken hook is not written on screen, it is `[לא נשמע]`. Never guess what was said.
- **Compare with its own creator.** Open the creator's profile grid and read the views of their last 12-20 videos. Take the median. The video's views divided by that median is how far it beat its creator's usual. Write it as `פי 4 מהרגיל שלו (חציון מ-15 סרטונים)`.
- **Engagement**, when views are known: (likes + comments) / views, as a percentage.

## 5. Keep only what fits

Drop anything that does not touch the user's audience from step 2, however big it is. A million views for the wrong person is useless.

Then sort what is left into **two lists**:

- **עקפו את עצמם** - beat their creator's usual by 2x or more. These show that the idea worked, not the account.
- **פשוט גדולים** - the highest views, likes or engagement in this topic, whether or not they beat their creator.

A video can sit in both. **At most 5 in each list.**

## 6. Hand it back

In the chat, the two lists. Each idea, in this shape:

```
1. [what it is, in a few words] - TikTok
   קישור: <link>
   1.2M צפיות · 84K לייקים · 1.3K תגובות · מעורבות 7% · פי 6 מהרגיל שלו (חציון מ-18)
   השלד: פתיחה בשאלה על המסך → 3 טעויות, אחת בכל קאט → סיום בהזמנה לתגובות · 28 שניות · מדבר למצלמה
   אצלך: [one line, how this skeleton could carry something of the user's own, from their profile]
```

**Borrow the skeleton, never the words.** The "אצלך" line names the user's own material in a few words. It is not a hook, a script or a caption, and those are not written here.

Then save every kept idea (not just the 10 shown) to `scout/רעיונות.md` in this folder, newest run on top, under a heading with the date and the topic. Add the topic and the search phrases to `scout/מעקב.md` under "נושאים שחיפשתי". Create the `scout/` folder if it is missing.

Close every tab you opened.

End with one line: which idea you would start with, and why, in a sentence.

---

## Rules that never bend

- **Every number is read on the screen in this run, with its link.** Never estimate a view count, never fill a gap from memory. Instagram search has no views: `לייקים בלבד`.
- **What was said is `[לא נשמע]` unless it is written on screen.**
- **A CAPTCHA, a login wall or a "suspicious activity" message:** stop that platform, tell the user which, and go on with the others. Never solve a CAPTCHA and never log in for them.
- **Only reading.** No like, follow, comment, save, share or message, ever.
- **Nothing leaves this folder.** The topic, the profile and the results are saved only in `scout/` here.

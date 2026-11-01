# How to update your site

Everything in the **Work** section (the projects, videos and songs) comes from one file:

**`data/projects.json`**

You don't need to touch any HTML to update projects. Think of that file as a setlist: it lists every project in order, and the site lays it out for you. The rest of the page (the top, About, Book and the footer) is written directly in `index.html`, which section 9 covers. The colors are in the stylesheet, which section 10 covers.

---

## 1. The quick version

1. On GitHub, open this project, then the `data` folder, then `projects.json`.
2. Click the **pencil icon** (top right of the file) to edit.
3. Make your change. Be careful with the commas and quote marks (see section 6).
4. Click **Commit changes**, then **Commit changes** again in the pop-up.
5. Wait one to three minutes, then refresh your site. Use **Cmd + Shift + R** (Mac) or **Ctrl + Shift + R** (Windows) so you see the newest version.

Your changes go live as soon as you commit. If something looks wrong, see section 8.

---

## 2. How the file is organized

```text
projects.json              the whole setlist
└─ a project               one band or project (Nehedar, Perl, ...)
   ├─ name, date           the title and years shown on the page
   └─ blocks               the pieces of that project, top to bottom
      ├─ text              a short blurb
      ├─ featured          the big highlighted album or song
      └─ videoGrid         the grid of thumbnails
         └─ items          each video or song in the grid
```

**The order in the file is the order on the page.** Move something up or down in the file and it moves on the site.

### Reading the punctuation

| You see | It means |
|---|---|
| `{ ... }` curly braces | One thing, like one project or one video |
| `[ ... ]` square brackets | A list of things |
| `"name": "Perl"` | A label, a colon, then its value. Both are in quote marks |
| `,` a comma | "Another one comes next." Goes after every item **except the last one in a list** |

---

## 3. One project, line by line

This is a reading guide only. The `<-` notes are not allowed in the real file.

```text
{                                      <- start of one project
  "name": "Perl",                      <- title shown on the label
  "date": "2020",                      <- years shown beside it (any text works)
  "blocks": [                          <- the pieces of this project, top to bottom
    {
      "kind": "text",                  <- this piece is a blurb
      "text": "Perl is a ..."          <- the blurb itself
    },
    {
      "kind": "videoGrid",             <- this piece is a grid of thumbnails
      "items": [                       <- the videos and songs, in order
        {
          "type": "soundcloud",        <- where it plays from
          "url": "https://soundcloud.com/perlmusic/sets/late-bloomer",
          "thumb": "img/perl-lateBloomer.png",   <- the picture on the tile
          "caption": "Perl - Late Bloomer"       <- the words under the tile
        }
      ]
    }
  ]
}
```

---

## 4. What every field does

### A project

| Field | What it is | Notes |
|---|---|---|
| `name` | Title on the label | Plain text |
| `date` | Years beside the title | Plain text, so `"2018-Present"` is fine |
| `blocks` | The pieces of the project | A list, shown top to bottom |

### A block (the `kind` says which piece it is)

| `kind` | Fields | What it shows |
|---|---|---|
| `text` | `text` | A short blurb about the project |
| `videoGrid` | `items` | A grid of video and song thumbnails. Click one to play it |
| `featured` | `embedSrc`, `linkUrl`, `linkText`, `title`, `description`, and optionally `width`, `height` | The big highlighted release with a player and a caption beside it. `title` and `description` show beside the player. `linkUrl` and `linkText` are a plain link that shows if the player can't load |

### An item inside a videoGrid (the `type` says where it plays from)

| `type` | Fields | Where to find the value |
|---|---|---|
| `youtube` | `id`, `caption` | `id` is the part of the video address after `v=`. In `youtube.com/watch?v=`**`k3J9xQ`**`&t=5s` it is `k3J9xQ`. Stop before any `&`. For short links like `youtu.be/k3J9xQ`, it's the end of the link |
| `soundcloud` | `url`, `thumb`, `caption` | `url` is the page address of the track or set, copied from the browser's address bar |
| `bandcamp` | `embedSrc`, `thumb`, `caption` | On Bandcamp, use Share / Embed and copy only the part inside `src="..."` in the embed code |
| `divider` | `label` | Not a video. A small heading between groups, like a year |

What the common fields do:

- **`caption`** is the words under the tile. It is also what screen readers announce on the play button, so write the real title.
- **`thumb`** is the picture on the tile. YouTube makes its own picture, so only SoundCloud and Bandcamp items need one.
- **`label`** (dividers only) is the heading text.

---

## 5. Common jobs

For every job, remember the comma rule in section 6.

### Change the order

Cut a whole `{ ... }` item and paste it where you want it. The order in the file is the order on the site. Copy the entire item, from its opening `{` to its closing `}`.

### Add a YouTube video

```json
{ "type": "youtube", "id": "VIDEO_ID", "caption": "Title of the video" }
```

### Add a SoundCloud song or set

```json
{ "type": "soundcloud", "url": "https://soundcloud.com/ARTIST/TRACK", "thumb": "img/cover.png", "caption": "Artist - Song Title" }
```

### Add a Bandcamp release

```json
{ "type": "bandcamp", "embedSrc": "PASTE_THE_EMBED_LINK", "thumb": "img/cover.png", "caption": "Album name" }
```

### Add a year heading inside a grid

```json
{ "type": "divider", "label": "2019" }
```

### Where a new item goes

1. Find the project and its `"items"` list.
2. Paste your new item **right after the item before it**.
3. Put a comma at the end of the item that comes before yours.
4. If yours is now the last in the list, it gets **no** comma.

### Change the big featured release

Find the project's block with `"kind": "featured"` and change its values:

```json
{
  "kind": "featured",
  "embedSrc": "PASTE_THE_EMBED_LINK",
  "linkUrl": "https://link-to-the-album-or-track",
  "linkText": "Song by Artist",
  "title": "Title shown above the description",
  "description": "A sentence or two about it."
}
```

Fill in all five of `embedSrc`, `linkUrl`, `linkText`, `title` and `description`. If one is missing, the page can show the word "undefined" in its place.

This spot was built for Bandcamp players, which are 350 pixels wide and 470 tall. If you ever need a different size, two optional extras are available: `"width"` and `"height"`, written as plain numbers (no quote marks around the number), in pixels.

Another kind of player, such as a YouTube embed, may load here, but it won't be the right shape unless you set the size. A wide one can also run off the edge of a phone screen. If you want to feature anything other than Bandcamp, ask Morgan to check it first.

### Change a project's blurb

Edit the words inside the quote marks after `"text":`. Keep the quote marks.

Blurbs, captions, titles and descriptions are plain text. A link, bold text or any other HTML typed into them will show up as typed characters, not as a link or bold. You can type `&` and apostrophes normally.

### Add a whole new project

Copy an entire project from its opening `{` to its closing `}`, paste it after the last project, put a comma after the project before it, then change the name, date and items. A starting point:

```json
{
  "name": "New project name",
  "date": "2026",
  "blocks": [
    { "kind": "text", "text": "A sentence or two about this project." },
    {
      "kind": "videoGrid",
      "items": [
        { "type": "youtube", "id": "VIDEO_ID", "caption": "Title of the video" }
      ]
    }
  ]
}
```

### Remove something

Delete the whole `{ ... }` item, then check the commas on either side. If you removed the last item in a list, the one before it now needs its comma taken off.

---

## 6. The mistakes that break the page

This file is picky. Any one of these can make the whole Work section fail to load.

| Mistake | What to do instead |
|---|---|
| A comma after the **last** item in a list | The last item never gets a comma |
| A missing comma between two items | Every item except the last needs one |
| **Curly quotes** “like this”, which happen when you paste from Google Docs, Notes or Word | Only straight quotes `"` work. Type them in the GitHub editor, or paste your words into a plain-text editor first |
| A quote mark **inside** your text, like `He said "hello"` | Put a backslash before it: `He said \"hello\"`. Or use an apostrophe instead |
| A line break in the middle of a caption or blurb | Keep each value on a single line |
| A missing closing `}` or `]` | Every `{` and `[` needs its partner |
| A project with no `"blocks"`, or a `videoGrid` with no `"items"` | Every project needs `blocks`, and every `videoGrid` needs `items` (even if there's just one). If one is missing, the **whole** Work section fails to load, not just that project |
| A quote mark missing around a label or value | Both the label and the text value need quote marks |

Before you commit, you can paste the whole file into **jsonlint.com**. It points to the exact line that is wrong.

---

## 7. Pictures

- Put images in the **`img`** folder. On GitHub, open `img`, then **Add file**, then **Upload files**.
- In `thumb`, write the path **without a slash at the start**: `img/cover.png`, not `/img/cover.png`. A leading slash can make the picture fail to load.
- Tiles are cropped to a wide rectangle, so keep the important part of the picture near the middle.
- Keep the file name simple. Letters, numbers and dashes are safest, and no spaces.

---

## 8. If something goes wrong

**What you'll see:** if the file has a mistake, the Work section shows a short error message instead of your projects. The rest of the site keeps working.

**To fix it:**

1. Paste the whole file into **jsonlint.com**. It points to the line with the problem. Fix it and commit again. This is usually a comma or a curly quote.
2. If you can't find it, go back to the last working version:
   - Open `data/projects.json` on GitHub and click **History**.
   - Click the last change from before things broke.
   - Open the file as it was at that point and copy its contents.
   - Go back to the current `projects.json`, click the pencil, select everything, paste, and commit.

   GitHub moves its buttons around now and then, so if you get stuck, message Morgan.

Nothing is ever lost. GitHub keeps every earlier version.

---

## 9. Everything else on the page

Only the Work section comes from `projects.json`. The rest is written directly in `index.html`:

- the top of the page (name, tagline, instruments, location, "available for")
- the Featured player and its button
- the About bio and press quotes
- the Book section (the inquiry options and the social links)
- the footer

To change the words, open `index.html` on GitHub, click the pencil, and use **Cmd / Ctrl + F** in the editor to find the text you want. Then:

- Change only the words between tags, or the web address inside a link's `href="..."`.
- Leave the tags and the `class="..."` names alone. The styling depends on them.
- Keep every opening tag with its closing tag. To add another quote or link, copy a whole existing one and change its words.
- Commit the same way as in section 1. Section 8 explains how to go back if something breaks.

---

## 10. Changing the colors

Every color on the site is set once, in a block called `:root` at the very top of the stylesheet (the `.css` file). Change a color there and it changes everywhere.

### To change one

1. On GitHub, open the stylesheet and click the **pencil icon**.
2. Press **Cmd / Ctrl + F** and search for `:root`.
3. Change the six characters after the `#` on the line you want, for example `--gold: #d99a2b;`. Keep the semicolon.
4. Commit the same way as in section 1, then hard refresh.

### What each one is

| Name | What it's for |
|---|---|
| `--parchment` | The paper behind the page |
| `--cream` | Light panels, and the text on dark or colored areas |
| `--ink` | Main text |
| `--ink-light` | Secondary text |
| `--muted` | Small, quiet text |
| `--rule` | Thin lines |
| `--seafoam`, `--gold`, `--avocado`, `--coral` | The colored tabs, buttons and accents |
| `--gold-text`, `--seafoam-text` | Gold and seafoam used as **text**. They are darker versions so they stay readable on the paper |
| `--footer-text` | Text in the dark footer |

### Try a color before you commit

You can test a color in your browser without changing anything:

1. Open your site, right-click the page and choose **Inspect**.
2. Click the `<html>` line at the top of the code.
3. In the **Styles** panel, find `:root` and click any color to change it.

The change disappears when you refresh, so nothing is saved until you edit the file on GitHub.

### Keep the words readable

- The words on each colored tab are set separately from the tab color. If you make a tab much **lighter**, its words get hard to read. A darker color is safer.
- Change one color at a time and look at the whole page, including the tabs, the Book form's selected option, and the footer.
- To check a pair of colors, use **webaim.org/resources/contrastchecker**. Aim for a ratio of **4.5** or higher for normal text, or **3** for big bold text.

### Going back to the old colors

The earlier colors are still in the file as comments, on the line above each new one, between `/*` and `*/`. To use an old one, delete the new line and remove the `/*` and `*/` around the old one. The `--gold-text`, `--seafoam-text` and `--footer-text` lines are new and have no old version, so leave them in.

---

## 11. Getting help from an AI assistant

If you ask an AI tool to help, tell it:

> Read the README in this project first. For projects, only edit `data/projects.json`, keeping every value in straight quote marks and checking the commas. For other text on the page, only change words in `index.html`, not tags or class names. Don't change the JavaScript files. Only change the CSS if I ask for new colors, and then only the color values in the `:root` block.

Then paste any new `projects.json` into jsonlint.com before you commit.

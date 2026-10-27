# PacketCapture CTF: OSINT Writeups

Writeups for the OSINT category.

| # | Challenge | Difficulty | Flag |
|---|-----------|------------|------|
| 1 | [Treasure Hunt Time](#1-treasure-hunt-time) | Easy | `PacketCapture{85.36,81.05}` |
| 2 | [Mirror Identities](#2-mirror-identities) | Medium | `PacketCapture{p3rf3ctly_P13c3D}` |
| 3 | [Digital Footprint](#3-digital-footprint) | Medium | `PacketCapture{3Ven_1f_i_D1s4ppe4r_tH3_W0rld_r3Memb3rs}` |
| 4 | [The Final Breadcrumb](#4-the-final-breadcrumb) | Hard | `PacketCapture{magentatexturemofsj7hk44wtsh762nv46lt3bnprcmttbaqxknux66rnaddmyd}` |

> **Screenshots:** image placeholders (`images/<challenge>/<n>.png`) mark where each screenshot belongs. Drop your images into those paths before publishing.

---

## 1. Treasure Hunt Time

**Category:** OSINT | **Difficulty:** Easy

### Description

> My grandfather left me this image as part of his will. He said I can find a treasure using it. I don't know, maybe he's just playing with me.

**Flag format:** `PacketCapture{00.00, 00.00}`

**Coordinate format:** Decimal degrees (DD). Degrees-minutes-seconds (DMS) is shown below for reference.

| Format | Example |
|--------|---------|
| DD | `85.36, 81.05` |
| DMS | `85°21'36" N, 81°03'00" E` |

### Solution

1. The image looks like a generic stock photo and doesn't point to any real location.

   ![Challenge image](images/treasure-hunt-time/1.png)

2. The EXIF data contains only a misleading comment, so the answer has to be found elsewhere.

   ![EXIF data](images/treasure-hunt-time/2.png)

3. Both the challenge title and the filename contain the phrase **treasurehunttrip**, which is odd. Searching for it directly leads nowhere.

4. The description says everything needed is already in the image, so I searched for unconventional ways of tagging coordinates.

5. This turned up a geocaching-style pattern that matches what we have: three words used as a location tag.

   ![Geocaching pattern](images/treasure-hunt-time/3.png)

6. Checking the three words on the **what3words** website returned a valid hit.

   ![what3words hit](images/treasure-hunt-time/4.png)

7. The coordinate display on what3words is paywalled, so I used an online geocache converter to get the coordinates.

   ![Converter output](images/treasure-hunt-time/5.png)

### Flag

```
PacketCapture{85.36,81.05}
```

### Tools Used

- what3words
- Geocaching toolbox

---

## 2. Mirror Identities

**Category:** OSINT | **Difficulty:** Medium

### Description

> Learn how to think outside of the cell. Think outside of the hole he left behind. See the identities linked to it without being named. Your only guidance is the excel they left.

### Provided Files / Artifacts

- Google Sheets link: <https://docs.google.com/spreadsheets/d/1ZXB6pnQEvym9pkDDoSxl49mxiiMAkzx0tS50OuwRt3Q/edit?usp=sharing>

### Solution

1. We are given a link to a public spreadsheet. Its contents are all noise, but the owner's Gmail can be extracted from any public Google document through its metadata. I used **xeuledoc** for this.

   ![xeuledoc output](images/mirror-identities/1.png)

2. To enumerate the services registered with that Gmail address, I used **holehe**. It returned one hit: **Twitter/X**.

   ![holehe output](images/mirror-identities/2.png)

3. Using the name from xeuledoc, I searched X to see whether the target uses the same name. This gave two accounts: the first an obvious lead, the second a likely dud.

   ![Search results on X](images/mirror-identities/3.png)

4. The first account leaks several pieces of information, mainly a GitHub account. Two tweets stand out:
   - one hints at a **second Twitter account**
   - the other hints at an **Instagram account**

   ![Interesting tweets](images/mirror-identities/4.png)

5. The GitHub account has a single repository with one commit. It contains what looks like a set of instructions. We don't yet know what they're for, so I pivoted to Instagram.

   ![GitHub repository](images/mirror-identities/5.png)

6. Searching for the target's X handle on Instagram finds their account, which has one post. This is where the GitHub instructions come in.

   ![Instagram post](images/mirror-identities/6.png)

7. Applying the instructions to the two numbers from the post and extracting characters 2-7 gives two names: **Namoor** (blue) and **Krommer** (red).

   ![Extraction result](images/mirror-identities/7.png)

8. Back on X, the target mentions another user whose handle is their pet's name. Their bio contains a quoted phrase.

   ![Pet account bio](images/mirror-identities/8.png)

9. The account `Cloveeen` earlier instructed us to make contact through email. The quoted phrase can likely be used to trigger an automated reply.

10. Sending the quoted phrase to that email address returns the flag.

    ![Automated reply](images/mirror-identities/9.png)

### Flag

```
PacketCapture{p3rf3ctly_P13c3D}
```

### Tools Used

- xeuledoc
- holehe

---

## 3. Digital Footprint

**Category:** OSINT | **Difficulty:** Medium

### Description

> A sub that I was in got banned as of recently. I did hear that they migrated, but rumors started to circulate in our chat server about a member's existence being scrubbed off except on the words of others.

**Sub URL:** <https://www.reddit.com/r/CogitantStray/>

### Solution

1. We are given a banned subreddit.

   ![Banned subreddit](images/digital-footprint/1.png)

2. Before scraping the archives, I searched for the migration subreddit mentioned in the description so I could cross-reference users across both communities.

3. Searching the same subreddit name finds a small, active subreddit.

   ![Migration subreddit](images/digital-footprint/2.png)

4. Cross-referencing every name that appears in both subreddits through **Arctic Shift** produces a long list of candidates, which would be tedious to check one by one. I set the list aside and looked for other clues.

5. The current subreddit's rules link to a **Pastebin**, set up as a temporary wiki for the game.

   ![Subreddit rules](images/digital-footprint/3.png)

6. The Pastebin lists the characters currently available in multiplayer mode. Any name on it can be crossed off our candidate list.

   ![Pastebin character list](images/digital-footprint/4.png)

7. Searching further turns up several red herrings, including a curious quote: *"A Kacelia flower once bloomed, no more shall her dreams linger in my sleep."*

8. In a recent post, a comment sits beneath a deleted one. It mentions **Rosint**, which is not a playable character from the wiki. It is an OSINT tool that pulls from Arctic Shift to make scraping a target user's history easier.

   ![Comment mentioning Rosint](images/digital-footprint/5.png)

9. Running Rosint on `u/OguriClap` reveals the parent comment, which leads toward the flag.

   ![Rosint output for u/OguriClap](images/digital-footprint/6.png)

10. Repeating the same process on `u/Axusismynameom` shows the flag in their deleted post.

    ![Deleted post with flag](images/digital-footprint/7.png)

### Flag

```
PacketCapture{3Ven_1f_i_D1s4ppe4r_tH3_W0rld_r3Memb3rs}
```

### Tools Used

- Reddit search
- Arctic Shift
- Rosint

---

## 4. The Final Breadcrumb

**Category:** OSINT | **Difficulty:** Hard

### Description

> We found a zip left behind by a hacker that keeps on breaching our backdoor. He seems to be deliberately leaving crumbs to him. Track it until you find what was left behind.

### Provided Files / Artifacts

A zip file containing:

- source code
- a PGP fingerprint
- multiple magenta images

### Solution

#### Phase 1: Initial triage

1. The source code contains a URL in its comments. Searching it leads to a dud, so we move on to the other points of interest.

   ![URL in source comments](images/final-breadcrumb/1.png)

2. I checked each image for leads, `flag.png` in particular because of its name. Only standard metadata is present, and the same holds for the other two images in the `nature` folder.

   ![Image metadata](images/final-breadcrumb/2.png)

#### Phase 2: The PGP fingerprint

3. Searching the fingerprint on PGP key servers returns an email format attached to the key. The username is generic, which makes it hard to find the right account.

   ![Key server result](images/final-breadcrumb/3.png)

4. Searching the fingerprint on social platforms is another dud. Searching the key on Google finally produces a hit.

   ![Google hit](images/final-breadcrumb/4.png)

5. The hit is a YouTube video showing a scene from *Final Destination*, with text in the video description. Submitting `PacketCapture{magentatexture}` did not work, so something is still missing.

   ![YouTube video](images/final-breadcrumb/5.png)

#### Phase 3: Mastodon

6. The channel description contains something interesting. A PGP fingerprint is normally 40 characters, so **removing the last 10 characters** of ours should give a usable search term. Doing so finds an account on **Mastodon**.

   ![Channel description](images/final-breadcrumb/6.png)
   ![Mastodon hit](images/final-breadcrumb/7.png)

7. The Mastodon account links to a Pastebin full of Mastodon emails. Searching any of them leads to a dud, because the accounts don't exist.

8. Everything so far has been black and magenta, yet **magenta is missing from the list**. Going back to where we found the first Mastodon account and searching for `magenta` gives a result.

   ![Pastebin list](images/final-breadcrumb/8.png)
   ![Magenta account](images/final-breadcrumb/9.png)

#### Phase 4: Following the paths

9. This account is relatively unique, so it is easier to track. Two paths look plausible:
   - the **support link**
   - the **YouTube URL on the nickname** (not the link)

10. The YouTube URL leads to a video whose title is a clear jab at the challenge title. It also contains **obfuscated text**, which I noted for later.

    ![Second YouTube video](images/final-breadcrumb/10.png)

11. The support link leads to a MediaFire download of the account's profile image. Running `exiftool` on it reveals a **comment containing a question**.

    ![MediaFire link](images/final-breadcrumb/11.png)
    ![exiftool comment](images/final-breadcrumb/12.png)

12. The filename (`fromdinoera`) is odd, which suggests **steganography**. It is a bit of a stretch, but worth checking. The question's answer is that the texture is simply missing: magenta and black is the standard placeholder for a missing texture, which is what ties the whole challenge together. Using that answer with **Steghide** reveals another MediaFire link.

    ![Steghide extraction](images/final-breadcrumb/13.png)

13. This link gives another flag, but it was rejected. It is a red herring, though it does give us one hint: **onion**.

    ![Red herring flag](images/final-breadcrumb/14.png)

#### Phase 5: Putting it together

14. Back to the YouTube videos. The video's description and its obfuscated text show that the text is missing something. Onion links usually start with readable words, and the description's "there's no house" hint points to the same thing. We already have the missing part.

    ![Obfuscated text and description](images/final-breadcrumb/15.png)

15. The video also tells us this is the "final destination", and the channel name is the flag format itself. Combining the channel name with the obfuscated text gives the flag.

### Flag

```
PacketCapture{magentatexturemofsj7hk44wtsh762nv46lt3bnprcmttbaqxknux66rnaddmyd}
```

### Tools Used

- PGP key servers
- Google search
- exiftool
- Steghide

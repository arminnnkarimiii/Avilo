# Avilo – Product Plan v1

**(road to 100 million evaluation)**

AI-native learning app for kids aged 3–12. Parents describe their child, and Avilo builds a personal learning path of games, subjects, skills and stories. The feel is "Duolingo for early development": daily habits, visible progress, and the parent's confidence that their child is growing.

- **Mascot:** Avi, a cozy pixel-art teddy bear with a red bow. Logo = `Design/Avi/avi-logo-head.webp`.
- **Platform:** native iOS (SwiftUI). English only at launch.
- **Contact:** avilo.education@gmail.com · Instagram @avilo.app · X @AviloApp

---

## 1. Decisions locked

| Topic | Decision |
|---|---|
| Name | Avilo |
| Age structure | 3 bands: **Little Cubs 3–5**, **Explorers 6–8**, **Trailblazers 9–12** |
| Subjects (v1) | Math & numbers · Reading & phonics · Science & nature · Feelings & life skills |
| AI approach | **Hybrid:** fixed game types and curriculum; AI personalizes themes, difficulty and word problems, and writes custom stories starring the child |
| Visual style | Soft, rounded modern iOS UI with pixel-art Avi and a few pixel accents (stars, badges) |
| Voice | AI voice narration for Avi and stories (tap any text to hear it) |
| Motivation | Daily streaks · Stars & level-ups on a path map · Badges · Dress up Avi / decorate his room |
| Parent tools | Skill progress report (AI insights) · Weekly summary · Edit learning profile |
| Sign up | Sign in with Apple only |
| Kids per plan | 1 child per subscription |
| Paywall | Hard paywall: **Education+ $7.99/mo** or **$59.99/yr (save 37%)**, both with a **7-day free trial** |
| Build order | Design system → Design canvas of all screens → review → full SwiftUI app |
| Building | Code is written on Windows. Builds and tests run on a Mac with Xcode (sister's Mac) |

## 2. The funnel (parent first, kid after paying)

1. **Welcome:** Avi waves (`avi-wave-excited`). "Hi! I'm Avi. Let's build a learning adventure made just for your child."
2. **Child quiz** (~8–10 tap-friendly questions, Avi `avi-thinking` / `avi-idea` beside each):
   - Child's first name or nickname, age, and avatar color
   - Interests (dinosaurs, space, animals, princesses, cars, music, art, sports…)
   - Personality (curious, energetic, shy, creative, chatty, careful…)
   - How they learn best (stories, puzzles, moving around, drawing)
   - Current level checks per subject (e.g. "Can they count to 10?", "Do they know letters?")
   - Parent goals (school readiness, reading confidence, focus, emotional skills)
   - Daily time goal (5 / 10 / 15 / 20 min)
3. **"Avi is making the profile…"** loader (`avi-reading-sit`) while the AI builds it.
4. **Learning profile reveal:** "Mila is a **Curious Builder** 🧱", with `avi-explorer`. It shows strengths, a sample of the first week's path, and a blurred preview of a personalized story ("Mila and the Dinosaur Who Lost His Numbers").
5. **Paywall:** unlock Mila's plan. Monthly or yearly, 7-day free trial, `avi-thumbs-up`. Restore purchases, terms and privacy links (behind a parent gate).
6. **Sign in with Apple** to save the profile.
7. **Trial started** celebration: `avi-wink-thumbs`, confetti in pixel stars.
8. **Kid dashboard unlocked.**

> The quiz → profile → paywall order is intentional. Parents invest effort and see real value (their child's profile) before the price, which converts much better than an upfront paywall.

## 3. Kid experience

- **Home / path map:** Avi waving at the top, the daily streak flame, stars, and today's 3 activities on a winding path. Big buttons, voice on every element, almost no reading needed for Little Cubs.
- **Subjects:** 4 colored "worlds" (Math meadow, Reading forest, Science sky, Feelings cottage).
- **Games** (fixed types, AI-themed content): count & tap, match pairs, sort & drag, letter tracing, sound-it-out, fill the word, quick quiz, pattern puzzles, emotion faces, story choices.
- **Stories:** a nightly AI story starring the child and their interests, narrated by Avi (`avi-reading-stack`). Picture pages use Avi art and simple illustrations.
- **Rewards:** stars → Avi's wardrobe and cozy room (`avi-tea-time` room scene). Badges shelf.
- **Breaks and limits:** after the daily goal, Avi sips cocoa (`avi-sip-cozy`): "Great job today! Time to rest."
- **Parent gate:** a hold-and-answer adult challenge before settings, links or anything parent-facing (Apple Kids Category rule).

## 4. Parent dashboard (behind the parent gate)

- Weekly overview: minutes, streak, activities, stars
- Skill progress per subject (mastered / learning / next), with an AI-written insight: "Mila is strong in counting; let's practise rhyming sounds"
- Weekly summary push notification and email
- Edit learning profile (interests, goals, daily time) → the AI adapts the path
- Subscription management, privacy and support (avilo.education@gmail.com)

## 5. Brand and design direction

Palette (derived from Avi):
- **Honey** `#D9894A` (primary), **Caramel** `#B8692F`, **Cream** `#FFF4E2` (background), **Cocoa** `#3B2418` (text)
- **Bow Red** `#D63B2F` (call to action, accents), **Sunny** `#F5B53D` (stars, rewards)
- Subject colors: Math **Leaf** `#6BBF59`, Reading **Sky** `#4FA3E0`, Science **Lilac** `#9B7BE0`, Feelings **Blush** `#F28BA8`

Type: SF Pro Rounded (friendly, Dynamic Type), plus a pixel display font only for stars, levels and badges.
Shape: large radii (20–28), soft shadows, 56pt+ tap targets for kids, and generous spacing.
Avi placement: every screen uses the pose that fits it (see `Design/Avi/`). Never cropped awkwardly, always on a clean background, and kept large enough to read the pixels (keep integer scaling where possible).

## 6. Technical architecture

- **iOS app:** SwiftUI, iOS 17+, MVVM, async/await, SwiftData cache for offline play of downloaded content
- **Backend:** Supabase (Postgres, Auth with Sign in with Apple, Storage, Edge Functions)
- **AI:** Claude API called **only from Supabase Edge Functions**, never from the app (keys stay server-side)
  - Learning profile + path generation
  - Story generation (child name + interests + age band + target skill)
  - Themed question and word-problem generation within fixed templates
  - Every generated text passes a kid-safety check (age-appropriate prompt rules + a second moderation pass) and is cached, so each piece is generated once
- **Voice:** a text-to-speech provider called from Edge Functions, with audio cached in Storage
- **Payments:** StoreKit 2 (two auto-renewable products, 7-day intro offer). Entitlement checked server-side through App Store Server Notifications
- **Analytics:** first-party only (Kids Category bans third-party analytics and ads)

## 7. Privacy and safety (must-haves)

- COPPA / GDPR-K: only the parent creates the account. Store only the child's first name or nickname and age band, never photos, location or contacts.
- Child data sent to AI is minimized (nickname, age band, interests) and disclosed in the privacy policy. No training on the data.
- No chat between kids, no free-text input from kids to the AI, no ads, no external links without the parent gate.
- Apple Kids Category guidelines (parental gate, no third-party analytics).

## 8. Milestones

1. **M0 Plan** ✅ (this doc)
2. **M1 Design** (v1 ready for review): [Avilo Design System](https://claude.ai/artifact/N7TC8gcLqnGgG4VnR2LDom) and the [Avilo App Screens canvas](https://claude.ai/artifact/BZJyrBo6UDQA6NaEGRfWNv) with 12 iPhone screens: welcome, 2 quiz steps, profile building, profile reveal, paywall, sign in, trial started, kid home, counting game, bedtime story and parent dashboard. Rewards (Avi's room) are still to design. The links are private until shared from each page's Share menu.
3. **M2 App shell:** SwiftUI project, design tokens, navigation, onboarding quiz, paywall (StoreKit sandbox)
4. **M3 Backend + AI:** Supabase, Apple auth, profile/story/question Edge Functions, TTS
5. **M4 Content:** games for all 4 subjects × 3 age bands, stories, rewards
6. **M5 Parent dashboard + weekly summary**
7. **M6 TestFlight beta** on the Mac → App Store submission

## 9. Open items (need later)

- Legal: privacy policy, terms (required for App Store and the paywall)
- Apple Developer account ($99/yr) and App Store Connect products
- More Avi poses later: celebrating, sleeping, sad/"try again", and for each subject world

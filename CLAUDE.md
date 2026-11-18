# Avilo – guidance for Claude Code

Educational SaaS iOS app for children. README must keep the line "(road to 100 million evaluation)".

## Stack
- Swift + SwiftUI, iOS 17+, MVVM, Swift Concurrency (async/await)
- Supabase backend; StoreKit 2 for subscriptions
- Swift Package Manager only (no CocoaPods)

## Conventions
- One feature per folder under `Avilo/Features/<Name>/` (View, ViewModel, Model).
- Never commit secrets; keep keys in `Config.xcconfig` (gitignored).
- Child safety: no ads, no third-party trackers, no free-text chat between kids, parental gate before purchases/external links.
- Accessibility: Dynamic Type, VoiceOver labels on all controls.

## Workflow
- Never commit directly to `main`; branch per task (`name/short-description`), open a PR.
- `git pull` before starting. Keep PRs small.

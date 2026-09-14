# 05a — Feed.fm (Feed Media Group) market check

Date: 2026-09-14. Method: WebSearch was unavailable (API 400 on every call); all findings via WebFetch of primary pages. Blocked sources: Crunchbase (403), G2 (403), Reddit (fetch blocked), Wayback (fetch blocked), PRWeb search (0 results). HN searched via Algolia API. GitHub org and issue pages fetched.

Labels: VERIFIED = quoted from fetched page with URL. INFERRED = my reading of verified facts. NO EVIDENCE FOUND = looked, found nothing.

## Summary table

| Question | Verdict | Confidence |
|---|---|---|
| 1 Pricing | No public prices. Quote-only, sales-led. Billing = song plays + number/type of stations; quarterly fees in advance, 1-yr auto-renew, optional setup "Standard Fee". No free/dev tier (only demo sandbox creds). | VERIFIED on model; NO EVIDENCE on numbers |
| 2 Catalogue | Major-label tier real: WMG deal (Nov 2025, Clips), Hopeless Records station; sample stations name Taylor Swift, Dua Lipa, Cardi B, Foo Fighters etc. Major-label Radio stations "available in the US, Canada, and Australia". No track count. Customer apps: Barry's X, Hydrow, Echelon, NKO Club, Sworkit, Ergatta. | VERIFIED |
| 3 Customers | Fitness/wellness dominated; also American Eagle, Golden State Warriors. All named customers are funded companies/brands. Indie/solo dev: NO EVIDENCE FOUND. Adaptr (sister brand) pitched at "startups"/"growth-stage businesses in the US". | VERIFIED / NO EVIDENCE on indie |
| 4 Reviews | Essentially none found. G2/Reddit blocked; HN only 2 Feed.fm-employee comments; GitHub issue creation restricted, 0 open issues on JS and iOS SDK. | NO EVIDENCE FOUND |
| 5 Terms | T&Cs are B2B/SOW-based. No explicit ban on consumer subscription apps; case studies are all consumer-facing apps. No clause on podcast interleaving or DJ voice; nearest: content "without alteration", no inserting content into API Content, no artist-focused sites, no advance playlists. | VERIFIED text; INFERRED on applicability |
| 6 Health | Active Sep 2026: news posts dated Sep 2026, GitHub SDK updates Jun–Aug 2026, "300% growth in new customer ARR" claim Jul 2026, LinkedIn 11-50 (48) employees. Funding/layoffs/acquisition: NO EVIDENCE FOUND. | VERIFIED active |

## 1. Pricing

- VERIFIED — no published prices anywhere. Pricing page: "Pricing that's in tune with your business"; "Choose a product below to get started. Our team will create a customized proposal for your business needs." Each product has "Request pricing". FAQ: "Feed.fm plans are based on the number of song plays and the number and type of curated soundtracks included in the package." https://www.feed.fm/pricing-page
- VERIFIED — feed.fm/pricing returns 404; canonical is /pricing-page.
- VERIFIED — T&Cs §4: "Customer shall pay to Feed.fm the Standard Fee, if any, stated on the SOW within 10 days of the Effective Date"; Quarterly Fees by Subscription Tier that "correspond to the amount of API Content to be streamed on a Site in any given month"; "payable in advance at the beginning of each quarter"; "nonrefundable"; late interest 1.5%/month. Term one year, "automatically renew for additional one-year terms" unless 30 days' notice. https://www.feed.fm/terms-and-conditions
- VERIFIED — Adaptr (sister brand): "One monthly price gets you unlimited access to the largest pre-cleared B2B music catalog" — but no figure; "GET A QUOTE" only. https://www.adaptr.com/pricing
- VERIFIED — Dev access is sales-gated: "Talk to a music specialist to get your token today". Public sandbox creds exist for the open-source Android SDK: "demo demo" (three stations, no skip limits), "counting counting", "badgeo badgeo" (simulates unlicensed region). https://www.feed.fm/feed.fm-android-sdk
- NO EVIDENCE FOUND — per-stream rate, per-MAU rate, monthly minimum, setup fee amount, contract minimum size. Wayback pricing snapshots not fetchable.
- INFERRED — sales-only, annual B2B contract, quarterly prepay; not self-serve. The DIY comparison on the Clips page ("12-24 mo" and "+ $150-250K tech spend") is what they price against, suggesting deals sized for funded companies.

## 2. Catalogue

- VERIFIED — three tiers: "Major label", "Indie label", "Royalty free". https://feed.fm/
- VERIFIED — Major-label page sample stations name artists: Pop "Taylor Swift, Dua Lipa, Olivia Rodrigo"; Hip-hop "Ty Dolla $ign, Megan Thee Stallion, Cardi B"; Latin "Karol G, Daddy Yankee, Maluma"; Country "Dasha, Luke Combs, Kacey Musgraves"; Rock "Foo Fighters, The Black Keys, Weezer"; Dance "Tiesto, David Guetta, and Calvin Harris"; R&B "Teddy Swims, Usher, SZA". Territories: "Sample Feed Radio stations shown below are available in the US, Canada, and Australia"; "Looking for global rights? Check out our indie catalog and wholly-owned collections". https://www.feed.fm/major-label-music
- VERIFIED — labels: "Warner Music Group and Feed.fm Partner to Bring Premium Music Clips to Apps" (Nov 17, 2025); "WMG teams up with Feed Media for Adaptr startup licensing platform" (Jan 25, 2021); Hopeless Records pop-punk station "featuring All Time Low, Avenged Sevenfold, PVRIS" (Sep 1, 2026), live in NKO Club and Echelon. https://blog.feed.fm/news ; https://www.linkedin.com/company/feed-fm
- VERIFIED — WMG alt text on Clips page: "Hunxho_Kivenchy_provided by WMG for Feed Clips". https://www.feed.fm/clips/how-to-use-real-major-label-music-in-your-app
- NO EVIDENCE FOUND — UMG or Sony named anywhere on site. Track count not stated ("You don't need a Spotify-sized catalog").
- VERIFIED — apps playing mainstream music via Feed.fm: "Barry's Launches Streaming Music Service Through Feed.fm" (Nov 8, 2023); "helping Barry's X and Hydrow users work out for longer" (Mar 21, 2024). https://blog.feed.fm/news
- INFERRED — major-label rights are effectively North America + Australia for radio; global apps must drop to indie/royalty-free tier.

## 3. Customers

- VERIFIED — customers page: American Eagle Outfitters, Golden State Warriors (with JBL/Harman), Sworkit, Conqur Endurance Group (LA Marathon), Future, Ergatta, Asensei, CITYROW, NKO Club, Aaptiv, Aescape, Alphabeats, Akina, Tempo; logos Alo Moves, Barry's, Johnson Health Tech, Tonal, Fight Camp, Life Fitness, Vaha, Technogym. https://www.feed.fm/customers
- VERIFIED — news: Hydrow, Echelon, PEAR Health Labs, SweatWorks, Materialize Labs, VR baseball game, "The Sound System" (Sean Olivera / Reach Records, Mar 2026). https://blog.feed.fm/news
- VERIFIED — footer verticals: Family-friendly, Faith-based, Social & UGC, Language learning, E-cards, Digital fitness, Health & wellness. https://feed.fm/
- VERIFIED — Adaptr targets "startup" and "growth-stage businesses in the US"; testimonial from "Ashish Rawat, Founder, Oga": "We got signed up [with Adaptr] in days, lightning fast!" https://www.adaptr.com/ ; https://www.adaptr.com/pricing
- NO EVIDENCE FOUND — any solo/indie developer or bootstrapped app as a customer. Akina/Alphabeats/Asensei may be small but are not described as indie.

## 4. Reviews and complaints

- VERIFIED — HN Algolia: 2 hits total, both Feed.fm staff (2017 hiring comment: serves "already-profitable businesses rather than consumers"; Dec 2021 Who is hiring). No user reviews. https://hn.algolia.com/api/v1/search?query=feed.fm
- VERIFIED — GitHub feedfm org: 22 repos; Javascript-SDK 8 stars, iOS-SDK 3, Android-SDK 5; "Issue creation is restricted in this repository"; 0 open issues on JS and iOS SDK issue pages. Android-SDK updated Jun 2026, iOS-SDK-Demo Aug 2026. https://github.com/feedfm ; https://github.com/feedfm/Javascript-SDK/issues ; https://github.com/feedfm/iOS-SDK/issues
- NO EVIDENCE FOUND — G2 (403), Capterra/Product Hunt not reached, Reddit blocked, no indie-hacker forum threads surfaced. No third-party developer sentiment located at all.
- INFERRED — low public footprint + restricted issues = feedback happens inside contracts, not publicly. Absence of complaints is not evidence of quality.

## 5. Terms

- VERIFIED — T&Cs are a B2B SOW agreement; no clause distinguishing consumer vs business end users. §5.7(s) bars public performance "in any establishment, venue or location open to the public"; §5.8(c) puts brick-and-mortar "Group Fitness" licensing on the customer. https://www.feed.fm/terms-and-conditions
- VERIFIED — self-description: "streaming of licensed music for business"; but all case studies are consumer apps (Sworkit, Future, Ergatta) and site says users "won't have to subscribe, pay, or be exposed to any other advertising" for the music itself. https://feed.fm/ ; https://www.feed.fm/music-licensing
- INFERRED — consumer subscription apps are the core customer profile (Barry's X, Hydrow, Echelon are all consumer subscriptions). Accepted.
- VERIFIED — relevant restriction clauses: content delivered "without alteration in any way whatsoever without Feed.fm's prior written consent"; "Customer will also not insert into any API Content delivered via a Player any content (e.g., audio advertisements)"; no "remix, alter, repurpose"; (n) no "synchronizing API Content in timed relation with external media"; (r) no "advance playlist"; (x) Site must not "focus on a specific musical artist or specific set of musical artists". https://www.feed.fm/terms-and-conditions
- NO EVIDENCE FOUND — any explicit clause on interleaving with podcasts/spoken audio, or on a DJ voice announcing tracks.
- INFERRED — playing spoken segments between tracks (not over them) is likely permissible since API Content itself is unaltered; talking over intros or ducking music under voice would plausibly hit "alteration"/"insert into API Content" and need written consent. Announcing artist/title is normal radio practice and not barred, but must be confirmed in SOW.

## 6. Company health

- VERIFIED — LinkedIn: "Company size 11-50 employees" / "48 employees"; HQ "845 Market St San Francisco"; "Privately Held"; page says "Founded 2021" (site says "Since 2012"). https://www.linkedin.com/company/feed-fm ; https://www.feed.fm/feedmediagroup
- VERIFIED — activity Sep 2026: news items dated Sep 1 and Sep 8, 2026; "Feed.fm Reports 300% Growth in New Customer ARR as Music Becomes a Core Feature for AI-Era Apps" (Jul 30, 2026); "2024 & 2026 Fast Company's Most Innovative Music Company"; "Inc Power Partner" 2024; DiMA member (May 2023). https://blog.feed.fm/news ; https://feed.fm/
- VERIFIED — historical: "Annual Revenue Growth of Nearly 200%" (Aug 2021); "Triples Music Streams in First Half of 2022". https://blog.feed.fm/news
- VERIFIED — open roles page links to a Rippling ATS; team is "remote-first". https://www.feed.fm/feedmediagroup
- NO EVIDENCE FOUND — funding rounds, total raised, investors (Crunchbase 403), layoffs, acquisitions. Adaptr's status unclear: site live but with placeholder footer text ("Menu Item One"); not mentioned on Feed Media Group about page.
- INFERRED — small (~50), self-reported growing, still shipping SDKs and label deals in 2026. Site has visible unfinished template copy on several pages, suggesting a lean marketing team.

## Implications for a personalised-radio app

- Sales-gated, annual, quarterly-prepaid B2B contract; no self-serve or dev tier. Fit for funded teams, not a solo validation experiment. INFERRED.
- Major-label radio limited to US/CA/AU. INFERRED from VERIFIED sample-station text.
- Spoken interludes between tracks not explicitly barred; voice-over-music and any editing of tracks are. Needs SOW confirmation before build.

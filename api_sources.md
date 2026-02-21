VIBE CODING PROMPT — IVF / Infertility / Surrogacy News Page (Mobile-first)

Goal:
Build a fast, mobile-first “News” page that aggregates IVF/infertility/surrogacy news from multiple sources, normalizes it into one common data structure, and displays it in a modern UI with smooth GSAP animations. Use Unsplash images as placeholders when a news item has no image.

APIs to use:

A) Image Placeholder API (Unsplash)

- Purpose: Preload a set of images to use as placeholders for news items that have missing/empty images.
- Endpoint:
  https://api.unsplash.com/search/photos?page=1&query=IVF,infertility,surrogacy&per_page=20&client_id=WztAjjff7Z9mPXfGCNwmu8qPlVOIjuZaDErzoSy-5Tw

B) News APIs (Fetch all asynchronously / in parallel)

1. Bing News (via my Cloud Function)

- Endpoint:
  https://us-central1-nandiraju-api.cloudfunctions.net/app/news?source=bing&q=IVF+infertility+surrogacy

2. Google News (via my Cloud Function)

- Endpoint (note: “goolge” is spelled as in the URL):
  https://us-central1-nandiraju-api.cloudfunctions.net/app/news?source=goolge&q=IVF+infertility+surrogacy

3. NewsDataHub (direct API)

- Use this sample cURL as reference:
  curl -X GET "https://api.newsdatahub.com/v1/news?q=IVF%2Cinfertlity%2Csurrogacy&deduplicate=true&per_page=20" \
   -H "X-API-Key: ndh_h2jQvFz3dQp1TDOmMGX-dogdS4prXtJycnCbU3-gjMI" \
   -H "User-Agent: NewsDataHub-API-Tester/1.0"

Implementation Requirements:

1. Fetch Strategy (Async + Performance)

- Fetch Unsplash placeholder images first (or at least ensure they are available before rendering cards with missing images).
- Fetch ALL 3 news sources concurrently (Promise.all).
- Deduplicate articles across sources (use a stable key: URL preferred; fallback: title+publishedAt).
- Normalize all news data into ONE common array of objects.

2. Common Data Structure (Normalized Article)
   Create a single structure for all news items, for example:

- id (string)
- title (string)
- summary/description (string)
- url (string)
- source (string: "bing" | "google" | "newsdatahub")
- publisher (string)
- publishedAt (ISO string or timestamp)
- imageUrl (string) // if missing, assign a placeholder from Unsplash
- tags/topics (array optional)

3. Placeholder Image Rules

- If imageUrl is missing/empty/null, assign one of the preloaded Unsplash images.
- Cycle through placeholders so repeated “no image” items still look varied.
- Prefer using small/optimized image sizes for performance.

4. UI Requirements

- Build a nice-looking News page UI:
  - Tailwind CSS via CDN
  - Responsive, mobile-first layout
  - Easy to navigate
  - Good performance (avoid heavy DOM work, lazy-load images, etc.)

5. Animations

- Use GSAP via hosted CDN.
- Smooth animations:
  - Page load: stagger in the cards
  - On scroll: subtle reveal animations
  - Theme toggle: small transition

6. Dark / Light Mode

- Implement a dark/light mode toggle.
- Remember user preference (localStorage).
- Use Tailwind’s dark mode pattern (class-based).

7. Rendering

- Use the normalized data array to populate the page.
- Each news card should show:
  - Image
  - Title
  - Publisher + Source badge
  - Publish date (human readable)
  - Summary
  - “Read more” link opening the article

Output:
Provide a single HTML file (mobile-first) that includes:

- Tailwind CDN
- GSAP CDN
- JavaScript that fetches + normalizes + deduplicates + renders + animates
- Dark/light toggle
- Responsive UI with good performance

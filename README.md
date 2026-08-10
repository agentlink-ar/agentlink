# AgentLink website

Static landing page for AgentLink. It currently works directly from local files (`index.html`).

## Open locally

Keep `index.html`, `og-image.jpg`, and the `logos/` folder together. Open `index.html` in your browser.

`og-image.jpg` is not a normal section of the page. It is the social-preview image that services such as WhatsApp, LinkedIn, Discord and X can use after the website has a public URL. It has little practical effect while the site only exists as local files.

## Connect the contact form with Make

The form is already prepared to send JSON with `POST`. When you are ready, create a **Custom Webhook** in Make and paste its webhook URL into the `data-endpoint` attribute of `#contactForm` in `index.html`.

Example:

```html
<form id="contactForm" data-endpoint="https://hook.us1.make.com/YOUR-WEBHOOK-ID" novalidate>
```

The browser sends approximately this payload:

```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@company.com",
  "companyName": "Acme Inc.",
  "companyWebsite": "https://acme.com",
  "role": "Owner / Founder",
  "companySize": "2–10 employees",
  "annualRevenue": null,
  "projectBudget": "Less than $10K",
  "goals": ["Internal operations"],
  "help": "...",
  "additionalContext": null,
  "source": "agentlink-website",
  "submittedAt": "ISO timestamp"
}
```

A useful Make scenario is:

`Custom Webhook → validate / normalize → Google Sheets or CRM → Gmail (send to agentnetlink@gmail.com) → optional confirmation email → Webhook response`

The site can still display the contact email while the webhook is not configured.

## When you buy a domain

At that point we should update the Open Graph image URL, add the final canonical URL, and create `robots.txt` + `sitemap.xml` using the real domain.

## Language routing

The root `index.html` is now a language router. It checks a previously selected language first and otherwise reads `navigator.languages` / `navigator.language`. Spanish browser languages are sent to `/es/`; all other languages are sent to `/en/`.

The actual pages are:
- `en/index.html` — English
- `es/index.html` — Español

Each localized page includes a compact manual EN/ES switch. A manual selection is stored in `localStorage` and takes priority on future visits to the root URL. This avoids browser geolocation permissions and external location APIs.

# Threat Model

## Project Overview

This repository currently deploys as a static real-estate landing page built with React 19, TypeScript, Vite, Tailwind CSS, and static image assets under `Desktop/app/`. In production, Replit builds `Desktop/app` and serves the generated `Desktop/app/dist` directory as a static site. There is no production backend, database, authentication system, or server-side API in the current deployment configuration.

## Assets

- **Site content integrity** -- buyers must receive the intended property content, images, and contact details without unauthorized script injection or defacement.
- **Contact information** -- the published email address, phone number, and property address are intentionally public, but they should not be altered or supplemented with attacker-controlled content.
- **Build and deployment configuration** -- the static build pipeline and asset selection determine what becomes reachable in production; incorrect inclusion of unrelated files could expose unintended content.

## Trust Boundaries

- **Browser to static site boundary** -- all visitors interact directly with client-side HTML, CSS, JavaScript, and image assets. The browser treats the delivered bundle as trusted code, so any injected or substituted script would execute with full page privileges.
- **Build pipeline to published assets boundary** -- only files included in the Vite build output under `Desktop/app/dist` become production-reachable. Repo artifacts outside that path are out of scope unless deployment configuration changes.
- **Application to third-party asset providers boundary** -- externally hosted resources such as Google Fonts introduce a supply-chain trust dependency for client rendering, though the site currently does not execute third-party hosted JavaScript in the deployed app.
- **Production vs dev-only boundary** -- `ReceiptGen/`, the Vite development server configuration, and unrelated standalone HTML files are not part of the configured production deployment and should be ignored unless separately deployed.

## Scan Anchors

- Production entry points: `Desktop/app/index.html`, `Desktop/app/src/main.tsx`, `Desktop/app/src/App.tsx`
- Highest-risk code areas: any future use of dynamic HTML rendering, external scripts, forms, or API integrations in `Desktop/app/src/`
- Public surface: entire deployed site is public; there are no authenticated or admin surfaces in current production config
- Dev-only / usually ignore: `ReceiptGen/`, `Desktop/index.html`, Vite dev server settings in `Desktop/app/vite.config.ts`, root-level experimental artifacts not included in `Desktop/app/dist`

## Threat Categories

### Tampering

For this project, tampering primarily means unauthorized modification of the client bundle or attacker-controlled content being introduced into the static site. The site must not render user-controlled HTML or import executable client-side code from untrusted locations. Any future interactive contact forms or CMS-fed content must validate and encode data before rendering.

### Information Disclosure

Because the deployed application is static, the main disclosure risk is accidental publication of secrets, internal data, or hidden files in the built output. Only intended public content should be included in `Desktop/app/dist`, and production pages must not expose credentials, build-time secrets, stack traces, or internal-only documents.

### Elevation of Privilege

There is no authentication or server-side privilege model in the current production deployment, so classic privilege-escalation issues are limited. The relevant guarantee is that the static site must not introduce client-side script injection or other code-execution paths that would let an attacker execute arbitrary JavaScript in visitors’ browsers.

### Denial of Service

As a static site, the application has little request-processing surface, but very large assets or broken third-party dependencies can still degrade availability or page load. The build should continue to avoid unnecessary external runtime dependencies and should keep the production page limited to static assets that browsers can fetch directly.

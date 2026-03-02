# OOD Dashboard Branding

This repository contains **branding overrides for the Open OnDemand (OOD) Dashboard**. All customization is done **without modifying the core OOD codebase**. Customizations are applied using supported configuration and variables.

This keeps the branding:

  - Safe for OOD upgrades  
  - Isolated from upstream source code  
  - Easy to deploy to dev and production  

---

## What Is Customized

Styled to visually match [https://ncar.ucar.edu](https://ncar.ucar.edu/) and [NCAR Koru theme](https://ncar.github.io/koru-jekyll-template/):

  - Fonts (Poppins) applied globally
  - Added NSF NCAR header above navbar
  - Add branding label (NSF NCAR HPC OnDemand) to navbar.
  - Style navbar (background color)
  - 2-layer footer with custom content and styling
  - Custom images (NSF and NCAR logos)

---

### Branding Files

- Custom CSS are static assets in public/branding
- NSF NCAR logos are static assets in public/branding
- Static assets are served by Apache relative to the Apache root under `/public/branding`
- Custom layouts are dynamic Ruby templates in `apps/dashboard/views/layouts` 

## File Structure

| File | Purpose |
|------|---------|
| `public/branding/ncar-branding.css` | NCAR-specific styles based on Koru theme |
| `public/branding/logo-ncar.png` | NSF NCAR logo used in header |
| `public/branding/nsf-logo.png` | NSF logo used in header/footer |
| `apps/dashboard/views/layouts/_footer.html.erb` | Custom dashboard footer override |
| `apps/dashboard/views/layouts/_org_header.html.erb` | Custom dashboard header addition |
| `apps/dashboard/views/layouts/application.html.erb` | Main OOD Ruby file edited to add header and footer |

## Config Files

Part of the theming is done via Open OnDemand config hooks that allow use of configuration rather than code for a limited number of features.
See BRANDING-DEVELOPMENT.md for more details.

~/ondemand/dev/dashboard/.env.local

~/ondemand/dev/dashboard/.env.overload

~/ondemand/ondemand.d/custom_branding.yml

~/ondemand/ondemand.d/public_url.yml

## Dev Process
Make all custom code changes in ~/ondemand/src/sage-ood-dashboard-branding on the On Demand server and check in manually using command line git.


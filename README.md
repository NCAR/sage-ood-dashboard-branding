# OOD Dashboard Branding

This repository contains **branding overrides for the Open OnDemand (OOD) Dashboard**.
All customization is done **without modifying the core OOD codebase**.  
Customizations are applied using supported development configuration and variables.

This keeps the branding:

- Safe for OOD upgrades  
- Isolated from upstream source code  
- Easy to deploy to dev and production  

---

## What Is Customized

Styled to visually match:
  - https://gdex.ucar.edu
  - https://scied.ucar.edu

Links:
   - Links use brand blueish grey color: `#c3d7ee` rgb(195, 215, 238)`
   - Custom teal hover highlight: `#00c1d5 rgb(0, 193, 213)`
  
- Header title text
- Navbar styling
- Footer layout and content
- Fonts (Poppins)
- Link colors and hover styles
- Background colors
- Custom images (NSF and NCAR logos)

### Header
- Add a custom NSF NCAR header above the nav bar containing NSF and NCAR logos with links.

### Footer
- Replaces the default OOD footer with a **GDEX-style three-section layout**:
  - Organization & navigation section
  - Legal/postal band
  - White NSF logo band


- Includes social media icons with generic NCAR resolutions

---

### Fonts

- Overrides all Dashboard fonts to use **Poppins**
- Applied globally:
  - Body text
  - Headings
  - Navigation menus
  - Modals
  - Buttons and inputs

- Uses Google Fonts via external CSS  
- No inline styles

---

### Branding Files

- Custom CSS are static assets in public/branding
- NSF NCAR logos are static assets in public/branding
- Static assets are served by Apache relative to the Apache root under `/public/branding`
- Custom layouts are dynamic Ruby templates in `apps/dashboard/views/layouts` 

## File Structure

| File | Purpose |
|------|---------|
| `public/branding/custom-fonts.css` | Global Poppins font override |
| `public/branding/custom-footer.css` | Custom footer override |
| `public/branding/custom-header.css` | Custom header override |
| `public/branding/custom-navbar.css` | Custom navbar override |
| `public/branding/main.min.css` | CSS theme copied from GDEX site |
| `public/branding/logo-ncar.png` | NSF logo used in header |
| `public/branding/nsf-logo.png` | NSF logo used in header/footer |
| `public/branding/NSF-NCAR_Logo_FullColor_RGB.png` | NSF logo used on main pane |
| `apps/dashboard/views/layouts/_footer.html.erb` | Custom dashboard footer override |
| `apps/dashboard/views/layouts/_org_header.html.erb` | Custom dashboard header override |
| `apps/dashboard/views/layouts/application.html.erb` | ? |
| `apps/dashboard/views/layouts/nav/_logo.html.erb` | ? |

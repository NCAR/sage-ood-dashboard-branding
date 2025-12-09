# Sage OOD Dashboard Branding

This repository contains **branding overrides for the Open OnDemand (OOD) Dashboard**

All customization is done **without modifying the core OOD codebase**.  
Overrides are applied using supported Rails view overrides and static asset injection.

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

### Logos & Images

- NSF logo deployed as a static asset in public/branding

## File Structure

| File | Purpose |
|------|---------|
| `app/views/layouts/_footer.html.erb` | Custom dashboard footer HTML override |
| `app/views/layouts/application.html.erb` | Adds external CSS links to branding assets |
| `public/branding/main.min.css` | CSS theme copied from GDEX site |
| `public/branding/custom-fonts.css` | Global Poppins font override |
| `public/branding/nsf-logo.png` | NSF logo used in footer |
| `.env.overload` | Create or override known OOD config parameters. This will override any environment variable whether it's set or not. |

All assets are served directly from `/public/branding` and **do not use the Rails asset pipeline**.

---

---

## Development Setup (`pun/dev`)

Follow these steps in the NCAR HPC Open OnDemand development environment.

---

### 1. Clone the upstream OOD repository

```bash
cd ~/ondemand/misc
git clone https://github.com/OSC/ondemand.git
cd ondemand
git checkout -b dev_work

Create Dashboard symlink
cd ~/ondemand/dev
ln -s ../misc/ondemand/apps/dashboard dashboard
cd dashboard

### Apply branding overrides

From this branding repository:

cp -r app/views/layouts/*   ~/ondemand/dev/dashboard/app/views/layouts/
cp -r public/branding/*   ~/ondemand/dev/dashboard/public/branding/

### Restart dev dashboard

From the OOD web interface:

Interactive Apps → Sandboxes → Development → Dashboard


Your branded dev dashboard will be visible at:

/pun/dev/dashboard
```
---

---

### Production Deployment

Deployment uses the same files but places them into OOD’s override directories.

No core source code is modified.

### 1. Copy branding files
sudo cp app/views/layouts/* /etc/ood/config/apps/dashboard/views/layouts/

sudo cp -r public/branding/* /etc/ood/config/apps/dashboard/public/branding/


(Exact paths depend on site configuration.)

### 2. Restart OOD
sudo systemctl restart ondemand-*

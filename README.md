# Third attempt: fixed form placeholder braces and create zip.
from pathlib import Path
from zipfile import ZipFile
import shutil, os

base = Path("/mnt/data")
work_dir = base / "gopal_website"
assets_dir = work_dir / "assets"

# Clean previous if exists
if work_dir.exists():
    shutil.rmtree(work_dir)
work_dir.mkdir(parents=True, exist_ok=True)
assets_dir.mkdir(parents=True, exist_ok=True)

# Copy provided assets (check existence)
src_photo = Path("/mnt/data/DSC_0788a.jpeg")
src_logo = Path("/mnt/data/89C17845-D504-4986-B3A2-BD0DF8029F6E.jpeg")
src_pdf = Path("/mnt/data/GT - NB & NCJ.pdf")

for src, dest_name in [(src_photo, "photo.jpg"), (src_logo, "logo.png"), (src_pdf, "CV.pdf")]:
    if src.exists():
        shutil.copy(src, assets_dir / dest_name)
    else:
        # create placeholder file
        (assets_dir / dest_name).write_text(f"Placeholder for {dest_name}")

# Common header/nav and footer
nav_html = '''
<header class="topbar">
  <div class="container nav-inner">
    <a class="brand" href="index.html"><img src="assets/logo.png" alt="CRESENT Logo" /></a>
    <nav class="menu">
      <a href="index.html">Home</a>
      <a href="about.html">About</a>
      <a href="experience.html">Experience</a>
      <a href="skills.html">Skills</a>
      <a href="contact.html">Contact</a>
    </nav>
  </div>
</header>
'''

footer_html = '''
<footer class="footer">
  <div class="container">
    <p>© Gopal Timilsina — IT & Operations Expert</p>
  </div>
</footer>
'''

styles_css = """
/* styles.css - Red & White theme */
:root{
  --bg:#fff;
  --card:#fff;
  --text:#0b2340;
  --muted:#6b7280;
  --accent:#c8232c; /* Crescent red */
  --accent-2:#f6f6f6;
}
*{box-sizing:border-box}
body{font-family:Inter,Roboto,Arial,sans-serif;background:var(--accent-2);color:var(--text);margin:0;line-height:1.6}
.container{max-width:980px;margin:0 auto;padding:20px}
.topbar{background:#fff;border-bottom:1px solid rgba(0,0,0,0.06)}
.nav-inner{display:flex;align-items:center;justify-content:space-between}
.brand img{height:48px}
.menu a{margin-left:18px;text-decoration:none;color:var(--accent);font-weight:600}
.hero{background:linear-gradient(90deg,var(--accent),#b71c24);color:#fff;padding:44px 20px;border-radius:6px;margin-top:18px}
.hero-inner{display:flex;gap:24px;align-items:center}
.hero .photo{width:120px;height:120px;border-radius:8px;overflow:hidden;border:4px solid rgba(255,255,255,0.12)}
.hero h1{margin:0;font-size:28px}
.tag{opacity:0.95;margin-top:6px}
.badge{background:rgba(255,255,255,0.12);padding:6px 10px;border-radius:6px;display:inline-block;margin-top:10px}
.section{background:#fff;padding:20px;border-radius:8px;margin-top:16px;box-shadow:0 6px 18px rgba(20,40,80,0.06)}
.section h2{margin-top:0;color:var(--text)}
.row{display:flex;gap:24px;flex-wrap:wrap}
.col{flex:1;min-width:240px}
ul{margin:8px 0 0 18px}
.btn{display:inline-block;padding:10px 14px;background:#fff;color:var(--accent);border-radius:6px;text-decoration:none;font-weight:700;border:2px solid rgba(0,0,0,0.04)}
.cv-link{background:#fff;color:var(--accent);padding:8px 12px;border-radius:6px;text-decoration:none;font-weight:700;border:1px solid rgba(0,0,0,0.06)}
.skills-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:12px}
.skill-card{padding:12px;border-radius:8px;background:linear-gradient(180deg,#fff,#fafafa);border:1px solid rgba(0,0,0,0.04)}
.contact-form input, .contact-form textarea{width:100%;padding:10px;border:1px solid #ddd;border-radius:6px;margin-top:8px}
.contact-form button{margin-top:10px;padding:10px 14px;background:var(--accent);color:#fff;border:none;border-radius:6px;font-weight:700}
.footer{padding:18px;text-align:center;color:var(--muted);font-size:14px;margin-top:18px}
@media(max-width:720px){
  .hero-inner{flex-direction:column;align-items:flex-start}
  .menu{display:none}
  .nav-inner{justify-content:space-between}
}
"""

# Page contents (use double braces for literal placeholder)
index_html = f"""<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Gopal Timilsina — IT & Operations Expert</title>
<link rel="stylesheet" href="styles.css" />
</head>
<body>
{nav_html}
<main class="container">
  <section class="hero">
    <div class="hero-inner">
      <div class="photo"><img src="assets/photo.jpg" alt="Gopal Timilsina" style="width:100%;height:100%;object-fit:cover" /></div>
      <div>
        <h1>Gopal Timilsina</h1>
        <p class="tag">IT & Operations Expert • 13+ years experience in IT support, logistics, and operations</p>
        <div class="badge">Former IT Officer — Rezayat Logistics Group / CRESENT Transportation Company (Saudi Arabia)</div>
        <div style="margin-top:12px">
          <a class="cv-link" href="assets/CV.pdf" download>Download CV (PDF)</a>
          <a class="btn" href="contact.html" style="margin-left:10px">Contact Me</a>
        </div>
      </div>
    </div>
  </section>

  <section class="section">
    <h2>Professional Summary</h2>
    <p>Experienced IT and operations professional with over 13 years of hands-on experience in managing IT infrastructure, providing technical support to executives and field staff, and coordinating large-scale logistics operations. Skilled in network & system administration, troubleshooting, team leadership, and cross-departmental coordination.</p>
  </section>

  <section class="section">
    <h2>Quick Links</h2>
    <div class="row">
      <div class="col">
        <h3>Experience</h3>
        <p>IT Officer — Rezayat Logistics Group / CRESENT Transportation</p>
      </div>
      <div class="col">
        <h3>Skills</h3>
        <p>IT Support, Network Administration, Team Management, Hardware Troubleshooting</p>
      </div>
      <div class="col">
        <h3>Channels</h3>
        <p><a href="https://youtube.com/@circuitwarp?si=pXDhredXaqUE_NxL" target="_blank">Circuit Warp</a><br/><a href="https://youtube.com/@timilsinavlogs?si=wCdy4iYuh6OMHKHf" target="_blank">Timilsina Vlogs</a></p>
      </div>
    </div>
  </section>

</main>
{footer_html}
</body>
</html>
"""

about_html = f"""<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>About — Gopal Timilsina</title>
<link rel="stylesheet" href="styles.css" />
</head>
<body>
{nav_html}
<main class="container">
  <section class="section">
    <h2>About Me</h2>
    <div class="row">
      <div class="col">
        <p>I am an IT and operations professional with over thirteen years of experience across logistics, transportation, and corporate IT environments. I have supported executives, managers, and large teams, ensuring systems run smoothly and operations remain uninterrupted.</p>
        <p>My strengths include problem-solving under pressure, coordinating cross-functional teams, and implementing practical solutions to improve system reliability and operational efficiency.</p>
      </div>
      <div class="col">
        <h3>Career Objectives</h3>
        <ul>
          <li>Secure a senior IT/support role where I can combine technical expertise with operational leadership.</li>
          <li>Contribute to process improvements and system stability in logistics and operations companies.</li>
          <li>Grow as a leader and mentor for technical and non-technical staff.</li>
        </ul>
      </div>
    </div>
  </section>
</main>
{footer_html}
</body>
</html>
"""

experience_html = f"""<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Experience — Gopal Timilsina</title>
<link rel="stylesheet" href="styles.css" />
</head>
<body>
{nav_html}
<main class="container">
  <section class="section">
    <h2>Professional Experience</h2>

    <article style="margin-bottom:12px">
      <h3>IT Officer — Rezayat Logistics Group / CRESENT Transportation Company</h3>
      <p><strong>Location:</strong> Saudi Arabia</p>
      <p><strong>Overview:</strong> CRESENT Transportation Company is a sister company of the Rezayat Group, focused on logistics, fleet management, and supply chain services across the Kingdom. As the organization’s IT Officer I supported logistics operations through reliable IT systems and responsive end-user support.</p>

      <h4>Key Responsibilities</h4>
      <ul>
        <li>Managed and maintained IT infrastructure across offices and logistics sites, including servers, workstations, and network devices.</li>
        <li>Provided Tier-1 and Tier-2 support to staff, managers, and VIP users to resolve technical issues quickly.</li>
        <li>Ensured system uptime by performing routine maintenance, updates, and backups.</li>
        <li>Implemented security best practices and assisted with data protection measures.</li>
        <li>Coordinated with operations and logistics teams to integrate IT solutions that improved workflow efficiency.</li>
      </ul>

      <h4>Achievements</h4>
      <ul>
        <li>Reduced average incident resolution time by improving support processes.</li>
        <li>Maintained high availability of critical logistics systems during peak operation periods.</li>
        <li>Supported cross-site communication and data access for operations teams.</li>
      </ul>
    </article>

  </section>

  <section class="section">
    <h2>Additional Roles & Short-Term Projects</h2>
    <p>Worked on various projects including hardware rollouts, user training, and small automation initiatives to streamline routine tasks.</p>
  </section>
</main>
{footer_html}
</body>
</html>
"""

skills_html = f"""<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Skills — Gopal Timilsina</title>
<link rel="stylesheet" href="styles.css" />
</head>
<body>
{nav_html}
<main class="container">
  <section class="section">
    <h2>Skills</h2>
    <div class="skills-grid">
      <div class="skill-card"><strong>IT Support</strong><div class="muted">User support, ticketing, documentation</div></div>
      <div class="skill-card"><strong>Network Administration</strong><div class="muted">LAN/WAN, routers, switches</div></div>
      <div class="skill-card"><strong>System Maintenance</strong><div class="muted">Windows Server, updates, backups</div></div>
      <div class="skill-card"><strong>Hardware Troubleshooting</strong><div class="muted">Workstations, peripherals, printers</div></div>
      <div class="skill-card"><strong>Operations Management</strong><div class="muted">Coordination, scheduling, team leadership</div></div>
      <div class="skill-card"><strong>Communication</strong><div class="muted">Stakeholder liaison, training</div></div>
      <div class="skill-card"><strong>Vlogging & Content</strong><div class="muted">YouTube channel management, video reviews</div></div>
      <div class="skill-card"><strong>Languages</strong><div class="muted">English (fluent), basic Czech</div></div>
    </div>
  </section>

  <section class="section">
    <h2>Hobbies & Interests</h2>
    <ul>
      <li>Vlogging and content creation</li>
      <li>Travel and cultural exploration</li>
      <li>Learning new languages</li>
      <li>Technology reviews and tutorials</li>
    </ul>
  </section>
</main>
{footer_html}
</body>
</html>
"""

contact_html = f"""<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Contact — Gopal Timilsina</title>
<link rel="stylesheet" href="styles.css" />
</head>
<body>
{nav_html}
<main class="container">
  <section class="section">
    <h2>Contact Me</h2>
    <p>If you would like to contact me, please use the form below or email me directly at <strong>gopal.tmlsna@gmail.com</strong>.</p>
    <div class="row">
      <div class="col">
        <h3>Message Form</h3>
        <!-- Formspree: to receive emails, sign up at Formspree and replace the action URL with your form endpoint -->
        <form class="contact-form" action="https://formspree.io/f/{{your_form_id}}" method="POST">
          <label>Name</label>
          <input type="text" name="name" required />
          <label>Email</label>
          <input type="email" name="_replyto" required />
          <label>Message</label>
          <textarea name="message" rows="6" required></textarea>
          <!-- You can add a hidden subject or redirect after submit -->
          <input type="hidden" name="_subject" value="Website Contact — Gopal Timilsina" />
          <button type="submit">Send Message</button>
        </form>
        <p style="margin-top:8px;font-size:13px;color:var(--muted)"><em>Note:</em> Replace <code>{{your_form_id}}</code> in the form action with your Formspree form ID. See README for quick setup.</p>
      </div>
      <div class="col">
        <h3>Other Ways to Reach Me</h3>
        <p><strong>Email:</strong> gopal.tmlsna@gmail.com</p>
        <p><strong>YouTube:</strong><br/>
          <a href="https://youtube.com/@circuitwarp?si=pXDhredXaqUE_NxL" target="_blank">Circuit Warp</a><br/>
          <a href="https://youtube.com/@timilsinavlogs?si=wCdy4iYuh6OMHKHf" target="_blank">Timilsina Vlogs</a>
        </p>
        <p><strong>Location:</strong> Romania</p>
      </div>
    </div>
  </section>
</main>
{footer_html}
</body>
</html>
"""

readme_text = """
Gopal Timilsina — Website Package
================================

Files included:
- index.html
- about.html
- experience.html
- skills.html
- contact.html
- styles.css
- assets/photo.jpg (your photo)
- assets/logo.png (CRESENT logo)
- assets/CV.pdf (your CV)

How to configure the contact form (Formspree):
1. Go to https://formspree.io/ and sign up (free plan is fine).
2. Create a new form and copy the form endpoint (it looks like: https://formspree.io/f/xyzabc).
3. Open contact.html and replace the form action value https://formspree.io/f/{{your_form_id}} with the endpoint you received.
   Example:
     <form action="https://formspree.io/f/xyzabc" method="POST">

4. Optionally set up email forwarding inside Formspree so messages go to: gopal.tmlsna@gmail.com

Quick GitHub Pages deployment:
1. Create a free GitHub account at https://github.com/ (if you don't have one).
2. Create a new public repository named (for example) gopal-website.
3. On your computer, unzip this package and upload all files and the 'assets' folder to the repository root.
   - You can drag-and-drop files in the GitHub web UI.
4. In the repository, go to Settings → Pages (or Settings → Code and automation → Pages).
5. Under "Build and deployment", choose "Deploy from a branch".
6. Choose the branch 'main' (or 'master') and folder '/' (root), then Save.
7. After a minute, GitHub will publish your site at: https://<your-github-username>.github.io/<repository-name>/
   Example: https://gopaltimilsina.github.io/gopal-website/

Notes:
- If you want your website at the root (https://gopaltimilsina.github.io/), name the repository exactly <your-github-username>.github.io
- To use a custom domain, add a CNAME file and configure DNS; instructions are in GitHub Pages docs.

If you want, I can:
- Update the contact form action with your Formspree endpoint once you provide it.
- Customize wording further or change visuals.
- Prepare a ready-to-publish GitHub repo (I can provide a ZIP that is Git-ready).
"""

# Write files
(work_dir / "index.html").write_text(index_html, encoding="utf-8")
(work_dir / "about.html").write_text(about_html, encoding="utf-8")
(work_dir / "experience.html").write_text(experience_html, encoding="utf-8")
(work_dir / "skills.html").write_text(skills_html, encoding="utf-8")
(work_dir / "contact.html").write_text(contact_html, encoding="utf-8")
(work_dir / "styles.css").write_text(styles_css, encoding="utf-8")
(work_dir / "README_GitHubPages.txt").write_text(readme_text, encoding="utf-8")

# Create zip
zip_path = base / "gopal_website_package.zip"
if zip_path.exists():
    zip_path.unlink()
with ZipFile(zip_path, "w") as z:
    for p in work_dir.rglob("*"):
        z.write(p, arcname=str(p.relative_to(work_dir)))

str(zip_path)

---
title: "Portfolio Project: Personal Portfolio Guide"
weight: 1
toc: true
---

## Introduction

A portfolio website is the single most important asset you can have when applying for web development jobs or internships. Employers and hiring managers look at portfolios before they look at resumes. A well-built portfolio demonstrates your skills more effectively than any bullet point on a piece of paper ever could.

This is not a class exercise that disappears after grading -- this is **your site**, representing you professionally. It will live on the internet and serve as your calling card when you start applying for positions. The work you put in here pays dividends long after the semester ends.

This project combines everything you have learned in weeks 1 through 7 of this course: HTML structure and semantics, CSS styling and layout, responsive design with media queries, and JavaScript interactivity. It is the culmination of those skills applied to a single, polished, real-world product.

**What you will learn:**

- How to plan content strategy before writing any code
- How to create a visual design system (colors, fonts, spacing) that holds an entire site together
- How to build a responsive, multi-section page that works on every device
- How to implement meaningful JavaScript interactivity
- How to deploy and maintain a professional web presence

This project is worth **150 points** and is due **Sunday, March 8, 2026** at 11:59 PM.

---

## Before You Start

{{% steps %}}

### Write Your Content First

Before you write a single line of code, write all of your content in a plain text document or Google Doc. This includes your bio, your project descriptions, your skills list, and your hero headline. Design follows content -- not the other way around. If you start coding without content, you will end up redesigning everything once you add real text.

### Gather Your Project Materials

You need at least 3 projects to showcase. For each project, collect:

- A screenshot or image of the project
- The live URL (if deployed)
- The GitHub repository link
- A list of technologies used
- A 2-3 sentence description of what the project does and what you learned

### Use Your Class Projects

You **can** include projects from this course. Assignment 1 (HTML/CSS component), Assignment 2 (responsive page), and Group Project 1 (team website) are all legitimate portfolio pieces. You built them, you learned from them, and they demonstrate real skills. Do not feel like you need outside projects to fill your portfolio.

### Choose a Color Palette and Fonts

Before coding, decide on 2-4 colors and 1-2 fonts. Write them down. A color palette typically includes:

- A **primary color** (your main brand color)
- An **accent color** (for buttons, links, highlights)
- A **neutral color** (for backgrounds and text)
- Optionally, a **secondary color** for variety

For fonts, one font for headings and one for body text is the standard approach. Google Fonts is free and has excellent options. Having these decisions made before you start coding prevents the "I keep changing colors" trap that wastes hours.

### Look at Real Developer Portfolios

Browse Dribbble, Behance, and awwwards for inspiration. Search "developer portfolio" and spend 15-20 minutes observing patterns: How do developers structure their navigation? How do they present projects? What makes a hero section compelling? Observe and take notes -- but do not copy. Your portfolio should reflect you, not someone else.

### Set Up Your GitHub Repository

Create a new **public** GitHub repository named `portfolio` or `[yourname]-portfolio`. Clone it to your computer and create a feature branch (e.g., `feature/portfolio-v1`) before you start building.

{{% /steps %}}

{{< callout type="important" >}}
Writing content first is the most important step. A portfolio with great content and a simple design will always beat a portfolio with flashy design and empty or placeholder content.
{{< /callout >}}

---

## Understanding the Requirements

Here is what each rubric category is worth and what I am looking for:

| Criteria                   | Points  | What This Means                                                     |
| -------------------------- | ------- | ------------------------------------------------------------------- |
| HTML Structure & Semantics | 20      | Clean, semantic, accessible HTML with proper heading hierarchy      |
| CSS & Visual Design        | 30      | Professional appearance, cohesive design system, smooth transitions |
| Responsiveness             | 25      | Works perfectly on mobile, tablet, and desktop with no overflow     |
| JavaScript Interactivity   | 20      | At least one meaningful, polished interactive feature               |
| Content Quality            | 20      | Well-written, professional, real content (no placeholder text)      |
| Deployment                 | 15      | Successfully deployed with a working live URL                       |
| Code Quality               | 10      | Clean, organized, well-commented code                               |
| Git Usage                  | 10      | Regular, meaningful commits showing steady progress                 |
| **Total**                  | **150** |                                                                     |

{{< callout type="warning" >}}
**CSS & Visual Design is worth 30 points** -- the highest single category. A consistent design system (colors, fonts, spacing) matters more than complex features. Simple and polished beats complex and messy every time. Invest in consistency.
{{< /callout >}}

{{< callout type="important" >}}
**Content Quality is worth 20 points.** Real, well-written content is required -- not "Lorem ipsum" or placeholder text. Spend time writing a compelling bio and clear project descriptions. Employers will read your portfolio content, and so will I.
{{< /callout >}}

---

## Planning Your Portfolio

Your portfolio needs six required sections. Here is what each one should contain and how to think about it:

{{< tabs items="Header/Nav,Hero/Intro,About,Projects,Contact,Footer" >}}
{{< tab >}}
**Header and Navigation**

Your name or a simple logo sits at the top, along with navigation links to each section of the page.

**On mobile:** Navigation links should stack vertically or collapse behind a hamburger menu icon. The key is that they remain accessible -- a user on a phone should be able to reach every section easily.

**On desktop:** Navigation links display horizontally in a row, typically with your name on the left and links on the right.

**Tips:**

- Keep navigation labels short and clear: "About," "Projects," "Contact" -- not "Learn More About Me"
- Make sure every link actually works and scrolls to (or navigates to) the correct section
- The navigation should be visible on every screen size without requiring the user to hunt for it
  {{< /tab >}}
  {{< tab >}}
  **Hero / Introduction Section**

This is the first thing every visitor sees -- make it count. You have about 3 seconds to communicate who you are and what you do.

**What to include:**

- A compelling headline that communicates your value. Not "Welcome to my portfolio" -- instead, something specific like "I build accessible, responsive web experiences" or "Front-end developer focused on clean, user-friendly design"
- A brief introduction of 1-2 sentences expanding on the headline
- A call-to-action button that leads somewhere meaningful ("View My Work" scrolling to the Projects section, or "Contact Me" scrolling to Contact)
- A professional photo is optional but recommended -- it makes the site feel personal and real

**Think about:** The hero sets the tone for your entire portfolio. A clear, confident headline tells visitors they are in the right place.
{{< /tab >}}
{{< tab >}}
**About Section**

This is where you tell your story in more depth. Think of it as introducing yourself at a professional networking event.

**What to include:**

- Your background: where you are in your education, what you are studying, how you got interested in web development
- What drives you: what kind of projects excite you, what problems you want to solve
- A skills or technologies list: HTML, CSS, JavaScript, Git, and any other tools or languages you know
- Education information: your school, your program, expected graduation

**Keep it genuine.** Employers value authenticity. You do not need to pretend you have 10 years of experience. Being a student who is actively learning and building projects is compelling on its own.
{{< /tab >}}
{{< tab >}}
**Projects Section (minimum 3)**

This is the heart of your portfolio. Each project should include:

- A **screenshot or image** of the project (take clear, well-cropped screenshots)
- A **title** for the project
- A **description** that explains what the project does and what you learned building it (2-3 sentences)
- The **technologies used** (HTML, CSS, JavaScript, etc.)
- A **link** to the live demo and/or the GitHub repository

**Important notes:**

- Write descriptions that explain what the project **does** and what you **learned**, not just a list of technologies. "A responsive restaurant website built with CSS Grid that taught me mobile-first design and media queries" is far more compelling than "HTML, CSS, JavaScript"
- Class projects absolutely count: Assignment 1, Assignment 2, and Group Project 1 are all legitimate projects you built and learned from
- **Quality over quantity** -- 3 well-presented projects with clear descriptions and working links beats 6 poorly described ones with broken screenshots
  {{< /tab >}}
  {{< tab >}}
  **Contact Section**

Make it easy for potential employers or collaborators to reach you.

**Options:**

- A **contact form** with fields for name, email, and message. For this class, the form can be non-functional (it does not need to actually send emails), but it should look complete and professional
- **Direct contact information** such as your email address
- **Social and professional links:** LinkedIn profile, GitHub profile, and any other relevant platforms

**Tips:**

- At minimum, include your GitHub and LinkedIn links -- these are standard for developer portfolios
- If you include a form, include clear labels and placeholder text so visitors know what to enter
- Make links open in a new tab so visitors do not navigate away from your portfolio
  {{< /tab >}}
  {{< tab >}}
  **Footer**

The footer anchors the bottom of your page and provides a sense of completeness.

**What to include:**

- A copyright notice with the current year (e.g., "2026 Your Name")
- Optionally: additional navigation links, social media icons, or a brief tagline

**Keep it simple.** The footer is not the place for heavy content. It should feel like a clean ending to the page.
{{< /tab >}}
{{< /tabs >}}

---

## Content Writing Tips

Good content separates a professional portfolio from a student project. Here is how to write each section well:

**Hero headline:** Be specific about what you do. "I build responsive web applications" tells the visitor something. "Welcome to my website" tells them nothing. Think about what makes you interesting or what you are working toward.

**About section:** Write as if you are introducing yourself at a professional event -- friendly, confident, and genuine. Aim for 2-3 paragraphs maximum. Start with who you are, move to what interests you, and end with what you are currently learning or working on.

**Project descriptions:** Answer two questions in 2-3 sentences: "What does this project do?" and "What did I learn building it?" This shows both your output and your growth as a developer.

**Proofread everything.** Spelling errors and grammar mistakes on a portfolio are a red flag for employers. They signal a lack of attention to detail -- the exact opposite of what you want to communicate. Read every word at least twice. Better yet, ask a friend or classmate to read your content before you finalize it.

{{< callout type="info" >}}
Ask a friend or classmate to read your portfolio content before you submit. A fresh pair of eyes catches mistakes you have become blind to after reading your own words multiple times.
{{< /callout >}}

---

## Design Guidelines

{{< tabs items="Minimalist (Recommended),Creative" >}}
{{< tab >}}
**Minimalist Approach** -- recommended for beginners and most portfolios.

- Clean layouts with lots of whitespace
- 2-3 colors maximum: one primary, one accent, one neutral
- 1-2 fonts: one for headings, one for body text (or one font for everything)
- Focus on readability and content over visual effects
- Easier to execute well and harder to mess up
- Most professional developer portfolios follow a minimalist approach

A minimalist portfolio signals confidence: you trust your content and your work to speak for themselves without needing visual noise to fill the space.
{{< /tab >}}
{{< tab >}}
**Creative Approach** -- for students with stronger design skills.

- More visual elements: gradients, illustrations, bold color choices
- Unique layouts that break from the standard top-to-bottom flow
- Animations and transitions that enhance (not distract from) the content
- Requires stronger design instincts to pull off well

**The risk:** a creative portfolio that is not executed well can look cluttered, unprofessional, or distracting. If you choose this route, remember that **less is still more**. Every visual element should serve a purpose. If it does not help the visitor understand who you are or see your work, remove it.
{{< /tab >}}
{{< /tabs >}}

### Design Do's and Don'ts

**Do:**

- Use consistent spacing throughout the entire page
- Ensure text has strong contrast against its background (dark text on light backgrounds, or light text on dark backgrounds)
- Make buttons and links obviously clickable -- use color, hover effects, and clear labels
- Use high-quality images (Unsplash provides free, professional stock photos)
- Keep navigation simple and predictable -- visitors should not have to guess how to move around your site

**Don't:**

- Use more than 3-4 colors -- too many colors make a site feel chaotic
- Make body text smaller than 16px -- anything smaller is difficult to read, especially on mobile
- Use low-quality, pixelated, or stretched images -- they undermine your credibility instantly
- Create walls of text with no visual breaks -- use headings, whitespace, and images to break up long content
- Add animations that serve no purpose -- a bouncing logo or spinning icon distracts without adding value
- Hide important information behind interactions -- your core content should be visible without requiring clicks

---

## JavaScript Feature Ideas

Your portfolio needs at least **one** interactive JavaScript feature. Here are options to consider:

- **Smooth scroll navigation** -- clicking a nav link scrolls smoothly to that section instead of jumping instantly. A polished touch that improves the browsing experience
- **Mobile menu toggle** -- a hamburger icon that shows and hides the navigation links on mobile screens. Essential for responsive sites
- **Form validation** -- check that required fields in the contact form are filled before "submitting." Display helpful error messages for empty or incorrectly formatted fields
- **Theme toggle (light/dark mode)** -- a button that switches the entire color scheme between light and dark. Demonstrates your understanding of CSS custom properties and DOM manipulation
- **Animated elements on scroll** -- elements fade in or slide up as the user scrolls down the page. Adds a dynamic, professional feel
- **Project filter/sort** -- buttons that filter your projects by technology or category (e.g., "HTML/CSS," "JavaScript," "All"). Shows advanced DOM manipulation skills
- **Typing animation** -- text in the hero section that "types" itself out letter by letter. An attention-grabbing effect when done well

{{< callout type="info" >}}
Pick **one** feature and make it work well. A polished smooth scroll is worth more than a half-broken dark mode toggle AND a buggy typing animation combined. Quality over quantity applies to JavaScript features too.
{{< /callout >}}

---

## Recommended Approach

{{% steps %}}

### Write All Content First

Open a text document and write your hero headline, your bio, your project descriptions, and your skills list. Do not open VS Code yet. Content drives design.

### Sketch Your Layout

Grab a piece of paper and draw quick wireframes of your page at three sizes:

- **Mobile (320px)** -- everything stacks vertically in a single column
- **Tablet (768px)** -- some elements go side-by-side, more breathing room
- **Desktop (1024px+)** -- full layout with maximum use of horizontal space

These sketches do not need to be beautiful -- they are a roadmap so you know where each section goes before you start coding.

### Build the Complete HTML Structure

Create all six sections in `index.html` with your real content in place. Use semantic elements: `<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`. Include all headings, paragraphs, images, and links. Your HTML should be complete and readable in the browser (unstyled) before you write any CSS.

### Style for Mobile First

Write your default CSS (no media queries) for the smallest screen size. Everything should stack vertically. Focus on:

- Typography (font family, sizes, line height)
- Colors (background, text, accents)
- Spacing (padding, margins)
- Basic layout (single column, full-width elements)

This is your baseline. Every screen gets these styles.

### Add Tablet and Desktop Breakpoints

Add `@media (min-width: 768px)` for tablet adjustments and `@media (min-width: 1024px)` for desktop enhancements. Only change what needs to change at each size -- your mobile styles carry forward automatically.

### Implement Your JavaScript Feature

Choose one interactive feature and build it. Test it thoroughly in the browser. Check the console (F12) for any errors. Make sure it enhances the experience without breaking anything.

### Test Responsiveness Thoroughly

Open browser DevTools and drag the viewport width from 320px all the way to 1200px. Watch for anything that breaks, overflows, or looks awkward at any width. Fix issues as you find them. Test on a real phone if you can.

### Validate Your HTML

Run your page through the [W3C HTML Validator](https://validator.w3.org/). Fix any errors it reports. Valid HTML is part of the rubric.

### Deploy and Test the Live Site

Deploy to GitHub Pages, Netlify, or Vercel. Visit the live URL and test everything: all links work, all images load, the JavaScript feature functions, and the responsive behavior is correct on the live site (not just locally).

### Create Your Pull Request

Push your feature branch and create a Pull Request to `main`. In the PR description, include:

- A summary of what you built
- Your design choices (color palette, font choices, layout approach)
- Your live URL

{{% /steps %}}

---

## Project Structure

{{< filetree/container >}}
{{< filetree/folder name="portfolio" >}}
{{< filetree/file name="index.html" >}}
{{< filetree/folder name="css" >}}
{{< filetree/file name="styles.css" >}}
{{< /filetree/folder >}}
{{< filetree/folder name="js" >}}
{{< filetree/file name="main.js" >}}
{{< /filetree/folder >}}
{{< filetree/folder name="images" >}}
{{< filetree/file name="profile.jpg" >}}
{{< filetree/file name="project1.png" >}}
{{< filetree/file name="project2.png" >}}
{{< filetree/file name="project3.png" >}}
{{< /filetree/folder >}}
{{< filetree/file name="README.md" >}}
{{< /filetree/folder >}}
{{< /filetree/container >}}

Keep your file structure clean and organized. CSS goes in a `css/` folder, JavaScript goes in a `js/` folder, and all images go in an `images/` folder. Your main HTML file should be named `index.html` -- deployment platforms expect this name.

---

## Deployment Guide

Your portfolio must be deployed to a live URL. Choose one of these free options:

{{< tabs items="GitHub Pages (Simplest),Netlify,Vercel" >}}
{{< tab >}}
**GitHub Pages** -- the simplest option for static HTML/CSS/JS sites.

1. Go to your repository on GitHub
2. Click **Settings** > **Pages** (in the left sidebar)
3. Under "Source," select **Deploy from a branch**
4. Choose the **main** branch and **/ (root)** folder
5. Click **Save**
6. Wait 1-2 minutes, then refresh. Your live URL will appear

Your site will be available at `https://username.github.io/portfolio/`. Every time you push to `main`, the site updates automatically.

Make sure your main page is named `index.html` -- GitHub Pages looks for this file by default.
{{< /tab >}}
{{< tab >}}
**Netlify** -- automatic deployments with a custom subdomain.

1. Create a free account at netlify.com
2. Click **Add new site** > **Import an existing project**
3. Connect your GitHub account and select your repository
4. Leave the build settings at their defaults (for a static site, no build command is needed)
5. Click **Deploy site**

Netlify will give you a URL like `random-name.netlify.app`. You can customize the subdomain in your site settings. The site redeploys automatically when you push to `main`.
{{< /tab >}}
{{< tab >}}
**Vercel** -- another popular deployment platform with fast global hosting.

1. Create a free account at vercel.com
2. Click **Add New** > **Project**
3. Connect your GitHub account and import your repository
4. Leave the default settings (Vercel auto-detects static sites)
5. Click **Deploy**

Vercel provides a URL like `project-name.vercel.app`. Like Netlify, it redeploys automatically when you push to `main`.
{{< /tab >}}
{{< /tabs >}}

{{< callout type="info" >}}
All three platforms are completely free. GitHub Pages is the simplest option with the least setup. Choose whichever you prefer -- the important thing is that your portfolio is live and accessible via a public URL.
{{< /callout >}}

---

## Video Explanation Guide

Your video is a required part of the submission. Record a screen recording that walks through your portfolio.

{{% steps %}}

### Demo Your Portfolio

Show your portfolio working on desktop and mobile. Use browser DevTools to demonstrate the responsive behavior -- resize from wide to narrow so the viewer can see how your layout adapts.

### Walk Through the Code

Open your code in VS Code and explain:

- Your HTML structure: how you organized the six sections, which semantic elements you used
- Your CSS approach: your design system (colors, fonts, spacing), how you implemented responsive design, your media query strategy
- Your JavaScript feature: what it does, how it works, and why you chose it

### Explain Your Design Choices

Talk about why you chose your color palette, your fonts, and your layout. What inspired your design? What trade-offs did you make? What would you improve if you had more time?

{{% /steps %}}

{{< callout type="info" >}}
Typical length is 5-8 minutes. Upload your video directly to D2L -- do not use YouTube or Loom.
{{< /callout >}}

---

## README Template

Your repository needs a `README.md` file. Here is what to include:

```
# [Your Name] - Portfolio

## Live Site
[your-deployment-url-here]

## About
A brief description of your portfolio and what it showcases.

## Technologies Used
- HTML5
- CSS3 (Flexbox / Grid)
- JavaScript
- [Any other tools]

## Features
- Responsive design (mobile, tablet, desktop)
- [Your JavaScript feature]
- [Any other notable features]

## Screenshots
[Include at least one desktop and one mobile screenshot]

## Setup
1. Clone the repository
2. Open index.html in a browser
```

---

## Submission Checklist

Before submitting, verify every item:

- [ ] GitHub repository is **public**
- [ ] All 6 required sections are present: Header/Nav, Hero, About, Projects, Contact, Footer
- [ ] At least 3 projects showcased with descriptions, technologies, and links
- [ ] HTML is semantic and passes the [W3C Validator](https://validator.w3.org/) with no errors
- [ ] CSS is in an external stylesheet (not inline or in `<style>` tags)
- [ ] Site is responsive with at least 2 breakpoints (768px and 1024px)
- [ ] At least one JavaScript interactive feature works correctly with no console errors
- [ ] Consistent design system throughout (colors, fonts, spacing)
- [ ] Site is deployed and the live URL works
- [ ] README includes your name, live URL, technologies used, features, and screenshots
- [ ] Pull Request created with a description and your live URL
- [ ] Video explanation uploaded directly to D2L

---

## Troubleshooting & Common Mistakes

{{% details title="My portfolio looks good on desktop but terrible on mobile" closed="true" %}}
You likely built desktop-first and are now fighting your own CSS. The fix is to rebuild with a mobile-first approach: write your default styles for the smallest screen (no media queries), then add `min-width` media queries to enhance for larger screens. This is the same approach covered in Assignment 2. It feels like starting over, but it is faster and produces better results than trying to "fix" mobile after the fact.
{{% /details %}}

{{% details title="My images are blurry or stretched" closed="true" %}}
Use high-resolution images and make sure they are not being stretched beyond their natural dimensions. In your CSS, set images to scale responsively:

```
max-width: 100%
height: auto
```

This ensures images scale down on small screens without stretching. For screenshots, take them at the highest resolution your screen supports.
{{% /details %}}

{{% details title="I don't have 3 projects to show" closed="true" %}}
Use your class projects. Assignment 1 (HTML/CSS component), Assignment 2 (responsive page), and Group Project 1 (team website) are all legitimate portfolio pieces. You built them, you learned from them, and they demonstrate real skills. You can also include personal projects, coding challenges from sites like Frontend Mentor, or redesigns of existing websites. Three well-presented projects is all you need.
{{% /details %}}

{{% details title="My JavaScript feature doesn't work" closed="true" %}}
Start with the basics:

1. Open the browser console (F12 > Console tab) and look for error messages -- they tell you exactly which line has the problem
2. Verify your JavaScript file is linked correctly in the HTML. Check that the `<script>` tag has the correct file path and is placed just before the closing `</body>` tag
3. Use `console.log()` statements to trace what your code is doing at each step
4. Test one small piece at a time instead of writing everything at once

If you are stuck, simplify. Smooth scroll navigation is one of the most straightforward features to implement and works well on any portfolio.
{{% /details %}}

{{% details title="My site looks different on GitHub Pages than locally" closed="true" %}}
Common causes:

- **File path case sensitivity:** GitHub Pages runs on Linux, which is case-sensitive. `Images/photo.jpg` and `images/photo.jpg` are different files on GitHub Pages but the same file on Mac. Check all file references for correct capitalization
- **Files not committed:** Run `git status` to check if all your files (especially images and CSS) are tracked and committed
- **Browser cache:** Hard refresh with Cmd+Shift+R (Mac) or Ctrl+Shift+R (Windows) to clear the cached version
- **Wrong branch deployed:** Verify in Settings > Pages that you are deploying from the correct branch
  {{% /details %}}

{{% details title="I don't know what to write for my bio" closed="true" %}}
Start with a simple template and then make it more natural:

"I am a [year] student at [school] studying [major]. I am passionate about [what interests you about web development]. I am currently learning [skills you are developing] and enjoy building [types of projects you like]."

Once you have the basic structure, rewrite it in your own voice. Remove anything that sounds generic and add details that are specific to you. Read it out loud -- if it sounds like it could describe anyone, make it more personal.
{{% /details %}}

{{% details title="My design looks inconsistent across sections" closed="true" %}}
This usually means you did not establish a design system before coding. The fix:

1. Pick your exact colors (write down the hex codes)
2. Pick your exact fonts (write down the font names)
3. Pick your spacing values (e.g., 1rem, 2rem, 3rem for small, medium, large gaps)
4. Go through every section and apply these same values consistently

Using CSS custom properties (variables) at the top of your stylesheet makes this easier to maintain -- change a color in one place and it updates everywhere.
{{% /details %}}

---

## After the Course

Your portfolio is a **living document** -- it does not stop when the semester ends.

- **Add new projects** as you build them. Group Project 2 and your Final Project are both strong additions
- **Update your skills section** as you learn React, APIs, and new technologies throughout the rest of the course
- **Keep your deployed URL active** and include it on your resume and LinkedIn profile
- **Revisit and improve the design** as your skills grow. The portfolio you build now will look different from the one you have in a year -- and that is a sign of growth
- **Show it in interviews.** When an employer asks to see your work, sending them a live portfolio URL is far more impressive than a list of GitHub repositories

---

## Key Takeaways

{{< callout emoji="🎯" >}}

- **Write content before you code** -- a portfolio with great content and simple design beats a portfolio with flashy design and empty content
- **Design system first:** choose your colors, fonts, and spacing BEFORE you start coding, and apply them consistently across every section
- **Mobile-first CSS** carries over from Assignment 2 -- use the same approach here: default styles for mobile, `min-width` queries for larger screens
- **Class projects count** as portfolio pieces -- Assignment 1, Assignment 2, and Group Project 1 are all legitimate work you built and learned from
- **One polished JavaScript feature** is worth more than three broken ones -- pick one and make it work flawlessly
- **Deploy early** so you can test the live version throughout development, not just at the end
- **Proofread everything** -- spelling errors on a portfolio are a red flag for employers and signal a lack of attention to detail
- **This portfolio lives beyond the class** -- it is a real professional asset for job and internship applications. The time you invest now pays off for years
  {{< /callout >}}

---

## Additional Resources

### Video Tutorials

{{< cards >}}
{{< card link="https://www.youtube.com/c/DesignCourse" title="DesignCourse: Build a Portfolio" subtitle="Portfolio design and development walkthrough" icon="play" >}}
{{< card link="https://www.youtube.com/kepowob" title="Kevin Powell: Responsive Portfolio Website" subtitle="Mobile-first responsive portfolio build" icon="play" >}}
{{< card link="https://www.youtube.com/c/TraversyMedia" title="Traversy Media: Build a Responsive Portfolio" subtitle="Full portfolio website tutorial" icon="play" >}}
{{< /cards >}}

### Design Inspiration

{{< cards >}}
{{< card link="https://dribbble.com" title="Dribbble" subtitle="Search 'developer portfolio' for design inspiration" icon="eye" >}}
{{< card link="https://www.behance.net" title="Behance" subtitle="Search 'web developer portfolio' for creative examples" icon="eye" >}}
{{< card link="https://www.awwwards.com" title="Awwwards" subtitle="Award-winning web designs for high-end inspiration" icon="academic-cap" >}}
{{< /cards >}}

### Tools

{{< cards >}}
{{< card link="https://fonts.google.com" title="Google Fonts" subtitle="Free, professional web fonts for your portfolio" icon="document-text" >}}
{{< card link="https://coolors.co" title="Coolors" subtitle="Color palette generator -- create cohesive color schemes" icon="color-swatch" >}}
{{< card link="https://unsplash.com" title="Unsplash" subtitle="Free high-quality stock images" icon="photograph" >}}
{{< card link="https://webaim.org/resources/contrastchecker" title="WebAIM Contrast Checker" subtitle="Verify your text has sufficient contrast against backgrounds" icon="eye" >}}
{{< card link="https://validator.w3.org" title="W3C HTML Validator" subtitle="Check your HTML for errors before submission" icon="code" >}}
{{< /cards >}}

### Official Documentation

{{< cards >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Web/HTML" title="MDN: HTML Reference" subtitle="Complete HTML element and attribute documentation" icon="book-open" >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Web/CSS" title="MDN: CSS Reference" subtitle="Complete CSS property documentation" icon="book-open" >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Web/JavaScript" title="MDN: JavaScript Reference" subtitle="Complete JavaScript documentation" icon="book-open" >}}
{{< card link="https://css-tricks.com/snippets/css/a-guide-to-flexbox/" title="CSS-Tricks: Guide to Flexbox" subtitle="Visual Flexbox reference for layout" icon="document-text" >}}
{{< card link="https://css-tricks.com/snippets/css/complete-guide-grid/" title="CSS-Tricks: Guide to Grid" subtitle="Visual Grid reference for layout" icon="document-text" >}}
{{< /cards >}}

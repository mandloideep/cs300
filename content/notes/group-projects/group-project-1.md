---
title: "Group Project 1: Team Website Guide"
weight: 1
toc: true
---

## Introduction

This project asks your team of 3-4 to build a **multi-page, responsive website** using HTML, CSS, and JavaScript. It is the largest early-semester project in CS300, worth **175 points** and due **Sunday, March 22, 2026** at 11:59 PM.

Real web development is almost never a solo activity. Developers work on teams, divide responsibilities, review each other's code, and coordinate through version control. This project simulates that workflow from start to finish.

**What you will learn:**

- How to build and maintain HTML, CSS, and JavaScript across multiple pages at scale
- How to collaborate using Git branches, pull requests, and code review
- How to communicate effectively as a development team
- How to divide work, manage milestones, and deliver a polished product on deadline

This project spans approximately 6 weeks. You will need to plan, build, test, and present as a team. The skills you develop here -- both technical and collaborative -- carry directly into professional web development.

{{< callout type="info" >}}
This project is as much about learning to collaborate as it is about building a website. The skills you develop here -- Git workflow, code review, team communication -- are the same skills employers look for.
{{< /callout >}}

---

## Before You Start: Team Setup

{{% steps %}}

### Connect with Your Team

Exchange contact information with your team members. Set up a group chat using Discord, Slack, iMessage, or whatever platform everyone checks regularly. Make sure every member can be reached quickly.

### Schedule Your First Meeting

Meet in person or over Zoom within the first few days. Decide on a regular meeting time that works for everyone -- even 15-20 minutes twice a week makes a significant difference. Put it on your calendars.

### Choose Your Project Option

As a team, decide which website type to build. Your options are:

- **Option A: Business/Company Website** -- restaurant, salon, gym, agency, etc.
- **Option B: Event/Conference Website** -- schedule, speakers, registration
- **Option C: Non-Profit/Cause Website** -- mission, how to help, events
- **Option D: Educational Resource Website** -- lessons, resources, interactive elements
- **Option E: Portfolio/Agency Website** -- project gallery, services, team

Vote as a team. Pick the option that excites the most members -- enthusiasm leads to better work.

### Assign Roles

Every team member contributes to all areas (HTML, CSS, JavaScript), but each person takes the lead on one domain to ensure quality and consistency:

| Role                | Responsibility                                                     |
| ------------------- | ------------------------------------------------------------------ |
| **Project Lead**    | Coordinates tasks, tracks milestones, runs meetings                |
| **HTML Lead**       | Ensures semantic structure, valid HTML, consistent navigation      |
| **CSS Lead**        | Maintains the design system, responsive layout, visual consistency |
| **JavaScript Lead** | Manages interactive features, ensures functionality across pages   |

For 3-member teams, one person may hold two roles (Project Lead often doubles with another role).

### Set Up the GitHub Repository

One team member creates the repository and adds the others as collaborators:

1. Create a new **public** repository on GitHub (e.g., `cs300-group-project-1`)
2. Go to **Settings > Collaborators** and add each team member by their GitHub username
3. Each collaborator accepts the invitation via email or GitHub notifications

### Create a Project Plan

Use GitHub Issues, a shared Google Doc, or a simple checklist to track what needs to be built. Break the project into concrete tasks: "Build homepage HTML," "Create shared navigation," "Style the hero section," "Add form validation," etc.

{{% /steps %}}

---

## Understanding the Requirements

Here is what each rubric category is worth and what the instructor is looking for:

| Criteria            | Points | What This Means                                                                                 |
| ------------------- | ------ | ----------------------------------------------------------------------------------------------- |
| HTML Quality        | 30     | Semantic HTML5, valid markup, proper heading hierarchy across all 5+ pages                      |
| CSS Quality         | 35     | Responsive design, mobile-first approach, Flexbox and/or Grid layouts, consistent design system |
| JavaScript Features | 30     | At least 3 working interactive features (mobile menu, form validation, carousel, etc.)          |
| Design & UX         | 25     | Consistent look and feel, intuitive navigation, professional appearance                         |
| Team Collaboration  | 25     | Git history shows all members contributing, feature branches, pull requests, code review        |
| Code Quality        | 15     | Clean indentation, organized files, meaningful class and variable names                         |
| Documentation       | 15     | Complete README with project info, team members, live URL, screenshots, setup instructions      |

{{< callout type="warning" >}}
Team Collaboration is worth 25 points. The instructor reviews your Git history to see if all members are contributing. One person doing all the work results in point deductions for the entire team, plus peer evaluation adjustments of up to plus or minus 20%.
{{< /callout >}}

---

## Team Collaboration Guide

This is the most important section of this guide. Technical skills get the website built, but collaboration skills determine whether the team succeeds or falls apart.

### Communication Best Practices

- **Set up a dedicated channel** -- a Discord server or Slack workspace specifically for this project. Keep project discussion separate from personal chats
- **Agree on response time expectations** -- for example, everyone responds within 24 hours. Set this expectation in your first meeting
- **Use async communication for updates, sync (meetings) for decisions** -- post progress updates in chat anytime; save complex decisions for live meetings
- **Post regular standups** -- every day or every other day, each person posts a brief update: "What I finished, what I am working on next, any blockers"

{{< callout type="important" >}}
The number one reason teams struggle is poor communication. Over-communicate. If you are stuck, say something. If you are behind, say something. Silence is the enemy.
{{< /callout >}}

### GitHub Workflow for Teams

This is the workflow your team should follow for every piece of work. It prevents conflicts, enables code review, and creates a clear history of who built what.

{{% steps %}}

### Clone the Repository

Every team member clones the shared repository to their own computer:

```bash
git clone https://github.com/your-team/group-project-1.git
cd group-project-1
```

### Create a Feature Branch

Never work directly on `main`. Every task gets its own branch with a descriptive name:

```bash
git checkout -b feature/homepage
# or
git checkout -b feature/contact-form
# or
git checkout -b fix/mobile-nav
```

Use prefixes like `feature/` for new work and `fix/` for bug fixes. The name after the slash should describe what you are building.

### Work on Your Branch

Make regular commits with meaningful messages as you work. Do not wait until everything is done to commit:

```bash
git add index.html css/styles.css
git commit -m "Add hero section HTML structure and basic styling"

# Continue working...
git add css/styles.css
git commit -m "Add responsive navigation with Flexbox"

# Continue working...
git add js/main.js
git commit -m "Add mobile hamburger menu toggle"
```

### Push Your Branch and Create a Pull Request

When your feature is ready for review, push it to GitHub:

```bash
git push origin feature/homepage
```

Then go to your repository on GitHub. You will see a banner that says "Compare & pull request." Click it. Write a description of what you built, what it looks like, and anything reviewers should check.

### Request Review from a Teammate

In the pull request, click "Reviewers" on the right sidebar and select a team member. They will get a notification to review your code.

### After Approval, Merge to Main

Once your teammate approves the pull request, click "Merge pull request" on GitHub. Then click "Delete branch" to keep things clean.

### Pull the Latest Main Before Starting New Work

Before starting your next task, always get the latest version of `main`:

```bash
git checkout main
git pull origin main
git checkout -b feature/next-task
```

This ensures you are building on top of the team's latest work, not an outdated version.

{{% /steps %}}

{{< callout type="warning" >}}
Never push directly to main. Always use feature branches and pull requests. This prevents conflicts and ensures code review happens.
{{< /callout >}}

### Avoiding Merge Conflicts

Merge conflicts happen when two people edit the same lines in the same file. They are not catastrophic, but they are annoying. Here is how to minimize them:

- **Pull from main often** -- at least daily when you are actively working:

```bash
git checkout main
git pull origin main
git checkout your-branch
git merge main
```

- **Communicate about which files you are editing** -- post in your group chat: "I am working on `styles.css` today, specifically the hero section"
- **Do not work on the same file at the same time** -- coordinate who edits what
- **If a conflict happens, resolve it together on a call** -- screen share so both people understand what gets kept
- **Consider splitting CSS into separate files** -- for example, `nav.css`, `hero.css`, `footer.css` that are all linked from each HTML page. This reduces the chance of two people editing the same CSS file

### Code Review Etiquette

When reviewing a teammate's pull request:

- **Be constructive, not critical:** "Have you considered using Flexbox here? It might simplify this layout" -- not "This code is wrong"
- **Focus on the code, not the person:** "This section could use more spacing" -- not "You did not add enough spacing"
- **Suggest improvements, do not just point out problems:** If something could be better, explain how
- **Approve promptly** -- do not let pull requests sit for days. Aim to review within 24 hours
- **Leave comments on specific lines** -- GitHub lets you comment on individual lines of code in a PR, which is much clearer than general feedback

### Meeting Structure

Keep meetings short and focused. Use this simple standup format (5-10 minutes):

1. **Each person:** What did you finish since last time?
2. **Each person:** What are you working on next?
3. Any blockers or issues?
4. Quick decisions needed?

{{< callout type="info" >}}
Keep meetings short and focused. If a discussion goes long, schedule a separate meeting for that topic. Respect everyone's time.
{{< /callout >}}

### Handling Team Issues

| Issue                   | Solution                                                                                                           |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Member not responding   | Try all contact methods. Contact the instructor after 48 hours of silence                                          |
| Unequal contribution    | Discuss with the team first. Document the situation for your peer evaluation. Talk to the instructor if unresolved |
| Merge conflicts         | Pair up on a call and resolve together. Pull from main more frequently going forward                               |
| Disagreements on design | Vote as a team (majority wins). If deadlocked, ask the instructor to mediate                                       |
| Behind schedule         | Reprioritize immediately. Cut non-essential features. Focus on meeting core requirements first                     |

---

## Weekly Milestones

These milestones keep your team on track across the six-week project timeline. Falling behind by more than one week puts the entire project at risk.

{{% steps %}}

### Week 3: Planning and Setup

- Finalize your project option and team roles
- Create your sitemap (which pages exist and how they connect)
- Set up the GitHub repository with all collaborators
- Create the HTML skeleton for all 5+ pages (empty structure with shared navigation)
- Each member makes at least one commit to verify the workflow works

### Week 4: HTML Structure

- All 5+ pages contain real, meaningful content (not placeholder text)
- Shared navigation is identical across every page and links work correctly
- Semantic HTML elements used consistently (`<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`)
- Forms include appropriate input types and labels
- Validate HTML with the W3C Validator

### Week 5: CSS Layout and Design System

- Define your design system: color palette (3-5 colors), fonts (1-2), spacing scale
- Apply consistent styling across all pages
- Implement Flexbox and/or Grid for page layouts
- **Team Health Check:** each member should have multiple commits and at least one merged pull request by now

### Week 6: Responsive Design

- Implement mobile-first CSS with media queries
- Test at three widths: 375px (phone), 768px (tablet), 1024px (desktop)
- Ensure no horizontal scrolling at any width
- Navigation works on all screen sizes
- Images scale properly

### Week 7: JavaScript Features

- Implement at least 3 interactive features across the site
- Test each feature independently in the browser console
- Ensure JavaScript does not break on pages where a feature is not present
- Add hover effects and CSS transitions where appropriate

### Week 8: Polish and Submit

- Final cross-browser and cross-device testing
- Deploy to GitHub Pages (or Netlify/Vercel)
- Write the README with all required information
- Prepare and deliver your team presentation
- Submit to D2L with PR links
- Complete peer evaluations within 3 days of the deadline

{{% /steps %}}

{{< callout type="important" >}}
These milestones exist for a reason. If your team is behind by more than one week, talk to the instructor immediately. Early intervention is always better than last-minute panic.
{{< /callout >}}

---

## Technical Guidance by Area

### HTML Approach

- **Start with a sitemap:** draw which pages exist and how they connect. A simple sketch on paper or a shared whiteboard is enough
- **Create the shared navigation first** -- it must be identical on every page. Get this right early so every team member can copy the same `<nav>` element
- **Use semantic elements consistently** across all pages: `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>`
- **Maintain proper heading hierarchy:** one `<h1>` per page, followed by `<h2>`, `<h3>`, etc. in order
- **Validate with the W3C Validator** after each page is complete. Fix errors immediately -- they compound if ignored

### CSS Approach

{{< tabs items="Design System First (Recommended),Page-by-Page" >}}
{{< tab >}}
**Design System First** -- the recommended approach.

Start by defining your consistent values before styling any pages:

- **Color palette:** choose 3-5 colors (primary, secondary, accent, background, text)
- **Fonts:** select 1-2 fonts from Google Fonts (one for headings, one for body text, or one for everything)
- **Spacing scale:** use consistent values based on `rem` (e.g., 0.5rem, 1rem, 1.5rem, 2rem, 3rem)

Create CSS custom properties (variables) or a set of consistent class names that everyone on the team uses. Then apply these across all pages.

This approach ensures visual consistency from the start. When every page uses the same colors, fonts, and spacing, the site feels professional and cohesive.
{{< /tab >}}
{{< tab >}}
**Page-by-Page** -- faster to start, more refactoring later.

Style one page completely, then replicate the same visual patterns on other pages. This feels faster at first because you see immediate results.

The downside: each team member may style their pages slightly differently, leading to inconsistency. You will spend time refactoring later to make everything match.

If you choose this approach, designate one person (the CSS Lead) to do a consistency pass across all pages before submission.
{{< /tab >}}
{{< /tabs >}}

### JavaScript Feature Ideas

You need at least 3 interactive features across the site. Here are ideas to consider, described conceptually. The implementation is up to you and your team.

1. **Mobile hamburger menu** -- a button that shows and hides the navigation links on small screens. Essential for responsive sites
2. **Form validation** -- check that required fields are filled in, email addresses are formatted correctly, and show helpful error messages before the form submits
3. **Image carousel or slider** -- cycle through a series of images with previous and next buttons. Common on homepages and gallery pages
4. **Accordion or FAQ section** -- collapsible content sections where clicking a question reveals the answer. Keeps long content organized
5. **Content filter or search** -- filter a list of items (products, team members, resources) by category using buttons or a search field
6. **Modal or popup** -- an overlay with content that appears when the user clicks a button or link. Closes when clicking outside or pressing a close button
7. **Smooth scroll** -- clicking a navigation link scrolls smoothly to the corresponding section on the page instead of jumping instantly
8. **Scroll animations** -- elements fade in, slide in, or otherwise animate as the user scrolls down the page
9. **Theme switcher** -- a toggle that switches between light and dark mode, changing background and text colors across the entire site
10. **Back to top button** -- a button that appears when the user scrolls down and smoothly scrolls back to the top of the page when clicked

{{< callout type="info" >}}
You need at least 3 JavaScript features. Start with the easiest one (like smooth scroll or a back-to-top button) to build confidence, then tackle more complex ones like form validation or a carousel.
{{< /callout >}}

---

## Project Structure

{{< filetree/container >}}
{{< filetree/folder name="group-project-1" >}}
{{< filetree/file name="index.html" >}}
{{< filetree/file name="about.html" >}}
{{< filetree/file name="services.html" >}}
{{< filetree/file name="contact.html" >}}
{{< filetree/file name="gallery.html" >}}
{{< filetree/folder name="css" >}}
{{< filetree/file name="styles.css" >}}
{{< /filetree/folder >}}
{{< filetree/folder name="js" >}}
{{< filetree/file name="main.js" >}}
{{< /filetree/folder >}}
{{< filetree/folder name="images" >}}
{{< filetree/file name="(your images here)" >}}
{{< /filetree/folder >}}
{{< filetree/file name="README.md" >}}
{{< /filetree/folder >}}
{{< /filetree/container >}}

{{< callout type="info" >}}
Page names will vary depending on your chosen project option. The structure above is an example for a Business/Company website. Adjust the HTML file names to match your sitemap.
{{< /callout >}}

---

## Presentation Guide

Your team will present the project live to the instructor -- either during a scheduled meeting or during a Thursday class session before March 22. Every team member must participate.

{{% steps %}}

### Each Member Presents Their Own Contributions (3-5 minutes per person)

Talk about what you personally built. Which pages did you work on? Which features did you implement? Walk through the specific HTML, CSS, or JavaScript you wrote.

### Show Your Code and Explain Your Decisions

Open VS Code and walk through the code you are most proud of. Explain why you made the choices you made -- why that HTML structure, why that CSS technique, why that JavaScript approach.

### Demo the Live Site Together

As a team, walk through the deployed website. Show it working at different screen sizes. Demonstrate all JavaScript features. Click through all pages and show the navigation.

### Be Prepared for Questions About Your Code

The instructor may ask follow-up questions: "Why did you use Grid here instead of Flexbox?" or "How does this JavaScript feature work?" You must be able to explain any code you submitted.

### All Cameras On During the Presentation

Every team member must have their camera on for the entire presentation. This is a required part of the grading criteria.

{{% /steps %}}

---

## Peer Evaluation

After submission, each team member completes a confidential peer evaluation:

- **Due within 3 days** of the project deadline
- **Confidential** -- the instructor sees them, your teammates do not
- **Rate each team member** on contribution level, communication quality, and work quality (1-5 scale)
- **Describe each member's contributions** in your own words
- **Self-assessment** of your own contribution
- **Adjusts individual grades** by up to plus or minus 20%

{{< callout type="info" >}}
Be honest in peer evaluations. They ensure fair grading when contribution is unequal. If one team member carried the project, the peer evaluations are how the instructor finds out and adjusts grades accordingly.
{{< /callout >}}

---

## Deployment Guide

Your project must be deployed to a live URL. Choose one of these options:

{{< tabs items="GitHub Pages (Recommended),Netlify,Vercel" >}}
{{< tab >}}
**GitHub Pages** -- the simplest option for static HTML/CSS/JS sites.

1. Go to your repository on GitHub
2. Click **Settings** > **Pages** (in the left sidebar)
3. Under "Source," select **Deploy from a branch**
4. Choose the **main** branch and **/ (root)** folder
5. Click **Save**
6. Wait 1-2 minutes, then refresh the page. Your live URL will appear at the top

Your site will be available at `https://your-username.github.io/group-project-1/`. Every time you merge to `main`, the site updates automatically.

Make sure your main page is named `index.html` -- GitHub Pages looks for this file by default.
{{< /tab >}}
{{< tab >}}
**Netlify** -- offers automatic deployments and custom domains.

1. Create a free account at netlify.com
2. Click **Add new site** > **Import an existing project**
3. Connect your GitHub account and select your repository
4. Configure the build settings (for a static site, leave them at defaults)
5. Click **Deploy site**

Netlify will give you a random URL like `random-name.netlify.app`. You can customize the subdomain in your site settings. The site redeploys automatically when you push to `main`.
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

---

## Submission Checklist

Before submitting, verify every item:

- [ ] 5+ HTML pages with semantic elements (`<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`)
- [ ] Consistent, working navigation across all pages
- [ ] External CSS stylesheet(s), fully responsive (mobile-first with `min-width` media queries)
- [ ] Flexbox and/or CSS Grid used for layouts
- [ ] Consistent design system (colors, fonts, spacing applied across all pages)
- [ ] 3+ JavaScript interactive features working correctly
- [ ] No CSS frameworks (no Bootstrap, Tailwind, etc.)
- [ ] All team members have visible Git contributions (commits and pull requests)
- [ ] Feature branches and pull requests used (not direct pushes to main)
- [ ] Deployed to GitHub Pages, Netlify, or Vercel with a working live URL
- [ ] README includes: project name, team members and roles, live URL, screenshots, tech stack, setup instructions, feature list
- [ ] HTML passes the W3C Validator with no errors
- [ ] Each member has PR links ready for their individual D2L submission
- [ ] Presentation completed (all members participated, cameras on)
- [ ] Peer evaluations submitted within 3 days of the deadline

---

## Troubleshooting & Common Mistakes

{{% details title="We are having constant merge conflicts" closed="true" %}}
Pull from `main` more often -- ideally every time before you start working:

```bash
git checkout main
git pull origin main
git checkout your-branch
git merge main
```

Communicate in your group chat about which files each person is editing. If two people need to edit `styles.css`, consider splitting it into page-specific CSS files (e.g., `homepage.css`, `about.css`, `contact.css`) and linking each from the relevant HTML page.
{{% /details %}}

{{% details title="One team member is not contributing" closed="true" %}}
Talk to them directly first -- they may be struggling or dealing with something outside of class. If there is no response after 48 hours, contact the instructor. Document the situation for your peer evaluation. Do not wait until the last week to raise this issue.
{{% /details %}}

{{% details title="Our design looks inconsistent across pages" closed="true" %}}
Create a design system first: define your colors, fonts, and spacing values in one place (CSS custom properties work well for this). Use the same CSS class names across all pages. Have the CSS Lead do a consistency pass across every page before submission.
{{% /details %}}

{{% details title="The navigation does not work on all pages" closed="true" %}}
Check that file paths in your links are correct. If `index.html` links to `about.html`, make sure `about.html` exists at the same directory level. Use relative paths (e.g., `href="about.html"` not `href="/about.html"`). Test every link on every page.
{{% /details %}}

{{% details title="Our JavaScript features are buggy" closed="true" %}}
Test each feature independently. Use `console.log()` statements to track what your code is doing. Open the browser console (F12 > Console tab) and look for error messages -- they tell you exactly which line has the problem. If a feature works on one page but not another, check that the JavaScript file is linked on all pages and that the HTML elements the script targets actually exist on that page.
{{% /details %}}

{{% details title="We are behind on the milestone schedule" closed="true" %}}
Reprioritize immediately. Identify the core requirements (5 pages, responsive CSS, 3 JS features) and focus only on those. Cut nice-to-have features like animations or extra pages. Talk to the instructor -- they can help you prioritize and may offer guidance on where to focus.
{{% /details %}}

{{% details title="Our GitHub Pages deployment shows a 404" closed="true" %}}
Check these common causes:

1. Your main page must be named `index.html` (not `home.html` or `main.html`)
2. Verify that GitHub Pages is configured to deploy from the correct branch (usually `main`) in Settings > Pages
3. Make sure your latest code is actually merged to `main` -- check the `main` branch on GitHub to confirm
4. Wait 1-2 minutes after pushing; deployments are not instant
5. Clear your browser cache and try again (Ctrl+Shift+R or Cmd+Shift+R)
   {{% /details %}}

{{% details title="Git says 'fatal: not a git repository'" closed="true" %}}
You are in the wrong directory. Make sure you are inside the cloned project folder:

```bash
cd group-project-1
git status
```

If you never cloned the repository, start with `git clone` using the repository URL from GitHub.
{{% /details %}}

{{% details title="I accidentally committed to main instead of a branch" closed="true" %}}
If you have not pushed yet, you can move your commit to a new branch:

```bash
git branch feature/my-work
git reset --soft HEAD~1
git checkout feature/my-work
git add .
git commit -m "Your commit message"
```

If you already pushed to main, talk to your team. The Project Lead can help sort it out. Going forward, always check which branch you are on with `git status` before committing.
{{% /details %}}

---

## Key Takeaways

{{< callout emoji="🎯" >}}

- **Communication is the most important team skill** -- over-communicate, never go silent, post regular updates
- **Always use feature branches and pull requests** -- never push directly to main
- **Pull from main frequently** to avoid merge conflicts: `git pull origin main`
- **Start with a design system** (colors, fonts, spacing) so every page looks consistent
- **Build HTML structure first, CSS second, JavaScript last** -- this order reduces rework
- **Test at 375px, 768px, and 1024px** for responsive design at all screen sizes
- **All team members must have visible Git contributions** -- commits, branches, and pull requests
- **Peer evaluations adjust individual grades** by up to plus or minus 20% -- be honest and fair
- **Talk to the instructor early** if your team is struggling -- do not wait until the deadline
  {{< /callout >}}

---

## Additional Resources

### Git & GitHub

{{< cards >}}
{{< card link="https://www.youtube.com/watch?v=SWYqp7iY_Tc" title="Traversy Media: Git & GitHub Crash Course" subtitle="Complete beginner guide to Git and GitHub" icon="play" >}}
{{< card link="https://www.youtube.com/playlist?list=PL4cUxeGkcC9goXbgTDQ0n_4TBzOO0ocPR" title="The Net Ninja: Git & GitHub Tutorial" subtitle="Step-by-step playlist covering branches, merging, and PRs" icon="play" >}}
{{< card link="https://docs.github.com/en/pull-requests" title="GitHub: Pull Requests Documentation" subtitle="Official guide to creating and reviewing pull requests" icon="book-open" >}}
{{< card link="https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow" title="Atlassian: Git Feature Branch Workflow" subtitle="Industry-standard branching strategy used by development teams" icon="document-text" >}}
{{< /cards >}}

### Web Development

{{< cards >}}
{{< card link="https://developer.mozilla.org/en-US/docs/Web/HTML" title="MDN: HTML Reference" subtitle="Complete HTML element and attribute documentation" icon="book-open" >}}
{{< card link="https://css-tricks.com/snippets/css/a-guide-to-flexbox/" title="CSS-Tricks: Complete Guide to Flexbox" subtitle="Visual reference for all Flexbox properties" icon="document-text" >}}
{{< card link="https://css-tricks.com/snippets/css/complete-guide-grid/" title="CSS-Tricks: Complete Guide to Grid" subtitle="Visual reference for all CSS Grid properties" icon="document-text" >}}
{{< card link="https://validator.w3.org/" title="W3C HTML Validator" subtitle="Check your HTML for errors and fix them before submission" icon="code" >}}
{{< /cards >}}

### Team Collaboration

{{< cards >}}
{{< card link="https://discord.com/" title="Discord" subtitle="Free team communication platform with channels and voice chat" icon="link" >}}
{{< card link="https://docs.github.com/en/issues" title="GitHub Issues" subtitle="Track tasks, bugs, and feature requests for your project" icon="document-text" >}}
{{< /cards >}}

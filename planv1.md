# Plan v1 — cadeom Landing Page Implementation Roadmap

**Created:** 2025-07-05  
**Current Status:** Website live at https://www.cadeom.com  
**Last Updated:** 2025-07-05

---

## Overview

The cadeom landing page is now live with core functionality. This plan outlines remaining tasks to make it production-ready, improve user experience, and enable lead capture.

---

## Phase 1: Critical — Lead Capture (Week 1)

These items must be completed for the site to actually capture enquiries.

### 1.1 Wire Contact Form to Backend
- **Status:** ⚠️ Not Started
- **Priority:** CRITICAL
- **Description:** The form currently shows a success message but doesn't send data anywhere
- **Options:**
  - **Zapier (Recommended):** No-code, free tier available
    - Create a Zap: Webhook → Email / Google Sheets / CRM
    - Replace form submit handler in index.html with Zapier webhook URL
    - Cost: Free (up to 100 tasks/month)
  - **Formspree:** Simple form-to-email
    - Sign up at formspree.io
    - Get form endpoint URL
    - Update form action in index.html
    - Cost: Free (up to 50/month)
  - **SendGrid/Mailgun API:** For advanced integration
    - Requires backend or serverless function
    - Cost: Starts free, scales with usage
  - **Custom backend:** Build API endpoint to capture form data
    - More control but requires development
- **Effort:** 15–30 minutes (Zapier/Formspree) or 2–4 hours (custom backend)
- **Owner:** Jerome
- **Acceptance Criteria:**
  - Form submission sends data to backend
  - Data is stored (email, CRM, or spreadsheet)
  - Confirmation email sent to user
  - No data is lost

---

### 1.2 Test Form End-to-End
- **Status:** ⚠️ Not Started
- **Priority:** HIGH
- **Description:** Validate form submission flow before declaring complete
- **Steps:**
  1. Fill out form with test data
  2. Submit
  3. Verify data appears in backend (email/CRM/sheet)
  4. Check for any errors in browser console
  5. Verify user gets success message
- **Effort:** 10 minutes
- **Owner:** Jerome
- **Acceptance Criteria:**
  - Test submission captured successfully
  - No console errors
  - User confirmation received

---

## Phase 2: Analytics & Monitoring (Week 1)

Track traffic, errors, and uptime to understand how the site performs.

### 2.1 Set Up Google Analytics 4
- **Status:** ⚠️ Not Started
- **Priority:** HIGH
- **Description:** Understand visitor behavior, traffic sources, and engagement
- **Steps:**
  1. Create Google Analytics 4 property at analytics.google.com
  2. Get tracking ID (starts with "G-")
  3. Add tracking code to index.html `<head>`:
     ```html
     <!-- Google Analytics -->
     <script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXX"></script>
     <script>
       window.dataLayer = window.dataLayer || [];
       function gtag(){dataLayer.push(arguments);}
       gtag('js', new Date());
       gtag('config', 'G-XXXXXXXX');
     </script>
     ```
  4. Test: Visit site, check Real-time reports in GA4
  5. Deploy
- **Effort:** 15 minutes
- **Owner:** Jerome
- **Acceptance Criteria:**
  - Tracking ID configured
  - Real-time visitors show up in GA4
  - Page views are tracked
  - Form submissions tracked as events (optional but recommended)

---

### 2.2 Set Up Uptime Monitoring
- **Status:** ⚠️ Not Started
- **Priority:** HIGH
- **Description:** Get alerted if www.cadeom.com goes down
- **Options:**
  - **Updown.io** (free tier: 1 check/5min)
  - **StatusCake** (free tier available)
  - **Freshping** (free tier available)
- **Steps:**
  1. Sign up at chosen service
  2. Add monitoring check for https://www.cadeom.com
  3. Configure email alert
  4. Test: Should receive alert within 5 minutes if down
- **Effort:** 10 minutes
- **Owner:** Jerome
- **Acceptance Criteria:**
  - Service confirms site is up
  - Alert email configured
  - Test alert received

---

### 2.3 Set Up Error Tracking (Sentry)
- **Status:** ⚠️ Not Started
- **Priority:** MEDIUM
- **Description:** Catch and track JavaScript errors in production
- **Steps:**
  1. Sign up at sentry.io (free tier)
  2. Create project for cadeom-website
  3. Get DSN (Data Source Name)
  4. Add Sentry script to index.html before other scripts:
     ```html
     <script src="https://browser.sentry-cdn.com/7.x.x/bundle.min.js"></script>
     <script>
       Sentry.init({ dsn: "YOUR_DSN" });
     </script>
     ```
  5. Deploy and test
- **Effort:** 10 minutes
- **Owner:** Jerome
- **Acceptance Criteria:**
  - Sentry project created
  - DSN configured in site
  - Test error captured (open browser console, trigger error)

---

## Phase 3: Testing & Validation (Week 1)

Ensure the site works across all devices and browsers.

### 3.1 Cross-Browser Testing
- **Status:** ⚠️ Not Started
- **Priority:** HIGH
- **Description:** Test on all major browsers
- **Browsers to test:**
  - ✅ Safari (already tested, Safari cache issue resolved)
  - ⚠️ Chrome
  - ⚠️ Firefox
  - ⚠️ Edge
- **Test steps for each:**
  1. Visit https://www.cadeom.com
  2. Navigate through all 4 screens using:
     - Dots (bottom)
     - Keyboard arrows
     - Mouse wheel/trackpad
     - (Mobile) Swipe left/right
  3. Verify animations play smoothly
  4. Test form submission
  5. Check no console errors
- **Effort:** 30 minutes
- **Owner:** Jerome
- **Acceptance Criteria:**
  - All browsers render correctly
  - No visual glitches
  - No console errors
  - Animations smooth on all browsers

---

### 3.2 Mobile Device Testing
- **Status:** ⚠️ Not Started
- **Priority:** HIGH
- **Description:** Test on real mobile devices (not just browser emulation)
- **Devices to test:**
  - iPhone (Safari)
  - Android phone (Chrome)
  - Tablet (iPad/Android tablet)
- **Test steps:**
  1. Visit https://www.cadeom.com on device
  2. Test touch navigation (swipe between screens)
  3. Verify form fields are usable (keyboard appears, sizing okay)
  4. Check text is readable (no overflow, good font size)
  5. Verify layout switches to single-column below 760px
  6. Test that dots and nav are usable on small screen
- **Effort:** 20 minutes
- **Owner:** Jerome
- **Acceptance Criteria:**
  - Layout responsive on all screen sizes
  - Touch navigation works smoothly
  - Form is usable on mobile
  - No layout broken on any device

---

### 3.3 Accessibility Audit
- **Status:** ⚠️ Not Started
- **Priority:** MEDIUM
- **Description:** Ensure site is usable by people with disabilities
- **Manual checks:**
  1. Keyboard navigation: Can you tab through form inputs and buttons?
  2. Color contrast: Is text readable against backgrounds?
  3. ARIA labels: Do buttons/images have descriptive labels?
  4. Focus visible: Can you see which element has focus (keyboard users)?
- **Automated check:**
  - Install WAVE browser extension or use WebAIM axe
  - Run scan on each screen
  - Fix any high-priority issues
- **Effort:** 30 minutes
- **Owner:** Jerome
- **Acceptance Criteria:**
  - No high-priority accessibility issues
  - Keyboard navigation works
  - Color contrast meets WCAG AA standard

---

### 3.4 Performance Audit
- **Status:** ⚠️ Not Started
- **Priority:** MEDIUM
- **Description:** Ensure site loads fast
- **Steps:**
  1. Visit https://pagespeed.web.dev
  2. Enter www.cadeom.com
  3. Check Mobile and Desktop scores
  4. Note any red/yellow issues
  5. Fix if score is below 80
- **Effort:** 15 minutes
- **Owner:** Jerome
- **Acceptance Criteria:**
  - Mobile score ≥ 80
  - Desktop score ≥ 90
  - No red (critical) issues

---

## Phase 4: Documentation & Maintenance (Week 2)

Ensure future maintainers understand the project.

### 4.1 Create Developer Onboarding Guide
- **Status:** ⚠️ Not Started
- **Priority:** MEDIUM
- **Description:** Document how to work with this project
- **File:** Create `DEVELOPER.md` with:
  - How to make changes
  - How to test locally
  - How to deploy
  - Common issues and fixes
  - Where form data goes
  - Where to check analytics
- **Effort:** 30 minutes
- **Owner:** Jerome
- **Acceptance Criteria:**
  - File created and committed
  - Instructions are clear enough for a new developer to follow

---

### 4.2 Document Form Backend Integration
- **Status:** ⚠️ Not Started
- **Priority:** HIGH
- **Description:** Document exactly how form data flows (critical for maintenance)
- **Include in DEVELOPER.md:**
  - Which form submission service is used (Zapier/Formspree/etc.)
  - How to access submitted data
  - How to update the endpoint if needed
  - How to test form locally
  - Troubleshooting steps
- **Effort:** 15 minutes
- **Owner:** Jerome
- **Acceptance Criteria:**
  - Documentation covers all aspects
  - Clear enough for non-technical person to understand flow

---

### 4.3 Set Up Monthly Maintenance Checklist
- **Status:** ⚠️ Not Started
- **Priority:** LOW
- **Description:** Create a checklist to run monthly
- **File:** Create `MAINTENANCE.md` with tasks like:
  - Check uptime monitoring status
  - Review Google Analytics for issues
  - Check for any Sentry errors
  - Test form submission
  - Verify all 4 screens load correctly
  - Check mobile responsiveness
  - Review performance score
  - Backup current version
- **Effort:** 15 minutes
- **Owner:** Jerome
- **Acceptance Criteria:**
  - Checklist created
  - Scheduled (e.g., first of each month)

---

## Phase 5: Future Enhancements (Future)

Nice-to-have improvements that can be done after v1 is stable.

### 5.1 Add Blog/Resources Section
- **Priority:** LOW
- **Description:** Add a 5th screen with blog posts or case studies
- **Effort:** 4–6 hours
- **Blocked by:** Nothing (can start anytime)
- **Owner:** TBD

### 5.2 Add Video (Hero Background)
- **Priority:** LOW
- **Description:** Replace hero background gradient with looping video
- **Effort:** 2–3 hours (if video already exists)
- **Blocked by:** Video asset creation
- **Owner:** TBD

### 5.3 A/B Testing
- **Priority:** LOW
- **Description:** Test different CTAs, copy, or layouts to improve conversion
- **Options:** Google Optimize, Convert.com, Optimizely
- **Effort:** Depends on test complexity
- **Owner:** TBD

### 5.4 Newsletter Signup
- **Priority:** LOW
- **Description:** Add email capture (separate from contact form)
- **Effort:** 1–2 hours
- **Blocked by:** Email service (Mailchimp, ConvertKit, etc.)
- **Owner:** TBD

### 5.5 CRM Integration
- **Priority:** LOW
- **Description:** Automatically create leads in HubSpot, Salesforce, etc.
- **Effort:** 2–4 hours depending on CRM
- **Blocked by:** CRM selection and API documentation
- **Owner:** TBD

---

## Summary

| Phase | Title | Est. Effort | Priority | Owner |
|-------|-------|------------|----------|-------|
| 1.1 | Wire form to backend | 15–30 min | **CRITICAL** | Jerome |
| 1.2 | Test form end-to-end | 10 min | HIGH | Jerome |
| 2.1 | Google Analytics | 15 min | HIGH | Jerome |
| 2.2 | Uptime monitoring | 10 min | HIGH | Jerome |
| 2.3 | Error tracking (Sentry) | 10 min | MEDIUM | Jerome |
| 3.1 | Cross-browser testing | 30 min | HIGH | Jerome |
| 3.2 | Mobile device testing | 20 min | HIGH | Jerome |
| 3.3 | Accessibility audit | 30 min | MEDIUM | Jerome |
| 3.4 | Performance audit | 15 min | MEDIUM | Jerome |
| 4.1 | Developer guide | 30 min | MEDIUM | Jerome |
| 4.2 | Form documentation | 15 min | HIGH | Jerome |
| 4.3 | Maintenance checklist | 15 min | LOW | Jerome |
| **Total Phase 1–4** | | **~3.5 hours** | — | — |

---

## Getting Started

**Week 1 (This Week):**
1. ✅ Phase 1: Wire form, test end-to-end (1–2 hours)
2. ✅ Phase 2: Analytics & monitoring (45 min)
3. ✅ Phase 3: Testing (2 hours)
4. ✅ Phase 4: Documentation (1 hour)

**By end of week:** Site should be fully production-ready with lead capture working.

---

## Notes

- Dates and effort estimates are approximations
- Some tasks may take less time if you're familiar with the tools
- If you get stuck on any task, refer to the service's documentation or tutorials
- Update this plan as priorities change
- Mark items `✅ Done` when completed

---

**Questions? Need clarification on any tasks?**

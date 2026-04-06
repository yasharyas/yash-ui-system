# Reusable UI Component Registry
> Extracted from: Mahin Kumar Portfolio
> Stack: Vanilla HTML/CSS + GSAP + Lenis — JSX versions provided for portability
> Font: Space Grotesk (Google Fonts), Color scheme: black bg / white text

---

=== COMPONENT START ===

Name: CustomCursor

Code:
```jsx
import { useEffect, useRef } from 'react';
import gsap from 'gsap';

export default function CustomCursor() {
  const cursorRef = useRef(null);

  useEffect(() => {
    const cursor = cursorRef.current;
    if (!cursor || window.innerWidth < 768) return;

    cursor.classList.add('active');
    let isHovering = false;

    const onMove = (e) => {
      gsap.to(cursor, {
        x: e.clientX,
        y: e.clientY,
        duration: 0.5,
        ease: 'power3.out',
      });
    };

    const onDown = () =>
      gsap.to(cursor, { scale: isHovering ? 1.8 : 0.5, duration: 0.15, ease: 'power2.out' });

    const onUp = () =>
      gsap.to(cursor, { scale: isHovering ? 2.5 : 1, duration: 0.25, ease: 'power2.out' });

    document.addEventListener('mousemove', onMove);
    document.addEventListener('mousedown', onDown);
    document.addEventListener('mouseup', onUp);

    const interactives = document.querySelectorAll('a, button, [data-cursor-grow]');
    interactives.forEach((el) => {
      el.addEventListener('mouseenter', () => { isHovering = true; gsap.to(cursor, { scale: 2.5, duration: 0.3 }); });
      el.addEventListener('mouseleave', () => { isHovering = false; gsap.to(cursor, { scale: 1, duration: 0.3 }); });
    });

    return () => {
      document.removeEventListener('mousemove', onMove);
      document.removeEventListener('mousedown', onDown);
      document.removeEventListener('mouseup', onUp);
    };
  }, []);

  return (
    <div className="cursor" ref={cursorRef}>
      <div className="cursor_top" />
      <div className="cursor_bottom" />
      <div className="cursor_left" />
      <div className="cursor_right" />
    </div>
  );
}
```

Props:
```ts
// No props — self-contained cursor replacement.
// Add data-cursor-grow attribute to any element to trigger grow effect.
type Props = {};
```

Dependencies:
- gsap (npm: gsap)
- React 18+ (useEffect, useRef)

Styles:
```css
.cursor {
  width: 1.7rem;
  height: 1.7rem;
  position: fixed;
  top: 0; left: 0;
  pointer-events: none;
  mix-blend-mode: difference;
  z-index: 100000;
  opacity: 0;
  transition: opacity 0.3s;
}
.cursor.active { opacity: 1; }
.cursor_left, .cursor_right { position: absolute; background-color: #fff; width: 0.5rem; height: 0.2rem; top: 50%; transform: translateY(-50%); }
.cursor_top, .cursor_bottom { position: absolute; background-color: #fff; height: 0.5rem; width: 0.2rem; left: 50%; transform: translateX(-50%); }
.cursor_right { right: 0; }
.cursor_left { left: 0; }
.cursor_top { top: 0; }
.cursor_bottom { bottom: 0; }
/* Hide on mobile */
@media (max-width: 768px) { .cursor { display: none; } body { cursor: auto; } }
```

Usage:
```jsx
// Place once at the root of your app (e.g. _app.tsx or layout.tsx)
<CustomCursor />
```

Prompt:
> "Create a minimal cross-hair custom cursor using CSS that replaces the browser cursor. The cursor has four short white bars (top, bottom, left, right) forming a plus/crosshair. It follows the mouse smoothly with GSAP lag. On hover over interactive elements it scales up to 2.5x. Uses mix-blend-mode: difference so it inverts color over any background."

Registry JSON:
```json
{
  "name": "CustomCursor",
  "category": "interaction",
  "tags": ["cursor", "gsap", "crosshair", "mix-blend-mode", "interactive"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

Name: Preloader

Code:
```jsx
import { useEffect, useRef } from 'react';
import gsap from 'gsap';

type Props = {
  name?: string;        // Text to display — defaults to "LOADING"
  onComplete: () => void;
};

export default function Preloader({ name = 'LOADING', onComplete }: Props) {
  const rootRef = useRef<HTMLDivElement>(null);
  const counterRef = useRef<HTMLSpanElement>(null);

  useEffect(() => {
    const preloader = rootRef.current;
    if (!preloader) return;

    const chars = preloader.querySelectorAll<HTMLElement>('.pl_char');
    const counterEl = counterRef.current;

    // Identify "keep" indices (first + middle prominent char) — generalised: keep index 0
    const hideChars = Array.from(chars).filter((_, i) => i !== 0);

    const tl = gsap.timeline({
      delay: 0.3,
      onComplete: () => {
        gsap.to(hideChars, { opacity: 0, duration: 0.35, ease: 'power2.inOut' });
        gsap.to(preloader.querySelector('.pl_counter'), { opacity: 0, duration: 0.25, ease: 'power2.inOut' });
        gsap.to(preloader, {
          yPercent: -100,
          duration: 0.8,
          delay: 0.65,
          ease: 'power3.inOut',
          onComplete: () => {
            if (preloader) preloader.style.display = 'none';
            onComplete();
          },
        });
      },
    });

    const count = { val: 0 };
    tl.to(count, {
      val: 100,
      duration: 2,
      ease: 'power2.inOut',
      onUpdate: () => { if (counterEl) counterEl.textContent = Math.round(count.val) + '%'; },
    }, 0);

    tl.to(chars, { y: 0, duration: 1, stagger: 0.035, ease: 'power3.out' }, 0.2);
  }, [onComplete]);

  const letters = name.split('').map((ch, i) => (
    <span key={i} className="pl_char" style={{ display: 'inline-block', transform: 'translateY(110%)' }}>
      {ch === ' ' ? '\u00A0' : ch}
    </span>
  ));

  return (
    <div className="pl_root" ref={rootRef}>
      <div className="pl_name">{letters}</div>
      <div className="pl_counter"><span ref={counterRef}>0%</span></div>
    </div>
  );
}
```

Props:
```ts
type Props = {
  name?: string;       // Display name/text — defaults to "LOADING"
  onComplete: () => void; // Called when animation finishes
};
```

Dependencies:
- gsap (npm: gsap)
- React 18+

Styles:
```css
.pl_root {
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  z-index: 10000;
  background-color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  pointer-events: none;
}
.pl_name {
  display: flex;
  color: #000;
  font-size: clamp(3rem, 8vw, 12rem);
  letter-spacing: -0.5rem;
  font-weight: 500;
  overflow: hidden;
}
.pl_char { display: inline-block; transform: translateY(110%); }
.pl_counter {
  position: fixed;
  right: 5rem; bottom: 5rem;
  font-size: clamp(4rem, 6vw, 10rem);
  color: #000;
  font-weight: 500;
  overflow: hidden;
}
.pl_counter span { display: inline-block; }
```

Usage:
```jsx
const [loaded, setLoaded] = useState(false);

{!loaded && <Preloader name="MAHIN KUMAR." onComplete={() => setLoaded(true)} />}
```

Prompt:
> "Create a fullscreen white preloader that animates each letter of a name into view one by one sliding up from below. A percentage counter in the bottom-right corner counts from 0% to 100%. When complete, non-key letters fade out, then the whole panel slides upward off screen, revealing the page underneath."

Registry JSON:
```json
{
  "name": "Preloader",
  "category": "loaders",
  "tags": ["preloader", "gsap", "letter-reveal", "counter", "entrance"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

Name: SiteHeader

Code:
```jsx
type NavLink = { label: string; href: string };

type Props = {
  logo?: string;
  logoHref?: string;
  navLinks?: NavLink[];
  ctaLabel?: string;
  ctaHref?: string;
  visible?: boolean;
};

export default function SiteHeader({
  logo = 'BRAND.',
  logoHref = '/',
  navLinks = [],
  ctaLabel = 'GET IN TOUCH',
  ctaHref = '#contact',
  visible = true,
}: Props) {
  return (
    <header className={`site_header ${visible ? 'visible' : ''}`}>
      <h1 className="site_header_logo">
        <a href={logoHref}>{logo}</a>
      </h1>
      <nav>
        <ul className="site_header_nav">
          {navLinks.map((link) => (
            <li key={link.href} className="site_header_nav_item">
              <div className="cta_hover" />
              <a className="nav_link" href={link.href}>
                <span className="link_outer">{link.label}</span>
                <span className="link_inner">{link.label}</span>
              </a>
            </li>
          ))}
        </ul>
      </nav>
      <div className="site_header_cta">
        <div className="cta_hover" />
        <a className="nav_link" href={ctaHref}>
          <span className="link_outer">{ctaLabel}</span>
          <span className="link_inner">{ctaLabel}</span>
        </a>
      </div>
    </header>
  );
}
```

Props:
```ts
type NavLink = { label: string; href: string };

type Props = {
  logo?: string;        // Logo text — default "BRAND."
  logoHref?: string;    // Logo link target — default "/"
  navLinks?: NavLink[]; // Navigation items array
  ctaLabel?: string;    // CTA button label — default "GET IN TOUCH"
  ctaHref?: string;     // CTA link target — default "#contact"
  visible?: boolean;    // Controls slide-down visibility — default true
};
```

Dependencies:
- React 18+
- No external packages

Styles:
```css
.site_header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  height: 9rem;
  padding: 0 5rem;
  padding-top: 2.5rem;
  position: fixed;
  top: 0; left: 0;
  z-index: 1000;
  mix-blend-mode: difference;
  transform: translateY(-100%);
  transition: transform 0.8s cubic-bezier(0.25, 1, 0.5, 1);
}
.site_header.visible { transform: translateY(0); }

.site_header_logo { font-size: 4rem; letter-spacing: -0.15rem; font-weight: 500; color: #fff; }
.site_header_logo a { text-decoration: none; color: inherit; }

.site_header_nav { display: flex; list-style: none; margin: 0; padding: 0; }
.site_header_nav_item {
  font-size: 1.4rem;
  border-radius: 5rem;
  padding: 0.6rem 1.6rem 0.5rem;
  position: relative;
  overflow: hidden;
  -webkit-mask-image: -webkit-radial-gradient(white, black);
}
.site_header_cta {
  font-size: 1.4rem;
  border: 1px solid rgba(255,255,255,0.3);
  border-radius: 4rem;
  padding: 0.6rem 1.6rem 0.5rem;
  position: relative;
  overflow: hidden;
  -webkit-mask-image: -webkit-radial-gradient(white, black);
  color: #fff;
}

/* Hover fill bubble */
.cta_hover {
  width: 150%; height: 150%;
  background-color: #fff;
  border-radius: 50%;
  position: absolute;
  left: -25%; top: -25%;
  transform: translateY(101%);
  transform-origin: top;
  transition: transform 0.5s cubic-bezier(0.25, 1, 0.5, 1);
  pointer-events: none;
}
.site_header_nav_item:hover .cta_hover,
.site_header_cta:hover .cta_hover { transform: translateY(0); }

/* Dual-span slide nav link */
.nav_link {
  position: relative;
  overflow: hidden;
  display: inline-block;
  color: #fff;
  text-decoration: none;
  text-transform: uppercase;
  letter-spacing: -0.05em;
}
.nav_link span { display: inline-block; transition: transform 0.5s cubic-bezier(0.25, 1, 0.5, 1); }
.nav_link .link_inner {
  position: absolute;
  left: 0; top: 0;
  transform: translateY(100%);
  color: #000;
}
.nav_link:hover .link_outer { transform: translateY(-100%); }
.nav_link:hover .link_inner { transform: translateY(0); }
```

Usage:
```jsx
<SiteHeader
  logo="ACME."
  logoHref="/"
  navLinks={[
    { label: 'WORK', href: '/work' },
    { label: 'ABOUT', href: '/about' },
  ]}
  ctaLabel="HIRE ME"
  ctaHref="mailto:hello@acme.com"
  visible={true}
/>
```

Prompt:
> "Design a fixed top navigation bar for a dark portfolio site. Logo text on the left, navigation links in the center, and a contact CTA button on the right. Each nav link has a dual-text slide-up effect on hover. The CTA button has an expanding white circle bubble that fills from below on hover flipping text to black. The header slides down from translateY(-100%) and uses mix-blend-mode difference for contrast over any content."

Registry JSON:
```json
{
  "name": "SiteHeader",
  "category": "navigation",
  "tags": ["header", "navbar", "hover-fill", "slide-reveal", "mix-blend-mode", "fixed"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

Name: MobileMenu

Code:
```jsx
import { useState } from 'react';

type NavLink = { label: string; href: string };

type Props = {
  logo?: string;
  logoHref?: string;
  links?: NavLink[];
};

export default function MobileMenu({
  logo = 'MK.',
  logoHref = '/',
  links = [],
}: Props) {
  const [open, setOpen] = useState(false);

  const toggle = () => setOpen((v) => !v);
  const close = () => setOpen(false);

  return (
    <>
      <header className="mobile_nav">
        <h1 className="mobile_nav_logo"><a href={logoHref}>{logo}</a></h1>
        <button
          className={`burger_btn ${open ? 'open' : ''}`}
          aria-label="Toggle menu"
          onClick={toggle}
        >
          <span />
          <span />
          <span />
        </button>
      </header>

      <div className={`mobile_overlay ${open ? 'open' : ''}`}>
        <nav>
          <ul>
            {links.map((link) => (
              <li key={link.href}>
                <a href={link.href} onClick={close}>{link.label}</a>
              </li>
            ))}
          </ul>
        </nav>
      </div>
    </>
  );
}
```

Props:
```ts
type NavLink = { label: string; href: string };

type Props = {
  logo?: string;       // Short logo text — default "MK."
  logoHref?: string;   // Logo link target — default "/"
  links?: NavLink[];   // Menu links array
};
```

Dependencies:
- React 18+

Styles:
```css
.mobile_nav {
  display: flex;
  width: 100%;
  height: 6rem;
  justify-content: space-between;
  align-items: center;
  padding: 0 2rem;
  position: fixed;
  top: 0; left: 0;
  z-index: 1000000;
  mix-blend-mode: difference;
}
.mobile_nav_logo { font-size: 4rem; font-weight: 500; color: #fff; }
.mobile_nav_logo a { text-decoration: none; color: inherit; }

.burger_btn {
  width: 2.4rem; height: 1.8rem;
  display: flex; flex-direction: column;
  justify-content: space-between;
  background: none; border: none;
  cursor: pointer; padding: 0;
}
.burger_btn span {
  width: 100%; height: 0.2rem;
  background-color: #fff;
  display: block;
  transition: 0.4s cubic-bezier(0.25, 1, 0.5, 1);
  transform-origin: center;
}
.burger_btn.open span:nth-child(1) { transform: translateY(0.8rem) rotate(45deg); }
.burger_btn.open span:nth-child(2) { opacity: 0; }
.burger_btn.open span:nth-child(3) { transform: translateY(-0.8rem) rotate(-45deg); }

.mobile_overlay {
  display: block;
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  background-color: #000;
  z-index: 100000;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.5s, visibility 0.5s;
}
.mobile_overlay.open { opacity: 1; visibility: visible; }

.mobile_overlay nav {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
}
.mobile_overlay ul { display: flex; flex-direction: column; gap: 4rem; list-style: none; margin: 0; padding: 0; }
.mobile_overlay li {
  font-size: 3rem;
  font-weight: 500;
  letter-spacing: -0.1rem;
  text-transform: uppercase;
}
.mobile_overlay li a {
  display: inline-block;
  border: 1px solid rgba(255,255,255,0.3);
  border-radius: 5rem;
  padding: 1rem 3rem;
  text-decoration: none;
  color: #fff;
  transition: background-color 0.3s, color 0.3s;
}
.mobile_overlay li a:hover { background-color: #fff; color: #000; }
```

Usage:
```jsx
<MobileMenu
  logo="AC."
  logoHref="/"
  links={[
    { label: 'HOME', href: '/' },
    { label: 'WORK', href: '/work' },
    { label: 'CONTACT', href: 'mailto:hi@example.com' },
  ]}
/>
```

Prompt:
> "Build a mobile navigation header with a compact logo on the left and a hamburger icon on the right. Clicking the hamburger animates it into an X (3-line to X transform using CSS). It opens a fullscreen black overlay menu with centered uppercase links in pill-shaped borders. Links animate to white background on hover."

Registry JSON:
```json
{
  "name": "MobileMenu",
  "category": "navigation",
  "tags": ["mobile", "hamburger", "overlay", "fullscreen-menu", "responsive"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

Name: ElasticLineDivider

Code:
```jsx
import { useEffect, useRef } from 'react';

type Props = {
  label?: string;
  index?: string;
  total?: string;
};

export default function ElasticLineDivider({ label = '', index = '01/', total = '/04' }: Props) {
  const svgRef = useRef<SVGSVGElement>(null);

  useEffect(() => {
    const svg = svgRef.current;
    if (!svg) return;
    const path = svg.querySelector('path') as SVGPathElement;
    if (!path) return;

    const N = 80, MID = 20, SPRING = 0.18, DAMPING = 0.55,
          PROPAGATION = 0.25, INFLUENCE = 18, AMPLITUDE = 2.8;

    const pos = new Float32Array(N + 1);
    const vel = new Float32Array(N + 1);
    let mouseActive = false, mouseIdx = 0, mouseForce = 0, raf: number | null = null;

    function buildPath() {
      const step = 1440 / N;
      let d = `M0,${MID + pos[0]}`;
      for (let i = 1; i <= N; i++) {
        const x = i * step, y = MID + pos[i], py = MID + pos[i - 1];
        const cpx = (i - 0.5) * step;
        d += ` C${cpx},${py} ${cpx},${y} ${x},${y}`;
      }
      path.setAttribute('d', d);
    }

    function tick() {
      if (mouseActive) {
        for (let i = 0; i <= N; i++) {
          const dist = Math.abs(i - mouseIdx);
          if (dist < INFLUENCE) {
            const falloff = Math.exp(-3 * (dist / INFLUENCE) ** 2);
            vel[i] += (mouseForce * falloff - pos[i] * 0.05) * 0.4;
          }
        }
      }
      const prevPos = new Float32Array(pos);
      for (let i = 1; i < N; i++) {
        vel[i] += ((prevPos[i - 1] + prevPos[i + 1]) * 0.5 - prevPos[i]) * PROPAGATION;
      }
      let energy = 0;
      for (let i = 0; i <= N; i++) {
        vel[i] += -pos[i] * SPRING;
        vel[i] *= DAMPING;
        pos[i] += vel[i];
        energy += Math.abs(vel[i]) + Math.abs(pos[i]);
      }
      pos[0] = 0; vel[0] = 0; pos[N] = 0; vel[N] = 0;
      buildPath();
      if (energy > 0.02) { raf = requestAnimationFrame(tick); }
      else { for (let i = 0; i <= N; i++) { pos[i] = 0; vel[i] = 0; } buildPath(); raf = null; }
    }

    const ensure = () => { if (!raf) raf = requestAnimationFrame(tick); };

    const onMove = (e: MouseEvent) => {
      const rect = svg.getBoundingClientRect();
      mouseIdx = Math.max(0, Math.min(1, (e.clientX - rect.left) / rect.width)) * N;
      mouseForce = ((e.clientY - (rect.top + rect.height / 2)) / (rect.height / 2)) * AMPLITUDE;
      mouseActive = true;
      ensure();
    };
    const onLeave = () => { mouseActive = false; ensure(); };

    svg.addEventListener('mousemove', onMove);
    svg.addEventListener('mouseleave', onLeave);
    return () => {
      svg.removeEventListener('mousemove', onMove);
      svg.removeEventListener('mouseleave', onLeave);
      if (raf) cancelAnimationFrame(raf);
    };
  }, []);

  return (
    <div className="elastic_divider">
      <svg
        ref={svgRef}
        className="elastic_divider_line"
        preserveAspectRatio="none"
        viewBox="0 0 1440 40"
      >
        <path d="M0,20 L1440,20" stroke="#fff" strokeWidth="1" fill="none" vectorEffect="non-scaling-stroke" />
      </svg>
      {(index || label || total) && (
        <div className="elastic_divider_inner">
          <span>{index}</span>
          <span>{label}</span>
          <span>{total}</span>
        </div>
      )}
    </div>
  );
}
```

Props:
```ts
type Props = {
  label?: string;  // Center label text — e.g. "about"
  index?: string;  // Left counter — e.g. "02/"
  total?: string;  // Right counter — e.g. "/04"
};
```

Dependencies:
- React 18+ (useEffect, useRef)
- No external packages — pure canvas math

Styles:
```css
.elastic_divider { padding: 0 5rem; margin-bottom: 5rem; }
.elastic_divider_line {
  display: block;
  width: calc(100% + 10rem);
  margin-left: -5rem;
  height: 80px;
  margin-top: -30px;
  margin-bottom: -30px;
  overflow: visible;
  transform-origin: left;
  position: relative;
  z-index: 2;
}
.elastic_divider_inner {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 2rem;
  font-size: 1.4rem;
  letter-spacing: -0.056rem;
  text-transform: uppercase;
  color: #fff;
}
```

Usage:
```jsx
<ElasticLineDivider index="02/" label="services" total="/05" />
```

Prompt:
> "Create an interactive SVG horizontal divider line that reacts to mouse movement like an elastic thread with spring physics. When the cursor hovers and moves vertically over the line, it deflects the nearest points up or down. A wave propagates outward to neighboring points, damped like a guitar string settling back to rest. Section number labels appear below."

Registry JSON:
```json
{
  "name": "ElasticLineDivider",
  "category": "dividers",
  "tags": ["svg", "physics", "spring", "interactive", "wave", "divider"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

Name: CircleCTA

Code:
```jsx
import { useEffect, useRef } from 'react';
import gsap from 'gsap';

type Props = {
  href: string;
  label?: string;
  size?: number;    // diameter in rem — default 10
  strokeColor?: string;
};

export default function CircleCTA({ href, label = 'view\nmore', size = 10, strokeColor = '#fff' }: Props) {
  const ctaRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const cta = ctaRef.current;
    if (!cta) return;
    const circle = cta.querySelector<SVGCircleElement>('.circle_draw');
    if (!circle) return;

    const circumference = 2 * Math.PI * 48;
    gsap.set(circle, { strokeDasharray: circumference, strokeDashoffset: circumference });

    const enter = () => gsap.to(circle, { strokeDashoffset: 0, duration: 0.6, ease: 'power2.out' });
    const leave = () => gsap.to(circle, { strokeDashoffset: circumference, duration: 0.4, ease: 'power2.in' });

    cta.addEventListener('mouseenter', enter);
    cta.addEventListener('mouseleave', leave);
    return () => { cta.removeEventListener('mouseenter', enter); cta.removeEventListener('mouseleave', leave); };
  }, []);

  const lines = label.split('\n');

  return (
    <div className="circle_cta" ref={ctaRef} style={{ width: `${size}rem`, height: `${size}rem` }}>
      <a href={href}>
        <svg viewBox="0 0 100 100">
          <circle cx="50" cy="50" r="48" className="circle_draw" stroke={strokeColor} />
        </svg>
        <span className="circle_label">
          {lines.map((l, i) => <span key={i}>{l}{i < lines.length - 1 && <br />}</span>)}
        </span>
      </a>
    </div>
  );
}
```

Props:
```ts
type Props = {
  href: string;           // Link target (URL or mailto:)
  label?: string;         // Label text; use \n for line breaks — default "view\nmore"
  size?: number;          // Diameter in rem — default 10
  strokeColor?: string;   // SVG circle stroke color — default "#fff"
};
```

Dependencies:
- gsap (npm: gsap)
- React 18+

Styles:
```css
.circle_cta { position: relative; }
.circle_cta a { display: block; width: 100%; height: 100%; position: relative; }
.circle_cta svg { width: 100%; height: 100%; position: absolute; top: 0; left: 0; transform: rotate(-90deg); }
.circle_draw { fill: none; stroke-width: 1; }
.circle_label {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  font-size: 1.1rem;
  text-align: center;
  line-height: 1.5;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: #fff;
  transition: opacity 0.3s;
}
.circle_cta:hover .circle_label { opacity: 0.7; }
```

Usage:
```jsx
<CircleCTA href="/projects" label={"view\nall"} size={10} />
<CircleCTA href="mailto:hi@me.com" label={"write a\nmessage"} size={13} />
```

Prompt:
> "Create a circular call-to-action button that draws an SVG circle stroke around itself on hover using GSAP strokeDashoffset animation. The circle starts as invisible (dashoffset = circumference) and animates to fully drawn on mouseenter, then retracts on mouseleave. Centered label text inside the circle fades slightly on hover."

Registry JSON:
```json
{
  "name": "CircleCTA",
  "category": "buttons",
  "tags": ["cta", "svg", "stroke-draw", "hover", "circular", "gsap", "animated"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

Name: ImageReveal

Code:
```jsx
import { useEffect, useRef } from 'react';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

type Props = {
  src: string;
  alt?: string;
  className?: string;
  borderRadius?: string;
  aspectRatio?: string;
};

export default function ImageReveal({ src, alt = '', className = '', borderRadius = '4rem', aspectRatio = '3/2' }: Props) {
  const wrapRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const el = wrapRef.current;
    if (!el) return;
    const img = el.querySelector('img');

    gsap.set(el, { clipPath: 'inset(100% 0% 0% 0%)' });
    if (img) gsap.set(img, { scale: 1.4 });

    const st = ScrollTrigger.create({
      trigger: el,
      start: 'top 82%',
      end: 'bottom 12%',
      onEnter:     () => { gsap.to(el, { clipPath: 'inset(-0.5% -0.5% -0.5% -0.5%)', duration: 1.2, ease: 'power3.inOut', overwrite: true }); if (img) gsap.to(img, { scale: 1, duration: 1.5, ease: 'power3.out', overwrite: true }); },
      onLeave:     () => { gsap.to(el, { clipPath: 'inset(100% 0% 0% 0%)', duration: 2.0, ease: 'power2.in', overwrite: true }); if (img) gsap.to(img, { scale: 1.4, duration: 2.0, ease: 'power2.in', overwrite: true }); },
      onEnterBack: () => { gsap.to(el, { clipPath: 'inset(-0.5% -0.5% -0.5% -0.5%)', duration: 1.2, ease: 'power3.inOut', overwrite: true }); if (img) gsap.to(img, { scale: 1, duration: 1.5, ease: 'power3.out', overwrite: true }); },
      onLeaveBack: () => { gsap.to(el, { clipPath: 'inset(100% 0% 0% 0%)', duration: 2.0, ease: 'power2.in', overwrite: true }); if (img) gsap.to(img, { scale: 1.4, duration: 2.0, ease: 'power2.in', overwrite: true }); },
    });

    return () => st.kill();
  }, []);

  return (
    <div
      ref={wrapRef}
      className={`img_reveal_wrap ${className}`}
      style={{ borderRadius, aspectRatio, overflow: 'hidden', isolation: 'isolate', transform: 'translateZ(0)' }}
    >
      <img src={src} alt={alt} loading="lazy" decoding="async" style={{ width: '100%', height: '100%', objectFit: 'cover', display: 'block' }} />
    </div>
  );
}
```

Props:
```ts
type Props = {
  src: string;             // Image URL
  alt?: string;            // Accessible alt text
  className?: string;      // Extra class names
  borderRadius?: string;   // CSS border-radius — default "4rem"
  aspectRatio?: string;    // CSS aspect-ratio — default "3/2"
};
```

Dependencies:
- gsap (npm: gsap)
- gsap/ScrollTrigger (bundled with gsap)
- React 18+

Styles:
```css
/* No extra CSS needed — all styles are inline via props.
   Optional: add object-position as a style prop for focal-point control. */
```

Usage:
```jsx
<ImageReveal src="/photo.jpg" alt="Mountain valley at sunset" borderRadius="2rem" aspectRatio="16/9" />
```

Prompt:
> "Create an image component that reveals itself on scroll using GSAP clipPath animation. The image starts hidden (clipPath inset 100% from top), and on entering the viewport it clips open downward with a power3 ease. Simultaneously the image itself scales down from 1.4 to 1 for a cinematic parallax feel. Reverses when scrolled out. Rounded corners via border-radius."

Registry JSON:
```json
{
  "name": "ImageReveal",
  "category": "media",
  "tags": ["image", "scroll-reveal", "gsap", "clip-path", "parallax", "cinema"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

Name: FeaturedProjectCard

Code:
```jsx
type Props = {
  title: string;
  eyebrow?: string;
  category?: string;
  year?: string | number;
  imageSrc: string;
  imageAlt?: string;
  href?: string;
  alignRight?: boolean;
};

export default function FeaturedProjectCard({
  title,
  eyebrow = 'Featured Work',
  category = '',
  year = '',
  imageSrc,
  imageAlt,
  href = '#',
  alignRight = false,
}: Props) {
  return (
    <div className={`feat_card ${alignRight ? 'feat_card--right' : ''}`}>
      <a href={href}>
        <div className="feat_card_img img_reveal_wrap">
          <img src={imageSrc} alt={imageAlt || title} loading="lazy" decoding="async" />
        </div>
        <div className={`feat_card_info ${alignRight ? 'feat_card_info--right' : ''}`}>
          <span className="feat_eyebrow">{eyebrow}</span>
          <h3 className="feat_title">{title}</h3>
          <div className="feat_meta">
            {category && <span>{category}</span>}
            {year && <span>{year}</span>}
          </div>
        </div>
      </a>
    </div>
  );
}
```

Props:
```ts
type Props = {
  title: string;          // Project/work title
  eyebrow?: string;       // Small label above title — default "Featured Work"
  category?: string;      // Category label — e.g. "Street + Urban"
  year?: string | number; // Year label
  imageSrc: string;       // Hero image URL
  imageAlt?: string;      // Alt text — defaults to title
  href?: string;          // Card link target — default "#"
  alignRight?: boolean;   // Align image & info to the right (for alternating layout)
};
```

Dependencies:
- React 18+
- ImageReveal component (or supply `.img_reveal_wrap` CSS separately)

Styles:
```css
.feat_card { position: relative; }
.feat_card a { display: block; text-decoration: none; color: inherit; }

.feat_card_img {
  width: 100%;
  height: 60rem;
  overflow: hidden;
  border-radius: 4rem;
  isolation: isolate;
  transform: translateZ(0);
}
.feat_card_img img { width: 100%; height: 100%; object-fit: cover; display: block; }
.feat_card--right .feat_card_img { width: 70%; margin-left: auto; }

.feat_card_info { margin-top: 3rem; }
.feat_card_info--right { text-align: right; }

.feat_eyebrow {
  display: block;
  font-size: 1.4rem;
  letter-spacing: -0.056rem;
  margin-bottom: 1.5rem;
  opacity: 0.5;
  text-transform: uppercase;
  color: #fff;
}
.feat_title {
  font-size: 8rem;
  line-height: 7rem;
  letter-spacing: -0.3rem;
  font-weight: 500;
  margin-bottom: 2rem;
  text-transform: uppercase;
  color: #fff;
}
.feat_meta {
  display: flex;
  gap: 3rem;
  font-size: 1.1rem;
  letter-spacing: 0;
  opacity: 0.5;
  color: #fff;
  text-transform: none;
}
.feat_card_info--right .feat_meta { justify-content: flex-end; }

@media (max-width: 768px) {
  .feat_card_img { height: 35rem; border-radius: 2rem; }
  .feat_title { font-size: 4rem; line-height: 3.5rem; }
  .feat_card--right .feat_card_img { width: 100%; }
  .feat_card_info--right { text-align: left; }
}
```

Usage:
```jsx
<FeaturedProjectCard
  title="Golden Hour"
  eyebrow="Featured Photograph"
  category="Photography + Nature"
  year={2023}
  imageSrc="/images/sunset.jpg"
  href="/projects"
  alignRight={false}
/>

/* Alternating layout */
{projects.map((p, i) => (
  <FeaturedProjectCard key={p.id} {...p} alignRight={i % 2 !== 0} />
))}
```

Prompt:
> "Create a large full-width project card for a portfolio showcase. It has a tall rounded image on top, followed by a small eyebrow label, a large bold title in uppercase, and meta tags (category + year) below. Every other card shifts the image to the right 70% and right-aligns the text for visual rhythm. Dark background, white text throughout."

Registry JSON:
```json
{
  "name": "FeaturedProjectCard",
  "category": "cards",
  "tags": ["project-card", "portfolio", "editorial", "dark", "alternating-layout"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

Name: ProjectCard

Code:
```jsx
type Props = {
  title: string;
  subtitle?: string;
  imageSrc: string;
  imageAlt?: string;
  href?: string;
  large?: boolean;
};

export default function ProjectCard({ title, subtitle = '', imageSrc, imageAlt, href = '#', large = false }: Props) {
  return (
    <div className={`proj_card ${large ? 'proj_card--large' : ''}`}>
      <a href={href}>
        <div className="proj_card_img img_reveal_wrap">
          <img src={imageSrc} alt={imageAlt || title} loading="lazy" decoding="async" />
        </div>
        <div className="proj_card_info">
          <h3>{title}</h3>
          {subtitle && <span>{subtitle}</span>}
        </div>
      </a>
    </div>
  );
}
```

Props:
```ts
type Props = {
  title: string;       // Project title
  subtitle?: string;   // Category/date — e.g. "Photography — 2023"
  imageSrc: string;    // Image URL
  imageAlt?: string;   // Alt text
  href?: string;       // Card link — default "#"
  large?: boolean;     // Applies larger title typography variant
};
```

Dependencies:
- React 18+

Styles:
```css
.proj_card { position: relative; break-inside: avoid; margin-bottom: 5rem; }
.proj_card a { display: block; text-decoration: none; color: inherit; }

.proj_card_img {
  overflow: hidden;
  border-radius: 4rem;
  isolation: isolate;
}
.proj_card_img img {
  width: 100%;
  height: auto;
  display: block;
  transition: transform 0.8s cubic-bezier(0.25, 1, 0.5, 1);
}
.proj_card:hover .proj_card_img img { transform: scale(1.05); }

.proj_card_info { margin-top: 2rem; }
.proj_card_info h3 {
  font-size: 4rem;
  line-height: 3.6rem;
  letter-spacing: -0.2rem;
  font-weight: 500;
  margin-bottom: 1rem;
  text-transform: uppercase;
  color: #fff;
}
.proj_card--large .proj_card_info h3 {
  font-size: 6rem;
  line-height: 5rem;
  letter-spacing: -0.3rem;
}
.proj_card_info span {
  font-size: 1.1rem;
  letter-spacing: 0;
  opacity: 0.5;
  text-transform: none;
  color: #fff;
}

/* Masonry grid container — apply to parent element */
.proj_grid {
  column-count: 2;
  column-gap: 5rem;
  padding: 0 5rem;
}
@media (max-width: 768px) {
  .proj_grid { column-count: 1; padding: 0 2rem; }
  .proj_card_img { border-radius: 2rem; }
  .proj_card_info h3 { font-size: 3rem; line-height: 2.8rem; }
  .proj_card--large .proj_card_info h3 { font-size: 3rem; line-height: 2.8rem; }
}
```

Usage:
```jsx
/* Wrap cards in .proj_grid for masonry layout */
<div className="proj_grid">
  {photos.map((p, i) => (
    <ProjectCard
      key={p.id}
      title={p.title}
      subtitle={`${p.category} — ${p.year}`}
      imageSrc={p.src}
      href={p.href}
      large={i % 5 === 0}
    />
  ))}
</div>
```

Prompt:
> "Create a masonry-style project card for a photography portfolio grid. It has a rounded image that scales slightly on hover, a title in large uppercase text, and a small subtitle below. Every 5th card gets extra-large title typography for visual variety. Two-column CSS masonry layout, single column on mobile."

Registry JSON:
```json
{
  "name": "ProjectCard",
  "category": "cards",
  "tags": ["masonry", "project-card", "grid", "hover-zoom", "portfolio"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

Name: Marquee

Code:
```jsx
import { useEffect, useRef } from 'react';
import gsap from 'gsap';

type Props = {
  text: string;
  speed?: number;    // Duration of one full cycle in seconds — default 20
  fontSize?: string; // CSS font-size — default "21rem"
  opacity?: number;  // 0–1 — default 0.08
  separator?: string;
};

export default function Marquee({ text, speed = 20, fontSize = '21rem', opacity = 0.08, separator = ' —\u00A0' }: Props) {
  const trackRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const track = trackRef.current;
    if (!track) return;

    const tween = gsap.to(track, {
      xPercent: -50,
      repeat: -1,
      duration: speed,
      ease: 'none',
    });
    return () => { tween.kill(); };
  }, [speed]);

  // Duplicate 4× so the seamless loop always fills the viewport
  const items = Array.from({ length: 4 }, (_, i) => (
    <span key={i} className="marquee_item">{text}{separator}</span>
  ));

  return (
    <div className="marquee_root" style={{ opacity }}>
      <div className="marquee_track" ref={trackRef}>
        {items}
      </div>
    </div>
  );
}
```

Props:
```ts
type Props = {
  text: string;        // Text content to repeat
  speed?: number;      // Scroll cycle duration in seconds — default 20
  fontSize?: string;   // CSS font-size value — default "21rem"
  opacity?: number;    // Overall opacity (0–1) — default 0.08
  separator?: string;  // String appended after each item — default " — "
};
```

Dependencies:
- gsap (npm: gsap)
- React 18+

Styles:
```css
.marquee_root { overflow: hidden; pointer-events: none; margin: 5rem 0; }
.marquee_track {
  display: flex;
  width: max-content;
  will-change: transform;
}
.marquee_item {
  font-size: 21rem;   /* Override via CSS variable or inline style prop */
  font-weight: 500;
  letter-spacing: -0.5rem;
  white-space: nowrap;
  padding-right: 3rem;
  color: #fff;
  text-transform: uppercase;
}
@media (max-width: 1024px) { .marquee_item { font-size: 14rem; } }
@media (max-width: 768px)  { .marquee_item { font-size: 10rem; } }
```

Usage:
```jsx
<Marquee text="mahin kumar" speed={20} opacity={0.08} />
<Marquee text="creative studio" speed={15} fontSize="10rem" opacity={0.12} separator=" ★ " />
```

Prompt:
> "Create an infinite horizontal scrolling marquee of repeated text using GSAP. The text is oversized (20+ rem), uppercase, white, and semi-transparent (~8% opacity) for a background watermark effect. It scrolls left endlessly at a constant speed. On mobile the font-size reduces proportionally."

Registry JSON:
```json
{
  "name": "Marquee",
  "category": "animation",
  "tags": ["marquee", "scroll", "gsap", "infinite", "text", "watermark"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

Name: ContactSection

Code:
```jsx
import { useEffect, useRef } from 'react';
import gsap from 'gsap';

type Props = {
  eyebrow?: string;
  titleLine1?: string;
  titleLine2?: string;
  ctaLabel?: string;
  ctaHref?: string;
};

export default function ContactSection({
  eyebrow = "GOT A PROJECT IN MIND?",
  titleLine1 = "let's",
  titleLine2 = "Connect",
  ctaLabel = "write a\nmessage",
  ctaHref = "mailto:hello@example.com",
}: Props) {
  const spotlightRef = useRef<HTMLDivElement>(null);
  const sectionRef = useRef<HTMLElement>(null);
  const circleRef = useRef<SVGCircleElement>(null);

  useEffect(() => {
    const section = sectionRef.current;
    const spotlight = spotlightRef.current;
    if (!section || !spotlight) return;

    const onEnter = () => gsap.to(spotlight, { opacity: 1, duration: 0.4 });
    const onLeave = () => gsap.to(spotlight, { opacity: 0, duration: 0.6 });
    const onMove = (e: MouseEvent) => {
      const rect = section.getBoundingClientRect();
      gsap.to(spotlight, {
        x: e.clientX - rect.left - spotlight.offsetWidth / 2,
        y: e.clientY - rect.top - spotlight.offsetHeight / 2,
        duration: 0.6,
        ease: 'power2.out',
        overwrite: 'auto',
      });
    };

    section.addEventListener('mouseenter', onEnter);
    section.addEventListener('mouseleave', onLeave);
    section.addEventListener('mousemove', onMove);

    // Circle CTA draw
    const circle = circleRef.current;
    if (circle) {
      const circumference = 2 * Math.PI * 48;
      gsap.set(circle, { strokeDasharray: circumference, strokeDashoffset: 0 });
    }

    return () => {
      section.removeEventListener('mouseenter', onEnter);
      section.removeEventListener('mouseleave', onLeave);
      section.removeEventListener('mousemove', onMove);
    };
  }, []);

  const ctaLines = ctaLabel.split('\n');

  return (
    <section className="contact_section" ref={sectionRef}>
      <div className="contact_spotlight" ref={spotlightRef} />
      <div className="contact_content">
        <span className="contact_eyebrow">{eyebrow}</span>
        <h2 className="contact_heading">
          <span className="contact_word">{titleLine1}</span>
          <span className="contact_word">{titleLine2}</span>
        </h2>
        <div className="contact_circle_cta">
          <a href={ctaHref}>
            <svg viewBox="0 0 100 100">
              <circle ref={circleRef} cx="50" cy="50" r="48" className="contact_circle_draw" />
            </svg>
            <span className="contact_circle_label">
              {ctaLines.map((l, i) => <span key={i}>{l}{i < ctaLines.length - 1 && <br />}</span>)}
            </span>
          </a>
        </div>
      </div>
    </section>
  );
}
```

Props:
```ts
type Props = {
  eyebrow?: string;    // Small top label — default "GOT A PROJECT IN MIND?"
  titleLine1?: string; // First title word — default "let's"
  titleLine2?: string; // Second title word — default "Connect"
  ctaLabel?: string;   // Circle button label (use \n for line break)
  ctaHref?: string;    // CTA link (mailto: or URL)
};
```

Dependencies:
- gsap (npm: gsap)
- React 18+

Styles:
```css
/* CSS custom property — register at top of stylesheet */
@property --border-angle {
  syntax: '<angle>';
  initial-value: 0deg;
  inherits: false;
}

.contact_section {
  min-height: 72vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  position: relative;
  padding: 8rem 6rem;
  margin: 0 3rem 3rem;
  border-radius: 2.5rem;
  background: radial-gradient(ellipse at 50% 60%, #181818 0%, #060606 68%);
  overflow: hidden;
}

/* Spinning conic-gradient border */
.contact_section::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  padding: 1px;
  background: conic-gradient(
    from var(--border-angle),
    rgba(70,70,70,0.45) 0%,
    rgba(70,70,70,0.45) 74%,
    rgba(160,160,160,0.2) 78%,
    rgba(255,255,255,0.75) 83%,
    rgba(160,160,160,0.2) 88%,
    rgba(70,70,70,0.45) 92%,
    rgba(70,70,70,0.45) 100%
  );
  -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
  -webkit-mask-composite: xor;
  mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
  mask-composite: exclude;
  animation: border-spin 5s linear infinite;
  pointer-events: none;
  z-index: 3;
}
@keyframes border-spin { to { --border-angle: 360deg; } }

/* Cursor spotlight glow */
.contact_spotlight {
  position: absolute;
  top: 0; left: 0;
  width: 55rem; height: 55rem;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(255,255,255,0.065) 0%, transparent 65%);
  pointer-events: none;
  z-index: 0;
  opacity: 0;
  will-change: transform;
}

.contact_content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2.5rem;
  position: relative;
  z-index: 1;
}

.contact_eyebrow {
  font-size: 1.3rem;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  opacity: 0.55;
  color: #fff;
}

.contact_heading {
  font-size: clamp(8rem, 11vw, 16rem);
  line-height: 0.9;
  letter-spacing: -0.02em;
  font-weight: 700;
  text-transform: uppercase;
  color: #fff;
}
.contact_word { display: inline-block; margin: 0 1rem; }

/* Circle CTA */
.contact_circle_cta { position: relative; width: 13rem; height: 13rem; margin-top: 4rem; }
.contact_circle_cta a { display: block; width: 100%; height: 100%; position: relative; }
.contact_circle_cta svg { width: 100%; height: 100%; position: absolute; top: 0; left: 0; transform: rotate(-90deg); }
.contact_circle_draw { fill: none; stroke: #fff; stroke-width: 1; }
.contact_circle_label {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  font-size: 1.2rem;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  text-align: center;
  line-height: 1.6;
  color: #fff;
  opacity: 0.8;
}

@media (max-width: 768px) {
  .contact_section { padding: 8rem 2rem; min-height: 80vh; }
  .contact_heading { font-size: 5rem; line-height: 4rem; }
}
```

Usage:
```jsx
<ContactSection
  eyebrow="READY TO BUILD SOMETHING?"
  titleLine1="let's"
  titleLine2="Talk"
  ctaLabel={"send a\nmessage"}
  ctaHref="mailto:hello@you.com"
/>
```

Prompt:
> "Create a dark contact section card with a rotating conic-gradient border animated via CSS custom property. The background is a subtle radial gradient. A large glowing spotlight follows the cursor inside the card. The card contains a small eyebrow label, a massive two-word heading, and a circular CTA button with its stroke permanently drawn. Uses @property to animate --border-angle for the spinning border effect."

Registry JSON:
```json
{
  "name": "ContactSection",
  "category": "sections",
  "tags": ["contact", "conic-gradient", "animated-border", "spotlight", "dark-card", "glassmorphism"]
}
```

=== COMPONENT END ===

---

=== COMPONENT START ===

Name: TextDisperseLink

Code:
```jsx
import { useRef } from 'react';
import gsap from 'gsap';

const SCATTER = [
  { x: -0.8, y: -0.6, rz: -29 }, { x: -0.2, y: -0.4, rz: -6 },
  { x: -0.05, y: 0.1, rz: 12 },  { x: -0.05, y: -0.1, rz: -9 },
  { x: -0.1, y: 0.55, rz: 3 },   { x: 0, y: -0.1, rz: 9 },
  { x: 0, y: 0.15, rz: -12 },    { x: 0, y: 0.15, rz: -17 },
  { x: 0, y: -0.65, rz: 9 },     { x: 0.1, y: 0.4, rz: 12 },
  { x: 0, y: -0.15, rz: -9 },    { x: 0.2, y: 0.15, rz: 12 },
  { x: 0.8, y: 0.6, rz: 20 },
];

type Props = {
  label: string;
  href: string;
  target?: string;
  rel?: string;
  className?: string;
};

export default function TextDisperseLink({ label, href, target, rel, className = '' }: Props) {
  const linkRef = useRef<HTMLAnchorElement>(null);

  const getOffsets = () => {
    const em = parseFloat(getComputedStyle(linkRef.current!).fontSize);
    return label.split('').map((_, i) => {
      const t = SCATTER[i % SCATTER.length];
      return { x: t.x * em, y: t.y * em, rz: t.rz };
    });
  };

  const onEnter = () => {
    const spans = linkRef.current?.querySelectorAll<HTMLSpanElement>('.d_char');
    const offsets = getOffsets();
    spans?.forEach((span, i) => {
      gsap.to(span, { x: offsets[i].x, y: offsets[i].y, rotation: offsets[i].rz, duration: 0.75, ease: 'power3.out', overwrite: 'auto' });
    });
  };

  const onLeave = () => {
    const spans = linkRef.current?.querySelectorAll<HTMLSpanElement>('.d_char');
    spans?.forEach((span) => {
      gsap.to(span, { x: 0, y: 0, rotation: 0, duration: 0.75, ease: 'power3.out', overwrite: 'auto' });
    });
  };

  return (
    <a
      ref={linkRef}
      href={href}
      target={target}
      rel={rel}
      aria-label={label}
      className={`disperse_link ${className}`}
      onMouseEnter={onEnter}
      onMouseLeave={onLeave}
    >
      {label.split('').map((ch, i) => (
        <span key={i} className="d_char">{ch}</span>
      ))}
    </a>
  );
}
```

Props:
```ts
type Props = {
  label: string;      // Link text — each character gets scattered independently
  href: string;       // Link target URL
  target?: string;    // e.g. "_blank"
  rel?: string;       // e.g. "noopener noreferrer"
  className?: string; // Extra class names
};
```

Dependencies:
- gsap (npm: gsap)
- React 18+

Styles:
```css
.disperse_link {
  display: inline-flex;
  position: relative;
  cursor: pointer;
  text-decoration: none;
  color: inherit;
  font-size: 1.4rem;
  letter-spacing: 0.06em;
  text-transform: uppercase;
}
.d_char {
  display: inline-block;
  will-change: transform;
}
```

Usage:
```jsx
<TextDisperseLink label="INSTAGRAM" href="https://instagram.com/yourhandle" target="_blank" rel="noopener noreferrer" />
<TextDisperseLink label="TWITTER" href="https://twitter.com/handle" target="_blank" rel="noopener noreferrer" />
```

Prompt:
> "Create a text link where each character scatters to a pre-defined offset (translates + rotates slightly) on hover, then snaps back to their resting positions on mouse leave. Offsets are proportional to the current font-size in em. Animation is handled by GSAP with power3 easing for springy character scatter and return."

Registry JSON:
```json
{
  "name": "TextDisperseLink",
  "category": "buttons",
  "tags": ["text", "hover", "scatter", "gsap", "character-animation", "social", "link"]
}
```

=== COMPONENT END ===

---

## Summary Table

| # | Component | Category | Key Tech |
|---|-----------|----------|----------|
| 1 | `CustomCursor` | interaction | GSAP, mix-blend-mode |
| 2 | `Preloader` | loaders | GSAP, letter stagger |
| 3 | `SiteHeader` | navigation | CSS hover fill bubble |
| 4 | `MobileMenu` | navigation | CSS transition overlay |
| 5 | `ElasticLineDivider` | dividers | Float32Array spring physics |
| 6 | `CircleCTA` | buttons | GSAP SVG stroke-dashoffset |
| 7 | `ImageReveal` | media | GSAP clip-path + scale |
| 8 | `FeaturedProjectCard` | cards | Alternating layout |
| 9 | `ProjectCard` | cards | CSS masonry, hover-zoom |
| 10 | `Marquee` | animation | GSAP xPercent loop |
| 11 | `ContactSection` | sections | conic-gradient @property, spotlight |
| 12 | `TextDisperseLink` | buttons | GSAP per-char scatter |

## Global Requirements

All components assume:
- **Font:** `Space Grotesk` from Google Fonts — `https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700`
- **Base HTML font-size:** `0.6944vw` (so `1rem ≈ 10px` at 1440px viewport) — adjust if using Tailwind rem scale
- **Color scheme:** `#000` background, `#fff` text
- **Body reset:** `text-transform: uppercase`, `letter-spacing: -0.07rem`
- **npm packages:** `gsap@^3`
- **Smooth scroll (optional):** `lenis@^1`

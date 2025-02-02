---
slug: responsive-design-fem
title: Responsive Design Pathway - FEM Part 1
description: "Responsive Design pathway in the Frontend Mentor Website "
publishedAt: 2025-02-02
tags:
  - devcontainers
  - development-environment
  - docker
  - vscode
  - productivity
---

## Responsive Design Pathway - FEM Part 1

Use rem or em for breakpoint sizes 
    * <mark>Adapts better:</mark> Relative units scale with the user’s screen size or browser settings. For example, if someone has their browser zoomed in for easier reading, relative units adjust accordingly, but pixels stay the same size. This means your site can adapt better to different viewing conditions.
    * <mark>More consistent across devices:</mark> Different devices have different pixel densities. What looks good in pixels on one screen might look too small or too big on another. Relative units help ensure your design looks consistent, no matter the device.
    * <mark>Future-proof:</mark> As new devices with various screen sizes and resolutions come out, using relative units means your site is more likely to look good on them without needing adjustments.

### Modern CSS techniques for fluid typography
- The clamp() function: CSS clamp() offers a more controlled approach, allowing you to specify a minimum font size, a preferred size based on the viewport, and a maximum font size.

Example: ```font-size: clamp(1rem, 0.8rem + 1vw, 3rem)```

How It Works:
* If the viewport is small, the font size will stay at 1rem (the minimum).
* As the viewport grows, the font size increases according to 0.8rem + 1vw.
* If the viewport becomes very large, the font size caps at 3rem (the maximum).

Additional info: 
0.8rem + 1vw → This is the preferred (dynamic) font size that scales based on the viewport width (vw).

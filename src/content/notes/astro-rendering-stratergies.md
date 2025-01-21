---
slug: astro-rendering-stratergies
publishedAt: 2025-01-21
title: Astro - Rendering Stratergies
description: Learning about Astro through ChatGPT and YT videos
category: astro
---

## Astro SSG rendering method ChatGPT explained 
[Video to coding in public](https://youtu.be/aIHRjloFASU?si=BGFTsx1OAMGalCPA) explaining this.

**Thoughts**: First, I needed to understand the execution between `SSG` and `SSR`. 

**My question to ChatGPT**: With SSR, does the content get generated on user request?

***ChatGPT:*** Yes, with Server-Side Rendering (SSR), content is generated on the server in response to each user request. When a user requests a webpage, the server processes the request, fetches necessary data, and constructs the HTML content before sending it to the user’s browser. This approach ensures that users receive fully rendered HTML pages, leading to faster initial page loads and improved SEO, as search engines can easily crawl and index the pre-rendered content.  
___
**My question to ChatGPT:** With SSG, content is built during build mode? Which means the content is already generated when a user makes a request? 

***ChatGPT***: Yes, with Static Site Generation (SSG), content is pre-rendered during the build process. This means that when a user requests a page, the server delivers a pre-generated static HTML file, resulting in faster load times and reduced server load.  
___
**My question to ChatGPT:** During build process, if the content is already generated where is it stored? Is it already sent to the server waiting upon a request? 

***ChatGPT:*** During the build process in Static Site Generation (SSG), the content is pre-rendered into static HTML files. These files are then stored on the server or deployed to a Content Delivery Network (CDN). When a user requests a page, the server or CDN serves these pre-generated static files directly, resulting in faster load times and reduced server processing.  
___
**Thoughts:** Second, I needed to understand how Astro handles components that DO have JS. Since, it ships with no JS by default. 

**My question to ChatGPT:** So, Astro by default is SSG and it doesn’t ship JavaScript by default. But if a component has JavaScript during build process, what happens? 

***ChatGPT:*** Astro by default is an SSG and generates static HTML without JavaScript. However, if a component in your Astro project includes JavaScript, Astro uses its “partial hydration” feature, also known as the Islands Architecture:	
1. Static HTML Output: During the build process, Astro renders the component’s initial HTML to be part of the static site. 
2. Selective JavaScript Delivery: Astro identifies components requiring interactivity and ships only the required JavaScript for those components to the client.
3. Hydration Modes: You can control how and when the JavaScript is executed on the client. Astro supports different hydration options, such as 
    * load: Hydrate the component as soon as the page loads. 
    * idle: Hydrate when the browser is idle.
    * visible: Hydrate when the component enters the viewport. 
    * media: Hydrate based on a specific media query
    * only: Explicitly hydrate only on the client.

This ensures that JavaScript is shipped only where it is needed, while the rest of the site remains purely static, optimizing performance.
___

**My question to ChatGPT:** So if I don’t define the hydration mode, what will happen? 

***ChatGPT:*** If you don’t explicitly define a hydration mode for a JavaScript component in Astro, it will not include any JavaScript for that component on the client side by default. This means:
1. No Client-Side Interactivity: The component will be rendered as static HTML during the build process, but it won’t be interactive in the browser because no JavaScript is sent or executed for it.
2. Explicit Hydration Required: Astro requires you to explicitly specify a hydration mode (like client:load, client:idle, etc.) for components that need interactivity. Without this, Astro treats the component as a purely server-rendered or static component. 

If your component is interactive but you don’t set a hydration mode, users will see the rendered HTML, but any client-side functionality (e.g., button clicks, form submissions with dynamic behaviors) will not work. To enable interactivity, you can define a hydration mode like this:

<Component client:load />

This tells Astro to hydrate the component and include the necessary JavaScript in the build.
___

**My question to ChatGPT:** So to clarify, when I have a component that has some JS in it, the JS is included during the build process. But it’s not executed until one of the hydration modes is set. Is this correct?

***ChatGPT:*** Yes, that’s correct! Here’s a breakdown of the process:
1.	Build Process:
•	During the build, Astro identifies components that contain JavaScript.
•	The necessary JavaScript for these components is included in the output (bundled and shipped to the client).
•	However, unless a hydration mode is explicitly set (e.g., client:load, client:visible), the JavaScript is not executed on the client side.
2.	Without Hydration Mode:
•	If you don’t specify a hydration mode, Astro treats the component as purely static.
•	The JavaScript is not sent to or executed in the browser. Only the rendered HTML is delivered, so the component won’t have any interactivity.
3.	With Hydration Mode:
•	If you set a hydration mode (e.g., client:load or client:idle), the bundled JavaScript is sent to the browser.
•	The browser executes the JavaScript based on the hydration mode you specify (e.g., immediately after page load, when the component becomes visible, etc.).
•	This enables interactivity in the component.

Example

Without Hydration Mode

```js
<MyComponent />
```
•	The HTML from MyComponent is rendered statically during the build.
•	No JavaScript is shipped, so the component is not interactive.

With Hydration Mode

```js
<MyComponent client:visible />
```
•	The HTML from MyComponent is rendered statically during the build.
•	The JavaScript is shipped to the browser but only executed when the component becomes visible in the viewport.

Key Takeaway
•	The JavaScript is bundled during the build for components with JS.
•	It is executed on the client only when you specify a hydration mode.
•	Without a hydration mode, the component remains static, and the JavaScript is excluded from the client build.
___

**Thoughts:** I didn’t quite understand how the ```:only``` hydration method worked, so I asked ChatGPT to explain further. 

**My question to chatGPT:** Can you explain more about the :only hydration mode? 

***ChatGPT:*** The ```:only``` hydration mode in Astro is used to render and hydrate a component exclusively on the client side, without pre-rendering any HTML during the build process. This is useful for components that rely entirely on client-side JavaScript and do not need server-rendered HTML.

Key Characteristics of client:only
1.	No HTML Pre-Rendering: Unlike other hydration modes, Astro does not generate static HTML for the component during the build. Instead, it adds a placeholder (like an empty <div>), and the component is fully rendered in the browser after being hydrated.
2.	Full Client-Side Rendering (CSR): The component is treated as if it belongs to a fully client-rendered app. This is helpful when integrating third-party libraries or frameworks (e.g., React, Vue, Svelte) that may not work well with server-side rendering.
3.	Use Case: It is typically used for dynamic or interactive widgets that depend on JavaScript and cannot be pre-rendered, such as
•	Maps
•	Charts
•	Animations
•	Components from third-party libraries (e.g., React or Vue components)

Syntax Example

Here’s how you define a client:only hydration mode:
```js
<!-- Using a React component -->
<script>
  import SomeReactComponent from './SomeReactComponent.jsx';
</script>

<SomeReactComponent client:only="react" />
```
In this example:
•	The client:only="react" attribute tells Astro not to pre-render the component on the server.
•	The component will load and render entirely on the client using React’s runtime.

Benefits 
•	Flexibility: Makes it easy to integrate client-side libraries or components into an Astro project.
•	Efficient Rendering: Avoids unnecessary server-side rendering for components that don’t need it.
•	Compatibility: Allows usage of libraries or components that may not support SSR.

Caveats
•	SEO Impact: Since no HTML is pre-rendered, search engines won’t index the component’s content.
•	Performance: It relies fully on client-side JavaScript, so it may impact load time if overused or applied to large components.

When to Use client:only

Use client:only when:
•	A component cannot be server-rendered.
•	The component relies entirely on client-side JavaScript.
•	You are integrating third-party libraries that are not compatible with SSR.

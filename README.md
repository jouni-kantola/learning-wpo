# Web Performance Optimization

Learning material for web and rendering performance optimization.

## Goals

Goal 1: Optimize [Test page: Janky Animation](https://github.com/GoogleChrome/devtools-samples/tree/main/jank) to cheaper animation

Goal 2: A measurable performance (before/after) optimization in https://github.com/jouni-kantola/grid-snake

## Study material

1. [Get Started With Analyzing Runtime Performance](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance) (added: 2021-03-03)
1. [Rendering Performance](https://developers.google.com/web/fundamentals/performance/rendering) (added: 2021-03-15)
1. [Test page: Janky Animation](https://googlechrome.github.io/devtools-samples/jank/) (added: 2021-03-22)
1. [How browser rendering works — behind the scenes](https://blog.logrocket.com/how-browser-rendering-works-behind-the-scenes-6782b0e8fb10/) (added: 2021-03-29)
1. [Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path) (added: 2021-03-29)

## Reference material

* [What forces layout / reflow](https://gist.github.com/paulirish/5d52fb081b3570c81e3a) (added: 2021-03-03)
* [CSS Triggers](https://csstriggers.com/) (added: 2021-03-03)
* [Performance Analysis Reference](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference) (added: 2021-03-15)

## Q & A

| Question | Answer |
| - | - |
| How should I defined performance? | Art of doing less to produce equal or better results |
| What is a frame? | Naive explanation: Single picture, in a sequence of images |
| What is rendering? | Drawing an image to the screen |
| What is FPS? | Non-linear metric to show how many renders are made per second |
| What is jank? | When frame rate drops noticeably, causing content to judder on screen, because screen updates aren't done fast enough |
| What is good FPS? | Follow device refresh rate. Normally about 16 ms (1 second / 60 frames = 16.66ms). It's really stricter because of housekeeping work the browser does, so budget is rather 10 ms |  
| What is pixel-to-screen pipeline? | The steps taken in the process of rendering updates to screen |
| What is rasterization? | TBA |
| What is a render tree? | TBA |

## Checklist

1. Run runtime performance audits in incognito mode, to not be affected by browser extensions 
2. Undock DevTools to all charts in the Performance tab
3. Simulate a slow CPU to clearly detect bottlenecks
4. Open FPS meter and check % of frames successfully rendered
5. Check CPU use in Performance Monitor (`ctrl`+`shift`+`P`: `Show Performance monitor`)
6. Run audit and looks for red lines in the FPS chart. Compare below.

Bad, CPU fully occupied: ![image](https://user-images.githubusercontent.com/2670127/111128283-01a21780-8575-11eb-987c-bf2d272cdac9.png)

Better, CPU idle now and then: ![image](https://user-images.githubusercontent.com/2670127/111128451-32824c80-8575-11eb-9b9a-6924ab1f7930.png)

7. In Summary tab, check what process occupies main thread the most.
8. In the Main panel, find activities with "red triangles", which signals events with issues. Zoom in on those.
9. Dig deeper, find which specific events are causing problems. In the `Summary`, find which code blocks affect performance.

![image](https://user-images.githubusercontent.com/2670127/111133822-3e710d00-857b-11eb-9ec3-2f9125138124.png)

10. Measure Style Recalculation Cost, by finding how many elements are affected by a style recalculation.

![image](https://user-images.githubusercontent.com/2670127/111970705-d7fc6980-8afb-11eb-899e-4d8ab1ce995b.png)

11. Compare with [What forces layout / reflow](https://gist.github.com/paulirish/5d52fb081b3570c81e3a) and [CSS Triggers](https://csstriggers.com/) to find better solution.

12. For animations or modifying DOM nodes, use tool `Show paint flashing rectangles` to verify only expected elements are painted.

## Pixel-to-Screen pipeline

The work required for visual changes. The less work, the cheaper the process. When optimizing it's preferable to do as few steps as possible in the Pixel-to-Screen pipeline.

🐢 Change element's geometry (e.g. width): JavaScript / CSS > Style > Layout > Paint > Composite

🐄 Change paint only property (e.g. background): JavaScript / CSS > Style > Paint > Composite

🐇 Change composite only property (e.g. transform): JavaScript / CSS > Style > Composite

### JavaScript / CSS trigger visual change

Adding, removing or modifying DOM elements trigger visual change. Triggered by JavaScript, CSS animations or CSS transitions.

### Style

Recalculating DOM element's styles are called **computed style calculation**. This is done in two steps:

1. The browser figures out what selectors apply to a given element
2. Take style rules for given element and assess what updated styles are

### Layout

As changes to a specific element may affect others, the browser needs to consider parent and child nodes when an element's size and position is calculated. If an element's geometry is changed, the browser needs to reflow the page.

### Paint

Painting, often the most expensive part of the pipeline, is the process of "filling in the pixels". Paint consists of two tasks:

1. Create a list of draw calls
2. Rasterization, the process of filling in/drawing visual parts of an element

### Compositing

Handle order of drawing layers, so that page renders correctly. 

## JavaScript optimization

Find the bottlenecks before optimizing; micro-optimizations do not pay off in the end!

1. 🙇 Use `requestAnimationFrame` (start of the frame) instead of `setTimeout` or `setInterval` (some point in the frame) for visual changes.
2. 💁 Avoid forced synchronous layouts ("layout thrashing"). Batch style reads together, to reuse the previous frame's layout values. Once calculations are performed, then perform writes.
3. 🎡 For large operations that require DOM access, batch work in separate `requestAnimationFrame` tasks. This may require status indicators to clearly signal a long running process.
4. 👷 Move computational work to web workers to offload main thread and prevent blocking visual updates.

## CSS optimization

1. Don't focus on CSS as a bottle neck when it comes to selectors, but prefer more specific selectors to affect as few elements as possible when style calculations are applied.

## Log

1. 2013-03-03: Find study material
1. 2013-03-04: Goal 1 defined
1. 2013-03-04: Defined Q&A section
1. 2013-03-15: Defined checklist section
1. 2013-03-15: Cleared tutorial https://www.youtube.com/watch?v=7HSkc9TLF5U and https://developers.google.com/web/tools/chrome-devtools/evaluate-performance
1. 2013-03-15: Defined next tutorial to go through 
1. 2013-03-22: Defined section to describe the pixel-to-screen pipeline
1. 2013-03-22: Added section for what to think about when optimizing JavaScript
1. 2013-03-22: Added section for CSS optimizations
1. 2013-03-22: Updated goals to use the Test Page Demo before moving on to unkown territory
1. 2013-03-22: Read https://developers.google.com/web/fundamentals/performance/rendering, https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution and https://developers.google.com/web/fundamentals/performance/rendering/reduce-the-scope-and-complexity-of-style-calculations
1. 2013-03-29: Read https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing
1. 2013-03-29: Included further study material to understand how browser rendering works

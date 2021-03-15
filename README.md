# Web Performance Optimization

Learning material for web and rendering performance optimization.

## Goals

Goal 1: A measurable performance (before/after) optimization in https://github.com/jouni-kantola/grid-snake

## Study material

* [Get Started With Analyzing Runtime Performance](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance) (added: 2013-03-03)

## Reference material

* [What forces layout / reflow](https://gist.github.com/paulirish/5d52fb081b3570c81e3a) (added: 2021-03-03)
* [CSS Triggers](https://csstriggers.com/) (added: 2021-03-03)

## Q & A

| Question | Answer |
| - | - |
| How should I defined performance? | Art of doing less to produce equal or better results |
| What is a frame? | Naive explanation: Single picture, in a sequence of images |
| What is rendering? | Drawing an image to the screen |
| What is FPS? | Non-linear metric to show how many renders are made per second |

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

## Log

1. 2013-03-03: Find study material
1. 2013-03-04: Goal 1 defined
1. 2013-03-04: Defined Q&A section
1. 2013-03-15: Defined checklist section

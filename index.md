---
layout: home
---
# Robert Houdin

Robert Houdin is a Houdini-like node evaluation framework with common tech art functions. It is designed as modular as possible - not only different steps are broken down into reusable, abstract nodes, but also the API underneath the nodes. This means you can pull out a folder (sometimes with some dependencies on other folders) from Rober Houdin and use it as a standalone library!

### Why "Robert Houdin"?

The project is named after the famous French magician, and the reason being, well, Harry _Houdini_ is also a famous and remarkable figure in the history of prestidigitation. Plus, _Houdin_ is so similar to _Houdini_ so I couldn't help it.

### Why bother doing/using this at all?

- Houdini has a 4-digit price tag.
- You can't do everything with HDA, since HDA is not the complete Houdini.
- Houdini is CPU-bound.
- Visual Scripting isn't really tooling friendly.
- Working in the industry, you would sometimes find yourself needing to do similar things again and again.

In comparison:

- This project is free.
- You can do anything as long as you can program an appropriate node. (You can write a fluid sim if you have the time budget!)
- You can run compute shaders with this project, and feel the power of GPU.
- Your artist don't need to go back and forth between Unity and DCC applications, and they always prefer so.
- With a good node design, you could achieve the level of decoupling just as behaviour trees, write once, connect the dots later.

Of course, you would be using Houdini because it has those nice convenient features, not solely because it is procedural. This project does not aim to _replace_ Houdini, but rather a fast _Houdini-like_ editing experience all inside the Unity itself, for smaller, less complex, but highly Unity-bound features.
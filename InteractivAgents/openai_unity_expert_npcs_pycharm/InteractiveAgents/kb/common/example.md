# InteractiveAgents overview (common)

## Product summary

InteractiveAgents is a Unity-friendly NPC backend for defining virtual experts,
placing them in rooms, and making them available through chat. It supports:

- configurable personas and areas of expertise;
- spawn placement based on zones and tags;
- handoffs between agents;
- a local text-file knowledge base under `kb/`.

## Intended users

- studios building interactive booths, demonstrations, or showrooms;
- agency teams creating event installations;
- product teams building support or sales demonstrations in Unity.

## Short FAQ

- **Is an internet connection required?** Yes for language-model responses; the
  knowledge base itself is local.
- **What information can be stored?** Product information, prices, contracts,
  technical specifications, and use cases.
- **Can I define my own agents?** Yes. Use JSON files under `examples/` or
  provide custom paths.


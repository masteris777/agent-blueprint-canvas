# Agent Blueprint Canvas — UI Renderer

This folder contains a tool-agnostic build specification for a browser-based YAML editor and renderer for Agent Blueprint Canvas.

The renderer is an implementation artifact built on top of the core framework. It is not part of the canonical canvas specification in [manifest.md](../../manifest.md).

## Live Apps

- GitHub Spark: https://agent-blueprint-canv--masteris777.github.app/
- Firebase: https://studio-3006097909-4815a.web.app/

## What This Tool Does

- accepts Agent Blueprint Canvas YAML input
- renders visual canvases live as you type
- exports individual canvases as PNG images

## Build From Spec

The [spec.md](spec.md) file in this folder is intentionally builder-neutral.
You can use it with GitHub Spark, Firebase Studio, v0, Bolt, or any similar AI app builder.

Recommended workflow:

1. Open your preferred AI builder.
2. Paste the contents of [spec.md](spec.md) as the project prompt.
3. Generate the React app.
4. Compare the output to the live apps above and refine until the visuals and behavior match.

## Relationship To The Main Repo

- [manifest.md](../../manifest.md) defines the canonical Agent Blueprint Canvas framework.
- [spec.md](spec.md) defines one concrete UI renderer implementation target for that framework.

Use the manifest when defining the canvas language.
Use this folder when building or regenerating the YAML-to-canvas renderer.
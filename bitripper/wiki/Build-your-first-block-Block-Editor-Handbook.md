---
title: "Tutorial: Build your first block – Block Editor Handbook"
type: "Deep Dive"
source: "https://developer.wordpress.org/block-editor/getting-started/tutorial/#updating-block-json"
created: 2026-04-17
---

# Tutorial: Build your first block – Block Editor Handbook

## 🧩 Executive Synthesis
This tutorial guides you through building a "Copyright Date Block" in WordPress, a practical block displaying the copyright symbol (©), the current year, and an optional starting year. It covers scaffolding with create-block, modifying files for dynamic and static rendering, adding attributes, block supports, and user interfaces. The process emphasizes fundamentals like block.json configuration, JavaScript for editing and saving, PHP for rendering, and handling dynamic content to prevent validation errors.

## 🗂 Detailed Breakdown (Proportional to Source)

### What you’re going to build
The block displays "© [startingYear]–[currentYear]" or just "© [currentYear]". Interact with the finished project in [WordPress Playground](https://playground.wordpress.net/?blueprint-url=https://raw.githubusercontent.com/WordPress/block-development-examples/trunk/plugins/copyright-date-block-09aac3/_playground/blueprint.json) or use the [Quick Start Guide](https://developer.wordpress.org/block-editor/getting-started/quick-start-guide/).

### Prerequisites
- Code editor
- Node.js development tools
- Local WordPress environment (e.g., wp-env)

### Scaffolding the block
Run: `npx @wordpress/create-block@latest copyright-date-block --variant=dynamic` then `cd copyright-date-block`. This creates the plugin structure. Activate in WordPress admin; insert the block to verify.

### Reviewing the files
Plugin structure includes block.json, index.js, edit.js, save.js (added later), render.php, etc. Refer to [File structure of a block](https://developer.wordpress.org/block-editor/getting-started/fundamentals/file-structure-of-a-block/).

### Initial setup
Run `npm run start` to watch for changes.

#### Updating block.json
Modify to:
- Remove icon (add custom later)
- Update description: “Display your site’s copyright date.”
- Add supports: color (text only), typography (fontSize)
- Remove editorStyle, style, viewScript

Final block.json provided in source.

#### Updating index.js
Add custom calendar SVG icon to registerBlockType.

#### Updating edit.js
Update to display "© [currentYear]". Use useBlockProps for wrapper.

#### Updating render.php
Update to echo "© [date('Y')]". Use get_block_wrapper_attributes.

#### Cleaning up
Remove unnecessary imports and files (editor.scss, style.scss, view.js). Run `npm run build`.

### Adding block attributes
Add to block.json: showStartingYear (boolean), startingYear (string).

#### Updating edit.js
Import InspectorControls, PanelBody, TextControl, ToggleControl. Add panel with ToggleControl for showStartingYear and conditional TextControl for startingYear. Update displayDate based on attributes.

#### Updating render.php
Update to include startingYear if set and shown.

### Adding static rendering
Add save.js with useBlockProps.save, returning static content.

#### Updating save.js
Use attributes to compute displayDate with fallbackCurrentYear.

#### Adding a new attribute
Add fallbackCurrentYear (string) to block.json.

#### Setting the attribute in edit.js
Import useEffect. Set fallbackCurrentYear to currentYear on load if mismatched.

#### Optimizing render.php
If fallbackCurrentYear matches current_year, use $content; else, generate dynamically.

### Wrapping up
Resources: Block Development Environment, Fundamentals, Developer Blog, Block Development Examples on GitHub.

## 🛠 Technical Implementation / Skills
- **Scaffolding**: @wordpress/create-block with --variant=dynamic
- **Registration**: registerBlockType in index.js
- **Editor**: useBlockProps, InspectorControls, components for UI
- **Attributes**: Defined in block.json, managed with setAttributes
- **Rendering**: Dynamic via PHP, static via save.js with fallback
- **Hooks**: useEffect for initialization
- **Validation Handling**: fallbackCurrentYear prevents errors on year change

## 🔗 Knowledge Graph Connections
- **Context**: [[WordPress Block Development]]
- **Related Topics**: [[Gutenberg Blocks]], [[Dynamic Rendering]], [[Static Rendering]], [[Block Attributes]], [[Block Supports]]


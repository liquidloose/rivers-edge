---
title: "Supports – Block Editor Handbook"
type: "Deep Dive"
source: "https://developer.wordpress.org/block-editor/reference-guides/block-api/block-supports/"
created: 2026-04-17
---

# Supports – Block Editor Handbook

## 🧩 Executive Synthesis
Block Supports API enables blocks to declare support for various features, adding attributes and UI controls in the block editor. It covers properties like alignment, colors, dimensions, typography, and more, with detailed subproperties and defaults. Generated properties are added to the block's wrapper via useBlockProps or get_block_wrapper_attributes.

## 🗂 Detailed Breakdown (Proportional to Source)

### allowedBlocks
- Type: boolean
- Default: false
- Adds UI to select allowed child blocks for containers. Use with useInnerBlocksProps.

### anchor
- Type: boolean
- Default: false
- Enables anchor ID field and direct link copying.

### autoRegister
- Type: boolean
- Default: false
- Auto-registers PHP-only blocks in the editor using ServerSideRender.

### align
- Type: boolean or array
- Default: false
- Enables alignment controls (left, center, right, wide, full).

### alignWide
- Type: boolean
- Default: true
- Enables wide alignment; set false to disable.

### ariaLabel
- Type: boolean
- Default: false
- Enables aria-label definition without UI.

### background
- Type: Object
- Default: null
- Subproperties: backgroundImage (boolean, false), backgroundSize (boolean, false)
- Enables background image and size controls; stores in style.background.

### className
- Type: boolean
- Default: true
- Adds default class; set false to remove.

### color
- Type: Object
- Default: null
- Subproperties: background (boolean, true), button (boolean, false), enableContrastChecker (boolean, true), gradients (boolean, false), heading (boolean, false), link (boolean, false), text (boolean, true)
- Enables various color controls; stores presets and customs in attributes.

### contentRole
- Type: boolean
- Default: false
- Marks block as content for editing modes.

### customClassName
- Type: boolean
- Default: true
- Enables custom className field; set false to disable.

### dimensions
- Type: Object
- Default: null
- Subproperties: height (boolean, false), minHeight (boolean, false), minWidth (boolean, false), width (boolean, false)
- Enables dimension controls; stores in style.dimensions.

### filter
- Type: Object
- Default: null
- Subproperties: duotone (boolean, false)
- Enables duotone filter; applies via selectors.

### html
- Type: boolean
- Default: true
- Enables HTML editing; set false to disable.

### inserter
- Type: boolean
- Default: true
- Shows in inserter; set false to hide.

### interactivity
- Type: boolean or Object
- Default: false
- Subproperties: clientNavigation (boolean, false), interactive (boolean, false)
- Indicates Interactivity API usage.

### layout
- Type: boolean or Object
- Default: null
- Subproperties: default (Object, null), allowSwitching (boolean, false), allowEditing (boolean, true), allowInheriting (boolean, true), allowSizingOnChildren (boolean, false), allowVerticalAlignment (boolean, true), allowJustification (boolean, true), allowOrientation (boolean, true), allowWrap (boolean, true), allowCustomContentAndWideSize (boolean, true)
- Configures layout types and controls for inner blocks.

### listView
- Type: boolean
- Default: false
- Enables List View panel for inner blocks.

### lock
- Type: boolean
- Default: true
- Enables lock toggle; set false to disable.

### multiple
- Type: boolean
- Default: true
- Allows multiple instances; set false for single use.

### position
- Type: Object
- Default: null
- Subproperties: sticky (boolean, false)
- Enables position controls; stores in style.position.

### renaming
- Type: boolean
- Default: true
- Enables renaming; set false to disable.

### reusable
- Type: boolean
- Default: true
- Allows conversion to reusable; set false to disable.

### shadow
- Type: boolean
- Default: false
- Enables box-shadow picker; stores in style.shadow.

### spacing
- Type: Object
- Default: null
- Subproperties: margin (boolean/array, false), padding (boolean/array, false), blockGap (boolean/array, false)
- Enables spacing controls; stores in style.spacing.

### typography
- Type: Object
- Default: null
- Subproperties: fontSize (boolean, false), lineHeight (boolean, false), textAlign (boolean/array, false)
- Enables typography controls; stores in style.typography.

### visibility
- Type: boolean
- Default: true
- Enables hiding; set false to disable.

### splitting
- For simple text blocks; enables splitting on Enter.

## 🛠 Technical Implementation / Skills
- Use useBlockProps or get_block_wrapper_attributes for properties.
- Attributes extended automatically for supported features.
- Themes declare support for UI visibility.
- Selectors for applying filters like duotone.

## 🔗 Knowledge Graph Connections
- **Context**: [[WordPress Block Development]]
- **Related Topics**: [[Block Attributes]], [[Block Supports]], [[Theme JSON]], [[Interactivity API]], [[Layout Types]]


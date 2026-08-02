---
title: EasyClick Android Docs — Mobile Automation Scripts — Node and Color Capture
hide_title: false
hide_table_of_contents: false
sidebar_label: Node Tool Panel
description: 'EasyClick mobile automation scripts — configure EasyClick node capture, phone screenshots, and color generation in IDEA'
keywords:
 - EasyClick
 - mobile automation scripts
 - automation software
 - EasyClick script node capture
 - color generation
 - node
 - png
 - img
 - src
 - androidimg
 - alt
 - style
 - zoom
 - preview
 - size
 - mobile automation
 - test automation
 - script development
---

# Node and Color Capture
 - Location: `IDEA menu bar - EasyClick Android - Node Capture`
 - <img src='/androidimg/node/node_preview.png' alt="node_preview.png" style={{zoom:'30%'}} />
 - Click the `gear` icon on the node panel and select `View Mode - Float` to detach the node panel
 - <img src='/androidimg/node/node_size.png' alt="node_size.png" style={{zoom:'30%'}} />

## Capture Nodes
 - Click the `Capture Nodes` button to fetch current screen elements. The phone must have `automation service` enabled
 - For detailed log output, check the `EasyClick Android Run Log`
 - Panel after successful node capture:
 - <img src='/androidimg/node/node_ok.png' alt="node_ok.png" style={{zoom:'30%'}} />

## Node File List
 - <img src='/androidimg/node/node_filelist.png' alt="node_filelist.png" style={{zoom:'30%'}} />
 - In the `left file list search box`, type text to search files
 - Node files are named as `current running app package name + date`
 - Right-click in the `left file list` to `refresh files`, `delete files`, or `rename files`

## Image View
 - <img src='/androidimg/node/node_img.png' alt="node_img.png" style={{zoom:'30%'}} />
 - Click the center image with the left mouse button; the system draws a red box around the clicked region and selects the matching node in the tree on the right
 - Right-click to copy color values and more

## Node Properties
 - <img src='/androidimg/node/node_tree.png' alt="node_tree.png" style={{zoom:'30%'}} />
 - In the node properties panel on the right, enter a `keyword` in the input box and use the up/down arrows to search
 - Click each node in the `tree structure` to inspect detailed properties
 - Double-click the `property and value` panel to copy; use `Ctrl+A` to copy the value
 - Hold Ctrl, select properties, then use the right-click menu to `generate node lookup code`

## Other Actions
 - `Save Nodes & Screenshot`: Save the currently open nodes and image
 - `Load Nodes & Screenshot`: Load nodes and image from the computer
 - `Single Tab Mode`: When enabled, opening a node from the file list replaces the current tab; otherwise a new window opens

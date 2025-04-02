---
title: Vue Basic Syntax
date: 2025-01-12 11:27:19
tags:
    - vue
    - syntax
    - basics
    - tutorial
    - guide
---

# Vue Basic Syntax

## Directives

- **Event Binding**: `v-on` or shorthand `@click`
- **Attribute Binding**: `v-bind` or shorthand `:attribute`
    - Examples: `:input`, `:input.enter`, `:key`
- **Two-Way Binding**: `v-model`
- **Conditional Rendering**:
    - `v-if`
    - `v-else-if`
    - `v-else`
    - `v-show`
- **List Rendering**: `v-for`
    - Example: `v-for="goal in goals"`

## Communication Between Components

- **Props**: Pass data from parent to child
- **Emits**: Send events from child to parent
# Tailwind CSS Key Classes for Web Page Structure: Study Notes

When building web page layouts with Tailwind CSS, certain utility classes are essential for controlling structure, spacing, scrolling, and responsiveness.

## **1. Layout Foundation (Flexbox & Grid)**  
These classes define how elements are arranged on the page.

| Class               | Purpose                                                                 | Example Use Case                                  |
|---------------------|-------------------------------------------------------------------------|---------------------------------------------------|
| `flex`              | Enables flexbox layout (children become flex items).                    | `<div class="flex">` (arrange children horizontally) |
| `flex-col`          | Sets flex direction to vertical (children stack top-to-bottom).         | `<div class="flex flex-col">` (vertical layout)    |
| `flex-row`          | Sets flex direction to horizontal (default, children stack left-to-right). | `<div class="flex flex-row">`                     |
| `flex-1`            | Makes a flex item grow/shrink to fill available space.                  | `<div class="flex-1">` (expands to fill space)    |
| `grid`              | Enables CSS grid layout (children align in rows/columns).               | `<div class="grid grid-cols-3">` (3-column grid)  |
| `grid-cols-{n}`     | Defines number of columns in a grid (e.g., `grid-cols-2` = 2 columns).   | `<div class="grid grid-cols-4">` (4 columns)      |
| `grid-rows-{n}`     | Defines number of rows in a grid (e.g., `grid-rows-3` = 3 rows).         | `<div class="grid grid-rows-2">` (2 rows)         |


## **2. Sizing & Dimensions**  
Control the width, height, and proportional sizing of elements.

| Class               | Purpose                                                                 | Example Use Case                                  |
|---------------------|-------------------------------------------------------------------------|---------------------------------------------------|
| `w-full`            | Sets width to 100% of parent.                                           | `<div class="w-full">` (full-width container)     |
| `w-screen`          | Sets width to 100% of viewport (screen width).                          | `<div class="w-screen">` (full browser width)     |
| `h-full`            | Sets height to 100% of parent.                                          | `<div class="h-full">` (fills parent height)      |
| `h-screen`          | Sets height to 100% of viewport (screen height).                         | `<div class="h-screen">` (full browser height)    |
| `min-h-screen`      | Sets minimum height to viewport height (prevents content from being too short). | `<main class="min-h-screen">` (page content)      |
| `max-w-{size}`      | Limits maximum width (e.g., `max-w-lg` = max width of `32rem`).          | `<div class="max-w-2xl mx-auto">` (centered content with max width) |


## **3. Spacing & Alignment**  
Manage space between elements and their alignment.

| Class               | Purpose                                                                 | Example Use Case                                  |
|---------------------|-------------------------------------------------------------------------|---------------------------------------------------|
| `mx-auto`           | Centers a block element horizontally (auto left/right margins).         | `<div class="max-w-lg mx-auto">` (centered container) |
| `items-center`      | Vertically centers flex/grid items (along cross axis).                  | `<div class="flex items-center">` (align items vertically) |
| `justify-center`    | Horizontally centers flex/grid items (along main axis).                 | `<div class="flex justify-center">` (center items horizontally) |
| `justify-between`   | Pushes flex/grid items to opposite ends (space between).                | `<nav class="flex justify-between">` (logo left, links right) |
| `gap-{size}`        | Adds space between flex/grid children (e.g., `gap-4` = 1rem).           | `<div class="flex gap-2">` (space between buttons) |
| `p-{size}`          | Padding on all sides (e.g., `p-6` = 1.5rem).                             | `<div class="p-4">` (inner spacing)               |
| `m-{size}`          | Margin on all sides (e.g., `m-6` = 1.5rem).                              | `<div class="mb-4">` (space below an element)     |


## **4. Overflow & Scrolling**  
Control how content behaves when it exceeds container boundaries.

| Class               | Purpose                                                                 | Example Use Case                                  |
|---------------------|-------------------------------------------------------------------------|---------------------------------------------------|
| `overflow-hidden`   | Hides content that overflows the container (no scrollbars).             | `<div class="overflow-hidden">` (clip excess content) |
| `overflow-y-auto`   | Shows vertical scrollbar **only when needed** (content overflows vertically). | `<div class="overflow-y-auto h-60">` (scrollable list) |
| `overflow-x-auto`   | Shows horizontal scrollbar **only when needed** (content overflows horizontally). | `<div class="overflow-x-auto">` (scrollable table on mobile) |
| `overflow-y-scroll` | Forces a vertical scrollbar (even if not needed).                       | `<div class="overflow-y-scroll h-60">` (permanent scrollbar) |
| `scrollbar-hide`    | Hides scrollbars (while keeping content scrollable) (requires Tailwind config). | `<div class="overflow-y-auto scrollbar-hide">`    |


## **5. Positioning**  
Control how elements are positioned relative to their parent or viewport.

| Class               | Purpose                                                                 | Example Use Case                                  |
|---------------------|-------------------------------------------------------------------------|---------------------------------------------------|
| `relative`          | Sets element as a positioning context for child elements.               | `<div class="relative">` (parent for absolute children) |
| `absolute`          | Positions element relative to the nearest `relative` parent.            | `<div class="absolute top-0 right-0">` (top-right corner of parent) |
| `fixed`             | Positions element relative to the viewport (stays in place on scroll).  | `<header class="fixed top-0 w-full">` (sticky header) |
| `sticky`            | Sticks element to viewport when scrolled past (hybrid of `relative` and `fixed`). | `<nav class="sticky top-0">` (sticks to top on scroll) |
| `top-{size}`, `right-{size}`, etc. | Offsets for positioned elements (e.g., `top-4` = 1rem from top). | `<div class="absolute top-2 right-2">` (2rem from top/right) |


## **6. Borders & Dividers**  
Add visual separation between elements.

| Class               | Purpose                                                                 | Example Use Case                                  |
|---------------------|-------------------------------------------------------------------------|---------------------------------------------------|
| `border`            | Adds a default border (1px solid) on all sides.                         | `<div class="border">` (basic border)             |
| `border-t`, `border-b`, etc. | Adds border to a specific side (e.g., `border-t` = top border).          | `<div class="border-t border-gray-200">` (top divider) |
| `border-{color}`    | Sets border color (e.g., `border-gray-300`, `border-blue-500`).          | `<div class="border border-red-500">` (red border) |
| `rounded`           | Adds small border radius (rounded corners).                             | `<div class="rounded">` (soft corners)            |
| `rounded-full`      | Makes corners fully rounded (circular for square elements).             | `<div class="w-8 h-8 rounded-full">` (circle)     |


## **7. Responsiveness**  
Adapt layouts to different screen sizes (mobile-first).

| Class Pattern       | Purpose                                                                 | Example Use Case                                  |
|---------------------|-------------------------------------------------------------------------|---------------------------------------------------|
| `sm:{class}`        | Applies `class` on screens ≥ 640px (small mobile).                      | `<div class="sm:flex">` (flex on small screens)   |
| `md:{class}`        | Applies `class` on screens ≥ 768px (tablet).                            | `<div class="md:grid-cols-2">` (2 columns on tablet) |
| `lg:{class}`        | Applies `class` on screens ≥ 1024px (laptop).                           | `<div class="lg:w-1/2">` (50% width on laptops)   |
| `xl:{class}`        | Applies `class` on screens ≥ 1280px (desktop).                          | `<div class="xl:gap-6">` (larger gap on desktops) |
| `hidden`, `sm:block` | Hides element by default, shows on small screens.                       | `<div class="hidden sm:block">` (mobile-hidden content) |

# DrawIt Part 2 Assignment

You shall implement the API documented [here](https://btj.github.io/drawit_part2_docs/index.html).
Note: only the elements that are new or changed since Part 1 are documented.

You shall implement classes `Extent` and `ShapeGroup` twice: once in package `drawit.shapegroups1`, and once in package `drawit.shapegroups2`. Both versions shall implement exactly the same API. They shall be different only in their internal implementation approach:
- Class `drawit.shapegroups1.Extent` shall store the `left`, `top`, `right`, and `bottom` attributes and compute the `width` and `height` attributes, whereas class `drawit.shapegroups2.Extent` shall store the `left`, `top`, `width`, and `height` attributes and compute the `right` and `bottom` attributes.
- Class `drawit.shapegroups1.ShapeGroup` shall be implemented such that method `getSubgroup` runs in constant time, i.e. its running time shall not depend on the number of subgroups directly contained by the shape group. Class `drawit.shapegroups2.ShapeGroup` shall be implemented such that methods `bringToFront` and `sendToBack` run in constant time, i.e. their running time shall not depend on the number of subgroups directly contained by the shape group's parent.

Both versions of classes `Extent` and `ShapeGroup` shall deal with illegal calls defensively. You can throw any exception you like, but of course you must document which exception you throw using a `@throws` clause. Typical exceptions thrown defensively include `IllegalArgumentException`, `IllegalStateException`, and `UnsupportedOperationException`. (The GUI catches only `IllegalArgumentException`, but this does not mean that throwing other exceptions is wrong.)

Once you have finished implementing the API, you can run the [DrawIt GUI](https://github.com/btj/drawit_part2/releases/download/1/drawitgui_part2.jar) against it. The `.jar` file contains two copies of the GUI: `drawitgui1.GUI` uses `drawit.shapegroups1`, and `drawitgui2.GUI` uses `drawit.shapegroups2`.

The GUI extends the Part 1 GUI with the following functionality:
- Shapes are filled with their current color rather than outlined.
- You can set a shape's color by selecting it and pressing `r` (red), `g` (green), or `b` (blue).
- You can put a shape inside a `ShapeGroup` by selecting it and pressing `G` (Shift-G). Then, you can mutate the `ShapeGroup`'s extent by dragging the top-left and bottom-right controls. This causes a transformed view of the shape contained by the `ShapeGroup` to be shown.
- You can create a non-leaf `ShapeGroup` by selecting two or more `ShapeGroup`s and then pressing `G` (Shift-G). You can mutate the `ShapeGroup`'s extent and effectively transform all of the shapes it contains as one unit by dragging its top-left and bottom-right controls.
- To select a child of a `ShapeGroup`, first select the `ShapeGroup` and then click inside the child.
- To bring a subgroup of a `ShapeGroup` to the front, select the subgroup and press `F` (Shift-F). To send it to the back, press `B` (Shift-B).

You shall create proper documentation for the API. Detailed instructions follow later.

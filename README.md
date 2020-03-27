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

## Notes

- For the `fill` drawing command to fill the shape defined by the preceding sequence of `line` and `arc` commands correctly, each next line or arc in the sequence should start where the preceding one finished. However, whether the shape is filled correctly is not important, so you are free to ignore this issue.

## Inner and outer coordinate systems: example

Consider the following code:
```java
RoundedPolygon triangle = new RoundedPolygon();
triangle.setVertices(new IntPoint[] {new IntPoint(10, 10), new IntPoint(30, 10), new IntPoint(20, 20)});

ShapeGroup leaf = new ShapeGroup(triangle);
assert leaf.getExtent().getTopLeft().equals(new IntPoint(10, 10)) && leaf.getExtent().getBottomRight().equals(new IntPoint(30, 20));
leaf.setExtent(Extent.ofLeftTopWidthHeight(0, 0, 20, 10));

// For simplicity, we here ignore the constraint that a non-leaf ShapeGroup shall have at least two subgroups.
ShapeGroup nonLeaf = new ShapeGroup(new ShapeGroup[] {leaf});
assert nonLeaf.getExtent().getTopLeft().equals(new IntPoint(0, 0)) && nonLeaf.getExtent().getBottomRight().equals(new IntPoint(20, 10));
nonLeaf.setExtent(Extent.ofLeftTopWidthHeight(0, 0, 10, 5));
```

`leaf` applies a translation by (-10, -10) to its contents. `nonLeaf` applies a scaling by (0.5, 0.5) to its contents. The net transformation applied to `triangle` is a translation by (-10, -10), followed by a scaling by (0.5, 0.5).

In `leaf`'s inner coordinate system, the triangle's vertices are (10, 10), (30, 10), (20, 20), and `leaf`'s extent is `Extent.ofLeftTopRightBottom(10, 10, 30, 20)`.

In `leaf`'s outer coordinate system, the triangle's vertices are (0, 0), (20, 0), (10, 10), and `leaf`'s extent is `Extent.ofLeftTopRightBottom(0, 0, 20, 10)`.

In `nonLeaf`'s inner coordinate system, the triangle's vertices are (0, 0), (20, 0), (10, 10), and `nonLeaf`'s extent is `Extent.ofLeftTopRightBottom(0, 0, 20, 10)`.

In `nonLeaf`'s outer coordinate system, the triangle's vertices are (0, 0), (10, 0), (5, 5), and `nonLeaf`'s extent is `Extent.ofLeftTopRightBottom(0, 0, 10, 5)`.

Suppose, now, that I now perform the following call:

```java
leaf.setExtent(Extent.ofLeftTopWidthHeight(1000, 2000, 20, 10));
```

After this call:
- `leaf` applies a translation by (990, 1990) to its contents. `nonLeaf` applies a scaling by (0.5, 0.5) to its contents. The net transformation applied to `triangle` is a translation by (990, 1990), followed by a scaling by (0.5, 0.5).
- In `leaf`'s inner coordinate system, the triangle's vertices are (10, 10), (30, 10), (20, 20), and `leaf`'s extent is `Extent.ofLeftTopRightBottom(10, 10, 30, 20)`.
- In `leaf`'s outer coordinate system, the triangle's vertices are (1000, 2000), (1020, 2000), (1010, 2010), `leaf`'s extent is `Extent.ofLeftTopRightBottom(1000, 2000, 1020, 2010)`, and `nonLeaf`'s extent is `Extent.ofLeftTopRightBottom(0, 0, 20, 10)`. Notice that `nonLeaf`'s extent no longer includes its subgroup's extent. The extent of a ShapeGroup G is guaranteed to contain G's subgroups' extents only at the time of creation of G.
- In `nonLeaf`'s inner coordinate system, the triangle's vertices are (1000, 2000), (1020, 2000), (1010, 2010), `leaf`'s extent is `Extent.ofLeftTopRightBottom(1000, 2000, 1020, 2010)`, and `nonLeaf`'s extent is `Extent.ofLeftTopRightBottom(0, 0, 20, 10)`. A ShapeGroup's inner coordinate system always coincides with its subgroups' outer coordinate systems.
- In `nonLeaf`'s outer coordinate system, the triangle's vertices are (500, 1000), (510, 1000), (505, 1005), `leaf`'s extent is `Extent.ofLeftTopRightBottom(500, 1000, 510, 1005)`, and `nonLeaf`'s extent is `Extent.ofLeftTopRightBottom(0, 0, 10, 5)`.

Furthermore, `leaf.toInnerCoordinates(new IntPoint(500, 1000))` returns (10, 10), and `leaf.toGlobalCoordinates(new IntPoint(10, 10))` returns (500, 1000).

# DrawIt Part 2 Assignment

You shall implement the API documented [here](https://btj.github.io/drawit_part2_docs/).
Note: only the elements that are new or changed since Part 1 are documented.

You shall implement classes `Extent` and `ShapeGroup` twice: once in package `drawit.shapegroups1`, and once in package `drawit.shapegroups2`. Both versions shall implement exactly the same API. They shall be different only in their internal implementation approach:
- Class `drawit.shapegroups1.Extent` shall store the `left`, `top`, `right`, and `bottom` attributes and compute the `width` and `height` attributes, whereas class `drawit.shapegroups2.Extent` shall store the `left`, `top`, `width`, and `height` attributes and compute the `right` and `bottom` attributes.
- Class `drawit.shapegroups1.ShapeGroup` shall be implemented such that method `getSubgroup` runs in constant time, i.e. its running time shall not depend on the number of subgroups directly contained by the shape group. Class `drawit.shapegroups2.ShapeGroup` shall be implemented such that methods `bringToFront` and `sendToBack` run in constant time, i.e. their running time shall not depend on the number of subgroups directly contained by the shape group's parent.

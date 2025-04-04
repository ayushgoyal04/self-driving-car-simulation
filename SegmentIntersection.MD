# Segment Intersection Formula

## Overview

The segment intersection formula is used to determine if two line segments intersect and, if so, to find the point of intersection. This is particularly useful in applications such as computer graphics, robotics, and self-driving cars, where detecting intersections is critical for collision detection and path planning.

## Mathematical Background

The formula is based on parametric equations of the line segments. A line segment can be represented parametrically as:

```
P(t) = A + t * (B - A), where 0 ≤ t ≤ 1
```

Here:

- `A` and `B` are the endpoints of the segment.
- `t` is a parameter that interpolates between `A` and `B`.

Similarly, for another segment:

```
Q(u) = C + u * (D - C), where 0 ≤ u ≤ 1
```

To find the intersection, we solve for `t` and `u` such that:

```
P(t) = Q(u)
```

This expands to a system of equations:

```
A.x + t * (B.x - A.x) = C.x + u * (D.x - C.x)
A.y + t * (B.y - A.y) = C.y + u * (D.y - C.y)
```

Rearranging and solving for `t` and `u` gives:

```
t = [(D.x - C.x) * (A.y - C.y) - (D.y - C.y) * (A.x - C.x)] / [(D.y - C.y) * (B.x - A.x) - (D.x - C.x) * (B.y - A.y)]
u = [(C.y - A.y) * (B.x - A.x) - (C.x - A.x) * (B.y - A.y)] / [(D.y - C.y) * (B.x - A.x) - (D.x - C.x) * (B.y - A.y)]
```

The denominator (bottom) determines if the lines are parallel. If `bottom == 0`, the lines are parallel and do not intersect.

If `0 ≤ t ≤ 1` and `0 ≤ u ≤ 1`, the segments intersect, and the intersection point can be calculated as:

```
Intersection.x = A.x + t * (B.x - A.x)
Intersection.y = A.y + t * (B.y - A.y)
```

## Why It Works

The formula leverages the parametric representation of line segments and linear algebra to determine the intersection point. By solving for `t` and `u`, we check if the intersection lies within the bounds of both segments.

## Application in Sensors for Collision Detection

In self-driving cars, sensors such as LiDAR and cameras detect obstacles by emitting rays or capturing images. The segment intersection formula can be used to:

1. **Detect Obstacles**: Simulate sensor rays as line segments and check for intersections with objects in the environment.
2. **Predict Collisions**: Use the intersection point to calculate the distance and time to collision.
3. **Path Planning**: Avoid obstacles by determining safe paths that do not intersect with detected objects.

For example, a LiDAR sensor emits rays in multiple directions. By modeling these rays as line segments, the intersection formula can determine if any ray intersects with an obstacle, enabling the car to take corrective actions.

## Code Implementation

Below is the implementation of the segment intersection formula:

```javascript
function getIntersection(A, B, C, D) {
  const tTop = (D.x - C.x) * (A.y - C.y) - (D.y - C.y) * (A.x - C.x);
  const uTop = (C.y - A.y) * (A.x - B.x) - (C.x - A.x) * (A.y - B.y);
  const bottom = (D.y - C.y) * (B.x - A.x) - (D.x - C.x) * (B.y - A.y);

  if (bottom != 0) {
    const t = tTop / bottom;
    const u = uTop / bottom;
    if (t >= 0 && t <= 1 && u >= 0 && u <= 1) {
      return {
        x: lerp(A.x, B.x, t),
        y: lerp(A.y, B.y, t),
        offset: t,
      };
    }
  }

  return null;
}

function lerp(start, end, t) {
  return start + t * (end - start);
}
```

This function checks for intersections and returns the intersection point if it exists.

## Conclusion

The segment intersection formula is a powerful tool for detecting intersections between line segments. Its mathematical foundation ensures accuracy, and its implementation is efficient for real-time applications like collision detection in self-driving cars. By integrating this formula into sensor systems, vehicles can navigate safely and avoid obstacles effectively.

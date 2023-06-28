# Rectangles and HyperRectangles

These refer to rectangles in 2, 3, or N dimensions, all specified by `origin` and `widths` fields. The N-dimensional `HyperRectangle` generalizes these same fields to N axes.

## Types of rectangles

The general rectangle may be specified by the object `HyperRectangle{N,T}` with dimension `N` and type `T`. 
### Rectangles in 2D
A two-dimensional rectangle has an `origin` (corner location) and 2-dimensional `widths` in 2D. Both such fields are 2D and contain type `T`. The example below creates a rectangle with corner at (0., 1.) and width/height (2., 3.): 

```
julia> rect = Rect(Vec(0.0, 1.0), Vec(2.0, 3.0)) # Vec is a StaticArray 
Rect2{Float64}([0.0, 1.0], [2.0, 3.0])
```

There are several alternative ways to enter the same information:

```
julia> rect = Rect2((0., 1.), (2., 3.)) # two tuples instead of vecs
Rect2{Float64}([0.0, 1.0], [2.0, 3.0])

julia> rect = Rect2(0., 1., 2., 3.)    # a list of the corner x, corner y, width x, height y
Rect2{Float64}([0.0, 1.0], [2.0, 3.0])

julia> rect = Rect(0., 1., 2., 3.)     # Rect with m arguments is (m/2)-D
Rect2{Float64}([0.0, 1.0], [2.0, 3.0])
```

There are also several aliases for different types:
```
julia> rect = Rect2f((0., 1.), (2.,3.)) # type Float32
HyperRectangle{2, Float32}(Float32[0.0, 1.0], Float32[2.0, 3.0])

julia> rect = Rect2i((0, 1), (2,3)) # type Int
HyperRectangle{2, Int64}([0, 1], [2, 3])
```

### Rectangles in 3D

This is a straightforward generalization to 3D `origin` and `width`. A rectangle with corner at (0., 1., 2.) and width (3., 4., 5.):
```
julia> rect = Rect(Vec(0., 1., 2.), Vec(3., 4., 5.)) # Vec is a StaticArray
Rect3{Float64}([0.0, 1.0, 2.0], [3.0, 4.0, 5.0])
```

There are also several alternatives:
```
julia> rect = Rect3((0., 1., 2.), (3., 4., 5.)) # two tuples instead of vecs
Rect3{Float64}([0.0, 1.0, 2.0], [3.0, 4.0, 5.0])

julia> Rect3(0.,1.,2.,3.,4.,5.)                 # a list of 3D corner and 3D width
Rect3{Float64}([0.0, 1.0, 2.0], [3.0, 4.0, 5.0])

julia> Rect(0.,1.,2.,3.,4.,5.)                  # Rect with m arguments is (m/2)-D
Rect3{Float64}([0.0, 1.0, 2.0], [3.0, 4.0, 5.0])

  julia> Rect3f((0., 1., 2.), (3., 4., 5.))     # type Float32
HyperRectangle{3, Float32}(Float32[0.0, 1.0, 2.0], Float32[3.0, 4.0, 5.0])

julia> Rect3i((0, 1, 2), (3, 4, 5))             # type Int (or just use Rect3 with integer args)
HyperRectangle{3, Int64}([0, 1, 2], [3, 4, 5])
```

### Rectangles in N-D

The standard specification is `Rect{N,T}(origin, width)` with N-dimensional `Vec`s or tuples:

```
julia> rect = Rect(Vec(0., 1., 2., 3.), Vec(4., 5., 6., 7.))
HyperRectangle{4, Float64}([0.0, 1.0, 2.0, 3.0], [4.0, 5.0, 6.0, 7.0])

julia> rect = Rect{4}(Vec(0., 1., 2., 3.), Vec(4., 5., 6., 7.)) # with explicit N=4
HyperRectangle{4, Float64}([0.0, 1.0, 2.0, 3.0], [4.0, 5.0, 6.0, 7.0])
```

`Rect{N,T}` is an alias for `HyperRectangle{N,T}`.

### Conversion of rectangles

```
julia> Rect3{Float64}(rect3)
Rect3{Float64}([0.0, 0.0, 0.0], [1.0, 1.0, 1.0])
```

### Operations on rectangles

#### Translation

Use addition or subtraction to translate the origins. These operations are not commutative.
```
julia> Rect2((0., 1.), (2., 3.)) + Vec(1,2)  # addition
Rect2{Float64}([1.0, 3.0], [2.0, 3.0])

julia> Rect2((0., 1.), (2., 3.)) - Vec(1,2)
Rect2{Float64}([-1.0, -1.0], [2.0, 3.0])
```

#### Matrix multiplication
A matrix may be multiplied by an N-D rectangl to yield a transformation or to increase the dimension.

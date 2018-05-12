# JFXRadialEffect
## Non-affine transformation effects in JavaFX: Radial transformation as an example

Using the JavaFX-effect javafx.scene.effect.DisplacementMap we want arbitrary
Node-objects to appear curved.
This is illustrated in the sketch below:
<div align="center">
<img src="parameterization.svg" alt="drawing" style="width: 500px;"/>
</div>

We start out with a Node which occupies a rectangular area (a).
After the effect has been applied, we want an output that looks similar to (b):
we construct a Node which is slightly larger than the old node to accommodate
the original Node when it is curved.

### Parameterization

We know the bounds of the original Node as well as the radius we want
to use: <img src="img/constraints.png" alt="drawing">.
We require the middle-width to stay the same
<div align="center"><img src="img/width_constraint.png" alt="drawing">,</div>

which leads to constraints on the angles
<div align="center"><img src="img/angular_constraint.png" alt="drawing">,</div>
<div align="center"><img src="img/angular_constraint_1.png" alt="drawing">.</div>

For the radius we require
<div align="center"><img src="img/height_constraint.png" alt="drawing">,</div>

which leads to
<div align="center"><img src="img/radial_constraint.png" alt="drawing">.</div>

We are now in the position to derive the bounds of our final Node.
For this, we recall the coordinate transformation for polar
coordinates
<div align="center"><img src="img/polar_coords.png" alt="drawing">,</div>

to find
<div align="center"><img src="img/image_bound_x.png" alt="drawing">,</div>
<div align="center"><img src="img/image_bound_y.png" alt="drawing">.</div>

Using these values, we can now parameterize the original and target Node in
<img src="img/r_phi.png" alt="drawing">-space:
<div align="center"><img src="img/x_parametrization.png" alt="drawing">,</div>
<div align="center"><img src="img/y_parameterization.png" alt="drawing">,</div>
<div align="center"><img src="img/xp_parametrization.png" alt="drawing">,</div>
<div align="center"><img src="img/yp_parameterization.png" alt="drawing">,</div>

which we will use to construct a displacement map.

### Displacement map

The DisplacementMap uses a FloatMap to map a pixel in the target Node
(transformed Node) to a pixel in the original node (untransformed Node) whose
color the target image will use at the given position
<div align="center"><img src="img/color_map.png" alt="drawing">,</div>

where c' is the color of the (x',y')-pixel in the target Node, c is the color
of a pixel in the original Node, <img src="img/offset.png" alt="drawing"> is
an offset and <img src="img/displacementmap.png" alt="drawing"> is the
actual displacement map.
We have seen that the target node is larger than the original node while the
displacement map can not leave the bounds of the original node.
This is why we use the geometry in the sketch below:
<div align="center"><img src="img/combined_node.svg" alt="drawing" style="width: 500px;"></div>

We construct a new node which includes the original node plus spacing to fit
the transformed geometry.
We call the area bounded by <img src="img/sample_bounds.png" alt="drawing">
the sample and the area bounded by <img src="img/target_bounds.png" alt="drawing">
the target.
For our problem we get the map
<div align="center"><img src="img/xmap.png" alt="drawing">,</div>
<div align="center"><img src="img/ymap.png" alt="drawing">.</div>

We now loop over <img src="img/r_phi.png" alt="drawing">, where <img src="img/rbounds.png" alt="drawing">
and <img src="img/phibounds.png" alt="drawing"> and fill the map.

Care needs to be taken in terms of discretization.
We want at least one entry in our map for each pixel which means we need to
require <img src="img/discretization_req.png" alt="drawing">, which leads to
<div align="center"><img src="img/angular_disc.png" alt="drawing">,</div>

for the angular part.

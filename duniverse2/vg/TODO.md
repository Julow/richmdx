# Current backend improvements

* git grep FIXME
* SVG renderer, try to handle more glyph cuts.
* SVG renderer, give the opportunity to use SVG glyphs ? If no font
  resolver is provided, do it like now otherwise embed glyphs.
* Canvas, try to handle more glyph cuts, transform mess.
* PDF, implement stream compression. LZW would be easiest, deflate
  would be nice.
* PDF, implement gradients with alpha, have a look at how Inkscape
  encodes them. 
* PDF, implement font subsetting.
* PDF, text extraction, consider also using /ToUnicode maps instead of 
  solely /ActualText.
  http://lists.cairographics.org/archives/cairo/2007-September/011427.html
  
# Raster image primitive

val Image.raster : Gg.box2 -> Gg.raster -> image

* Accept directly a Gg.raster or create a proxy resolved by backends
  like for fonts ? Problem is source is quite different e.g. 
  in html canvas. Gg's raster formats can be plentiful, normalize or 
  mandate a few formats ? 
* Q: will js_of_ocaml support bigarrays over typed arrays once ? 
* Potentially add this comment under Remarks and tips 
  {- Images are said to be immutable. This is only true if you 
     don't change the samples of raster images given to {!I.raster}.}

# Path convenience

Quite a few convience operations on paths could be added. This would
be only at the Vg level, renderers wouldn't need to be extended. But
does it really belong in Vg ? Tension between general computational
geometry lib and rendering lib. However quite a few of these things
could be used by a potential rasterizer. 

* P.mem : area -> path -> p2 -> bool
* P.length : path -> float (* arc length *)
* P.split : path -> float -> path * path (* split at given distance on path *)
* P.cubic_fold
* P.square 
* Boolean operations on paths
* Minkowski sum
* The folds that are [`test/attic_path.ml`](test/attic_path)

I'm more and more convinced that this doesn't belong in Vg though, the
folds in `attic_path.ml` also show that this can be provided as
an external component with minimal reliance on `Vg.Vgr.Private`. 

# Blending groups and operators 

Support more blending operators, but is it really that used in
practice ? Support group opacity, that would be really useful, however
HTML canvas doesn't support it.

From an API point of view it's just a matter of adding the following 
two optional parameters to I.blend:

I.blend : ?a:float -> ?blender:blender -> image -> image -> image 

* http://cairographics.org/operators/
* http://lists.cairographics.org/archives/cairo/2008-October/015362.html
* http://www.w3.org/TR/compositing/

# Software rasterizer and OpenGL backend 

* http://processingjs.nihongoresources.com/bezierinfo/
* http://www.codeproject.com/Articles/226569/Drawing-polylines-by-tessellation
* http://portal.acm.org/citation.cfm?id=129906 
* http://books.google.com/books?q=vatti+clipping+agoston
* http://www.antigrain.com/research/adaptive_bezier/index.html

# Db images ideas 

* Test miànjï 面积 (area, surface area), vector.
* Test font http://www.twardoch.com/fonts/ Nadeyezhda 
* Quadratic paths.
* Test degenerate color stops discarding.
* Test degenerate subpaths rendering. 
* Dash offset is for start of each subpath, negative. 
* Primitives, special cases axial with p = p', radial with c = c'. 
* Test geometric primitives, quadric and ellipse for pdf renderer.
* The IEEE 754 double floating point line vs the real line.
* The IEEE 754 double floating point grid vs the real plane
* How many doubles between 10 and 11, 100 and 101, 1000 and 1001, etc.
  or 2 ** n and 2 ** n+1.
* Rectangle, area cut of w = 0 or s = 0 is nothing but outline cut
  is segment.

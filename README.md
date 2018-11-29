# BezierCurveEditor

This repo is a fork of [BezierCurveEditor by Arkham Interactive from Unity Asset Store](https://assetstore.unity.com/packages/tools/bezier-curve-editor-11278).

It differs from the version on the Asset Store - the first commit is the code by the original author, the rest are customizations made in the last couple of years we've been using the code. Those are mostly additions to existing functionality.

# Optimization

Add `BEZIER_POINT_NO_UPDATE` to Scripting Define Symbols to disable the Update method in BezierPoint class, which can boost performance in play mode when there are many bezier points in the scene. Note that if you move bezier points in play mode, you'll have to call `SetDirty()` on the curve manually when this optimization is enabled.


# Breaking changes

If you've used the Asset Store version and plan to switch to this one in existing project, you'll have to account for the following changes.


### Curve resolution behavior

Summary:

* Curve interpolation is now more homogenous across segments on same curve
* Curves in existing projects will change in appearance, and might impact performance until you update the values
* An automatic resolution recalculation will be done when Unity encounters old curves (on scene open, prefab instantiation, play, etc.)
* You might have to tweak the values on existing curves to desired precision
* You might have to update your code to revise how you calculate the resolution value

This change makes sure that all segments have approximately the same resolution - with resolution now meaning the same number of interpolated points **per unit of distance** across the whole curve.

Previously, a curve whose segments are very different in length (e.g. a curve made of 3 points, where one segment is short and one very long) would have all segments interpolated with the same number of points, causing short segments to be interpolated too densely, and long segments to be insufficiently precise, with visible poly-line shape.

Automatic curve resolution recalculation will be done when Unity encounters old curves (on scene open, prefab instantiation, play, etc.), which will update the resolution value by dividing the old resolution by the length of the shortest curve segment. This will cause the longer segments have more interpolated points than before, but will prevent loss of precision on the shortest segment.

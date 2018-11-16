# BezierCurveEditor

This repo is a fork of [BezierCurveEditor by Arkham Interactive from Unity Asset Store](https://assetstore.unity.com/packages/tools/bezier-curve-editor-11278).

It differs from the version on the Asset Store - the first commit is the code by the original author, the rest are customizations made in the last couple of years we've been using the code. Those are mostly additions to existing functionality.


# Breaking changes

If you've used the Asset Store version and plan to switch to this one in existing project, you'll have to account for the following changes.


### Curve resolution behavior

Summary:

* Curve interpolation is now more homogenous across segments on same curve
* Curves in existing projects will change in appearance, and might impact performance until you update the values
* You'll have to tweak the values on existing curves to desired precision
* You might have to update your code to revise how you calculate the resolution value

This change makes sure that all segments have approximately the same resolution - with resolution now meaning the same number of interpolated points **per unit of distance** across the whole curve.

Previously, a curve whose segments are very different in length (e.g. a curve made of 3 points, where one segment is short and one very long) would have all segments interpolated with the same number of points, causing short segments to be interpolated too densely, and long segments to be insufficiently precise, with visible poly-line shape.

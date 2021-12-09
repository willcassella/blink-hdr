# Explainer for `video-` prefixed media features

`video-` prefixed media features give webpages the ability to disambiguate media queries for devices which have different display characteristics for video and non-video web content.

## Motivation

While CSS media queries do an adequate job of describing the characteristics of the graphics output medium for a webpage, some user-agents have different rendering paths for different types of web content, with different characteristics and capabilities for each path.

The case we're concerned with here are smart TVs and dongles, as many render video and non-video content ("graphics") onto separate "planes"(referred to as *bi-plane* rendering). Many of these devices are capable of wider color gamuts, luminance ranges, and resolution, for their video plane than their graphics plane, hence it's critical to be able to disambiguate the target plane when querying these properties.

This ambiguity is prevalent in many smart TVs, and is unlikely to improve over time.

## Current Design

Specific, relevant media features such as `color-gamut`, `dynamic-range`, and `resolution` will have `video-` prefixed variants added which accept the same query parameters as their un-prefixed counterparts. User agents on bi-plane devices will run the `video-` queries against the properties of the video plane instead of the graphics plane, and user agents running on single-plane devices will treat both variants equivalently.

The full set of features under consideration is:

- `color-gamut` -> `video-color-gamut`
- `dynamic-range` -> `video-dynamic-range`
- `resolution` -> `video-resolution`
- `width` -> `video-width`
- `height` -> `video-height`

## Goals

Ultimately, the goal is to enable web developers to serve content appropriate for each plane on such devices, without introducing hardware-specific workarounds or inadvertantly under-estimating the capabilities of single-plane devices. This bi-plane abstraction applies to many different smart TVs and dongles, and is unlikely to become obsolete any time soon.

## Non-goals

- Introduce features for devices which have more planes beyond "video" and "graphics"
- Add `video-` variants to every media feature. Only features which commonly differ between the two planes, and are useful to be able to distinguish between the two, should be added.

## Other designs considered

One option raised had been to introduce a new [media-type](https://www.w3.org/TR/CSS21/media.html#media-types), "Video", however this was disfavored because it could lead to cases where webpages *only* provide HDR or high-resolution media to devices with a video plane, which goes against our goal.

Another consideration had been to choose the highest-capability plane when resolving the standard set of features, however this could lead to cases where the less-capable plane is unable to adequately render the content it's been provided, leading to a degredated user experience and again going against our goals.

The discussion surrounding these considerations can be viewed [here](https://github.com/w3c/csswg-drafts/issues/4471).

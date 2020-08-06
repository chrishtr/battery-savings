# Explainer: web page settings to save battery

## Summary

Saving battery or CPU is important for computing devices that are not connected to an electrical source, or to better share commong CPU & GPU resources among running processes.

Web sites that are known to have high CPU or battery costs may want to request that the UA optimize for CPU or battery, even if the user has not requested it.

Most modern OSes also have battery saving features that kick in either when the battery is low or the user wishes to save battery. Ideally web sites should be able to respect these settings. Sites may wish to advise the UA on which strategies work best for the site in these situations.

## Example usage

If a site wanted to allow reduced framerate if it saved battery, they would add this to their page:

```
<meta name="battery-savings" content="allow-reduced-framerate">
```

If a site wanted to allow generic slowdown of script execution:

```
<meta name="battery-savings" content="allow-reduced-script-speed">
```

If a site wished to change behavior based on battery-savings settings, they could use a CSS media query:

```
@prefer-reduced-framerate {
* {
  animation: none !important; /* or change animations to use keyframes optimized for battery-savings */
}

```

## Real-world use cases

* Video or video conferencing sites are CPU heavy and often run for a very long time, thus making them good candidates for saving battery use even if the battery is currently not close to empty (or even if the device is plugged in, to avoid hogging the CPU).

* If a device is not yet low on battery, but the user wishes to go into a battery savings mode, sites may wish to participate, along with the User Agent, on which strategies apply best to that site without ruining the site's UX.

## Possible implementation techniques

User Agents might implement `allow-reduced-framerate` by reducing the rate of [animation frame](https://html.spec.whatwg.org/multipage/webappapis.html#update-the-rendering) generation generically, or if a video is playing, matching animation framerate to the observed video playback rate. These techniques can improve CPU or battery usage by reducing compositing, raster and/or GPU usage.

User Agents might implement `allow-reduced-script-speed` by placing the execution thread or process on a [little core](https://en.wikipedia.org/wiki/ARM_big.LITTLE) rather than a big core.


## Details

Similar to features such as [color schemes](https://drafts.csswg.org/css-color-adjust/#color-scheme-meta), the full feature would have the following components:

* A `meta` tag that allows sites to indicate preferred modes.
* Media queries allowing sites to adjust their style sheets according to battery settings.
* Spec text saying that if the user or OS has gone into a battery savings mode, then the UA should apply one or more of the battery savings to sites.
* Spec text saying that UAs should respect the `meta` tag on a site unless it conflicts with user or OS settings.

## Alternatives considered

* Expose a new API to request a specific framerate. This solves some of the use cases for particular sites, but is not forward compatible to future devices, and can be hard if the optimal frame rate depends on device class.



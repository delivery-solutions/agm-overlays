# @deliverysolutions/agm-overlays
Custom marker overlay for the @grupo-san-cristobal/agm-core package

## Version Compatibility

| Library Version | Angular Version |
| --- | --- |
| `2.0.0` | Angular 22 |
| `1.0.0` | Angular 15 |

**Version `2.0.0` is for Angular 22 applications.** The upgrade preserves the
`AgmOverlays` NgModule API, `agm-overlay` selector, inputs, outputs, and existing
overlay logic. This does not certify the existing lifecycle logic or upstream
AGM packages as free of runtime issues; build and browser verification are
required before release.

## Angular 22 Upgrade

The development toolchain uses Angular `22.1.7`, CLI/build tooling `22.1.8`,
ng-packagr `22.1.0`, TypeScript `6.0.3`, and RxJS `7.8.2`. Use a supported Node
version: `^22.22.3`, `^24.15.0`, or `>=26.0.0`.

The library is packaged in Angular Package Format with partial compilation.
The example retains NgModule bootstrapping and zone-based change detection.

### AGM 20 Compatibility Trial

The library and example now use `@grupo-san-cristobal/agm-core@20.0.2` and
`@grupo-san-cristobal/agm-markerclusterer@20.0.2`. The consuming app also uses
`@grupo-san-cristobal/agm-snazzy-info-window@20.0.2`; that package is not needed
by this library or example.

All three AGM packages declare Angular `^20.3.9` peers, not Angular 22 support.
Using them with Angular 22 is a compatibility trial requiring build and browser
verification. The consuming app's existing pnpm `peerDependencyRules.allowedVersions`
permits the Angular peer mismatch; it does not establish runtime compatibility.
If validation fails due to an upgrade-related incompatibility, compatible
upstream releases or coordinated AGM forks may still be needed.

For temporary local verification only, npm's `--legacy-peer-deps` flag can
bypass those peer conflicts. It is not a compatibility fix:

```bash
cd ds-agm-overlays-angular22
npm install --legacy-peer-deps
npm run build:dist:js
npm run update:dist
npm run build:example
```

After resolving upstream peers, use `npm install` without the bypass.
`npm run build` runs the three build/package steps above. Build checks and browser testing
are still required before release. Verify overlay placement, clicks and info
windows, coordinate/z-index changes, bounds sizing, clustering, and map teardown
in the consuming app. The example requires an authorized Google Maps API key.

Publish the generated `dist` package, not the private workspace root. After
publishing `2.0.0`, update the consuming application's dependency from `1.0.0`
to `2.0.0`; the consumer is not switched to an unpublished version here.

[demo page](https://ackerapple.github.io/agm-overlays/)

[![hire me](https://ackerapple.github.io/resume/assets/images/hire-me-badge.svg)](https://ackerapple.github.io/resume/)
[![npm version](https://badge.fury.io/js/agm-overlays.svg)](http://badge.fury.io/js/agm-overlays)
[![npm downloads](https://img.shields.io/npm/dm/agm-overlays.svg)](https://npmjs.org/agm-overlays)
[![Dependency Status](https://david-dm.org/ackerapple/agm-overlays.svg)](https://david-dm.org/ackerapple/agm-overlays)

> Source Repository : [master branch here](https://github.com/AckerApple/agm-overlays/tree/master)

<details>
  <summary>Table of Contents</summary>

- [Version Compatibility](#version-compatibility)
- [Angular 22 Upgrade](#angular-22-upgrade)
- [Dependencies](#dependencies)
- [Install](#install)
- [Import](#import)
- [Usage](#usage)
  - [Zoom Sizing](#zoom-sizing)
- [Clustering Demo](#clustering-demo)
- [Resources](#resources)
- [Credits and Collaborators](#credits-and-collaborators)
- [Also Try](#also-try)

</details>

# Dependencies

Please be sure you have installed:
- [@grupo-san-cristobal/agm-core](https://www.npmjs.com/package/@grupo-san-cristobal/agm-core) `^20.0.2` (see the compatibility trial above)
- [@angular/common](https://www.npmjs.com/package/@angular/common) `22.x`
- [@angular/core](https://www.npmjs.com/package/@angular/core) `22.x`
- [rxjs](https://www.npmjs.com/package/rxjs) `^6.5.3` or `^7.4.0`

# Install
Open a command terminal and type the following
```bash
npm install @deliverysolutions/agm-overlays@2.0.0
```

# Import
```typescript
import { AgmOverlays } from "@deliverysolutions/agm-overlays"
import { AgmCoreModule } from "@grupo-san-cristobal/agm-core"
import { NgModule } from "@angular/core"
import { BrowserModule } from '@angular/platform-browser'

@NgModule({
  imports:[
    BrowserModule,
    AgmOverlays,
    AgmCoreModule.forRoot({
      apiKey: '...your-key-here...'
    })
  ]
}) export class AppModule {}
```

# Usage
**Multiple Custom Overlays**
```html
<agm-map style="height:300px;display:block;">
  <agm-overlay
    *ngFor      = "let item of latLngArray"
    [latitude]  = "item.latitude"
    [longitude] = "item.longitude"
  >
    <!-- blue html square -->
    <div style="width:15px;height:15px;background-color:blue;"></div>
  </agm-overlay>
</agm-map>
```
> With multiple custom overlays, the zoom is auto set by the bounds calculated amongst all custom overlays

**Single Custom Overlay**
```html
<agm-map
  [zoom] = "12"
  style  = "height:300px;display:block;"
  [latitude]  = "item.latitude"
  [longitude] = "item.longitude"
>
  <agm-overlay
    [latitude]  = "item.latitude"
    [longitude] = "item.longitude"
  >
    <!-- blue html square -->
    <div style="width:15px;height:15px;background-color:blue;"></div>
  </agm-overlay>
</agm-map>
```

### Zoom Sizing
By default, markers are always the same size regardless of zoom. Change that!

> The following example expands the latitude(0.003) and the longitude(0.0052) in both directions

```html
<agm-map
  [zoom] = "12"
  style  = "height:300px;display:block;"
  [latitude]  = "item.latitude"
  [longitude] = "item.longitude"
>
  <agm-overlay
    [latitude]  = "item.latitude"
    [longitude] = "item.longitude"
    [bounds] = "{x:{latitude:-0.003,longitude:-0.0052},y:{latitude:0.003,longitude:0.0052}}"
  >
    <!-- blue html square -->
    <div style="width:15px;height:15px;background-color:blue;"></div>
  </agm-overlay>
</agm-map>
```


# Clustering Demo
Clustering is NOT a responsibility of this package, however it can be done

[demo page](https://ackerapple.github.io/agm-overlays/)
This demo uses [@agm/js-marker-clusterer](https://www.npmjs.com/package/@agm/js-marker-clusterer) to demonstrate how to do clustering

```html
<agm-map
  [latitude]  = "latLngArray[0].latitude"
  [longitude] = "latLngArray[1].latitude"
>
  <agm-marker-cluster imagePath="https://raw.githubusercontent.com/googlemaps/v3-utility-library/master/markerclustererplus/images/m">
    <agm-overlay
      *ngFor      = "let item of latLngArray;let i=index"
      [latitude]  = "item.latitude"
      [longitude] = "item.longitude"
    >
      <!-- blue html square -->
      <div class="block">
        <strong style="color:white;">{{i}}</strong>
      </div>
    </agm-overlay>
  </agm-marker-cluster>
</agm-map>
```


# Resources
- [@agm/core](https://www.npmjs.com/package/@agm/core) installed

# Credits and Collaborators
- [Acker Apple](https://github.com/AckerApple)
- [Todd Greenberg](https://github.com/tsgreenberg1217)

# Also Try
- [ack-angular-webcam](https://www.npmjs.com/package/ack-angular-webcam)
- [ack-angular-fx](https://www.npmjs.com/package/ack-angular-fx)
- [angular-file](https://www.npmjs.com/package/angular-file)
- [ack-angular](https://www.npmjs.com/package/ack-angular)

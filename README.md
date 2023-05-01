Adivery JavaScript SDK
======================================================================

Official JavaScript SDK for serving [Adivery](https://adivery.io/docs) ads in browser.

- [Installation](#installation)
- [Usage](#usage)

## Installation

### Browser (manually via script tag)

```html
<script src="/path/to/dist/adivery.global.js"></script>
```

_OR if you are using ES modules:_
```html
<script type="module">
    import * as Adivery from '/path/to/dist/adivery.mjs'
    ...
</script>
```

### Node.js (via npm)

```sh
npm install adivery-js --save
```

```js
// Using ES modules (default)
import * as Adivery from 'adivery-js';

// OR if you are using CommonJS modules
const Adivery = require('adivery-js')
```

### Example usage

#### Initialization

```js
Adivery.configure("PUT_YOUR_APP_ID_HERE");
```

#### Native ads
```js
Adivery.requestNativeAd("PUT_YOUR_PLACEMENT_ID_HERE").then((ad) => {
  const $ = (id) => document.getElementById(id);
  
  $("headline").innerText = ad.headline;
  $("description").innerText = ad.description;
  $("advertiser").innerText = ad.advertiser;
  $("image").src = ad.image;
  $("icon").src = ad.icon;
  $("call-to-action").innerText = ad.callToAction;
  $("call-to-action").onclick = () => {
    ad.recordClick();
  };

  ad.recordImpression();
});

```

#### Interstitial ads

```js
Adivery.requestInterstitialAd("PUT_YOUR_PLACEMENT_ID_HERE").then(
  (ad) => {
    console.log("Interstitial ad loaded");
    ad.show().then(
      () => {
        console.log("Interstitial ad displayed");
      },
      (err) => {
        console.error("Failed to display insterstitial ad", err);
      }
    );
  },
  (err) => {
    console.error("Failed to load interstitial ad", err);
  }
);
```

#### Rewarded ads

```js
Adivery.requestRewardedAd("PUT_YOUR_PLACEMENT_ID_HERE").then(
  (ad) => {
    console.log("Rewarded ad loaded");
    ad.show().then(
      (isRewarded) => {
        if (isRewarded) {
          console.log("Rewarded ad watched completely");
        } else {
          console.log("Rewarded ad closed without reward");
        }
      },
      (err) => {
        console.error("Failed to display rewarded ad", err);
      }
    );
  },
  (err) => {
    console.error("Failed to load rewarded ad", err);
  }
);
```

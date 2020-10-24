# Open Mini Games Format

The Open Mini Games (OMG) format is a proposal to define a set of combined minimum requirements that game developers can implement when developing a game that is playable in a web browser or a client which renders HTML5 content.

The aim of the requirements is to provide the information necessary to surface a web game in a standardized manner on discovery platforms such as browsers but also embedded platforms.

In theory, the combination of these requirements will enable catalogs of web games to be collected, filtered and surfaced to end users.

The final representations of the games should be meaningingful concise to end users and showcase the games in a manner that users have become familiar with when browsing general game catalog interfaces - such as Steam, Google Play or the Apple App Store.

## User Experience Goals

The goal of each piece of metadata is to unlock a feature or improvement towards one of the following areas:
- Discovery
- Rediscovery
- Installability
- Load Time
- Offline Access

Different platforms which wish to surface HTML5 game content can leverage these metadata towards the above goals. Ideally each piece of metadata should be useful in its own right. When combined, they aim to reduce friction and enable new capabilities for game platforms.

## Requirements

An HTML document that is also a playable web game must have the following three pieces of metadata included on its page:
- VideoGame Schema Markup with additional constraints.
- Progressive Web App Manifest.
- OMG Resources Manifest: A list of the resources necessary for offline play or initial precaching.

To be a completely valid OMG, a webpage must include all 3 pieces of metadata. However, surfacing platforms can leverage individual pieces of metadata for their specific use cases.

### VideoGame Schema Markup for Web Games

#### Explainer

The VideoGame Schema Markup (with web game property constraints) provides a surfacing platform with the information necessary to describe the game effectively as a search result or a featured item on a store page. Theoretically, a surfacing platform could also provide users with a “recently played” user journey in which users could quickly jump back into their most recently played web games based on the existence of the games in the user’s browser or navigation history. This metadata helps with ‘Discovery’ and ‘Rediscovery’ of content.

#### Example

In index.html within HEAD tag:

~~~html
<head>
  <script type="application/ld+json">
    {
      "@context": "https://schema.org",
      "@type": "VideoGame",
      "mainEntityOfPage": {
        "@type": "WebPage",
        "@id": "https://google.com/videogame"
      },
      "name": "Game title",
      "description": "Game description",
      "url": "https://example.com/game.html",
      "genre": "action",
      "accessibilityControl": "touch",
      "operatingSystem": "web",
      "icon": "https://example.com/icon/icon_1024x1024.jpg",
      "gameBanner": "https://example.com/landscape_banner.jpg",
      "about": "https://example.com/about.html",
      "privacyPolicyURL": "https://example.com/privacy.html",
      “gameExecutionMode”: “clientside”,
      "image": [
        "https://example.com/screenshot/1.jpg",
        "https://example.com/screenshot/2.jpg",
        "https://example.com/screenshot/3.jpg"
       ],
      "author": {
        "@type": "Organization",
        "name": "Game Developer",
        "logo": {
          "@type": "ImageObject",
          "url": "https://example.com/developer_logo_1024x1024.jpg"
        }
      },
       "publisher": {
        "@type": "Organization",
        "name": "Game Publisher",
        "logo": {
          "@type": "ImageObject",
          "url": "https://example.com/publisher_logo_1024x1024.jpg"
        }
      }
    }
    </script>
  ...
~~~

Relevant docs:
https://schema.org/VideoGame 

#### Additional constraints:
- “genre" must be written in lowercase, and be one or two of the following predefined genres. To represent two genres, use a pipe character, for example “arcade|racing":
    - Action
    - Arcade
    - RPG
    - Adventure
    - Board
    - Card
    - Family
    - Music
    - Educational
    - Racing
    - Simulation
    - Strategy
    - Trivia
    - Word
- “operatingSystem" must be “web"
- “icon" must refer to an image in either PNG, JPG or WEBP format with a minimum width of 1024 pixels, a minimum height of 1024 pixels and a 1:1 aspect ratio.
- “gameBanner" must refer to an image in either PNG, JPG or WEBP format with a minimum height of 1024 pixels and an aspect ratio of 4:3
- “accessibilityControl" must be written in lowercase, and be one 
- “gameExecutionMode” must be written in lowercase and be either ‘clientside’ or ‘serverside’. ‘clientside’ refers to games that execute their game code in JavaScript on the client. ‘serverside’ refers to games that execute their game code on a remote server and stream their graphics and audio to the client.

### Progressive Web App Manifest

#### Explainer

The Progressive Web App Manifest is already used by platforms such as Android to enable web applications and web games to be installed to the device and easily launched via the Android home screen. This metadata serves as a form of ‘Rediscovery’ and ‘Installability’.

#### Example

In index.html within HEAD tag:

~~~html
<head>
  <link rel="manifest" href="/manifest.webmanifest">
  ...
~~~

Example manifest.webmanifest:

~~~json
{
  "name": "Game title",
  "short_name": "Game title",
  "start_url": ".",
  "display": "standalone",
  "background_color": "#fff",
  "description": "Game description",
  "icons": [{
    "src": "images/touch/homescreen48.png",
    "sizes": "48x48",
    "type": "image/png"
  }, {
    "src": "images/touch/homescreen72.png",
    "sizes": "72x72",
    "type": "image/png"
  }, {
    "src": "images/touch/homescreen96.png",
    "sizes": "96x96",
    "type": "image/png"
  }, {
    "src": "images/touch/homescreen144.png",
    "sizes": "144x144",
    "type": "image/png"
  }, {
    "src": "images/touch/homescreen168.png",
    "sizes": "168x168",
    "type": "image/png"
  }, {
    "src": "images/touch/homescreen192.png",
    "sizes": "192x192",
    "type": "image/png"
  }],
  "related_applications": [{
    "platform": "play",
    "url": "https://play.google.com/store/apps/details?id=cheeaun.hackerweb"
  }]
}
~~~

Relevant docs:
https://developer.mozilla.org/en-US/docs/Web/Manifest 

#### Additional constraints:
- If an OMG implements both the VideoGame Schema Markup and the PWA manifest, then the icon property of the VideoGame Schema Markup must match one of the URLs of the PWA manifest icons array.

### OMG Resources Manifest

#### Explainer

The Web Bundle Resources Manifest provides an explicit list of all the resources necessary for a web game to be played offline. Extending this proposal is the OMG Resources Manifest which can be referenced as a separate file from a webpage's meta data to indicate that it is ready for offline bundling. This would communicate that the game can be bundled with a set of resources for playing offline or that resources servce as an ideal set that could be precached to improve load times.

Enables surfacing platforms and clients to pre-cache resources of games discovered on the open web for offline playback or improved loading times. This metadata assists with ‘Load Time’ and ‘Offline Access’.

#### Example

In index.html within HEAD tag:

~~~html
<head>
  <link rel="bundle-manifest" href="/files.txt" offline-capable=”true” hash=”(hash of the .wbn)”>
  ...
~~~

If this bundle of files enables a web game to be completely playable offline then the optional flag “offline-capable” property can be included. If the “offline-capable” property is not included, the surfacing platform should assume these files are merely recommended for the purpose of precaching.

Example files.txt:

~~~bash
# A line starting with '#' is a comment.
https://example.com/
https://example.com/manifest.webmanifest
https://example.com/style.css
https://example.com/script.js
~~~

Relevant docs:
https://github.com/WICG/webpackage/tree/master/go/bundle#from-a-url-list


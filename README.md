# spickzettel-jitsi-meet

## housekeeping

```bash
docker run -it -p 8080:8080 -v "/home/trapapa/playground/Spickzettel-jitsi-meet:/home/coder/project" -u "$(id -u):$(id -g)" codercom/code-server:latest
git config --global user.name "Mathias Stadler"
git config --global user.EMAIL "email@mathias-stadler.de"
# https://help.github.com/en/github/using-git/caching-your-github-password-in-git
git config --global credential.helper 'cache --timeout=3600'
```

## downlaod docker version

```bash
cd
mkdir -p playground && $_
git clone https://github.com/jitsi/docker-jitsi-meet.git
cd docker-jitsi-meet
```

## docker run

- folow https://github.com/jitsi/docker-jitsi-meet#quick-start

## config files for docker container

```bash
ls -l $CONFIG/.jitsi-meet-cfg
```

- variable $CONFIG is define in .env


## change resolution / framerate / format for 

- edit in $CONFIG/.jitsi-meet-cfg/web/config.js
- sample for 8 frame per second 360 x 180 save some bandwidth
-  form here @TODO found link

```javascript
// Video

    // Sets the preferred resolution (height) for local video. Defaults to 720.
    resolution: 180,
    maxFps: 8,


    // w3c spec-compliant video constraints to use for video capture. Currently
    // used by browsers that return true from lib-jitsi-meet's
    // util#browser#usesNewGumFlow. The constraints are independent from
    // this config's resolution value. Defaults to requesting an ideal
    // resolution of 720p.
     constraints: {
         video: {
        frameRate: {
        max: 8
                },
             height: {
                 ideal: 180,
                 max: 180,
                 min: 180
             }
         }
     },

```

## 

- edit in $CONFIG/.jitsi-meet-cfg/web/config.js
- found here https://www.kuketz-blog.de/jitsi-meet-server-einstellungen-fuer-einen-datenschutzfreundlichen-betrieb/
- explain here https://community.jitsi.org/t/what-exactly-is-the-effect-of-channellastn/25244
- save bandwidth 

```javascript
channelLastN: -1 = show all participant videos
channelLastN: 4 = show videos of last 4 active speakers only although everyone has video enabled
```


```text
// Default value for the channel "last N" attribute. -1 for unlimited.
    //channelLastN: -1,
    channelLastN: 4,
```

## use german STUN and TURN server

- edit in $CONFIG/.jitsi-meet-cfg/web/config.js
- found here https://www.kuketz-blog.de/jitsi-meet-server-einstellungen-fuer-einen-datenschutzfreundlichen-betrieb/
- found here too https://blog.wydler.eu/2020/03/22/einrichten-von-jitsi-meet-kostenlose-videokonferenzen-fuer-alle/
- STUN server explanation https://de.wikipedia.org/wiki/Session_Traversal_Utilities_for_NAT
- TURN server explanation https://en.wikipedia.org/wiki/Traversal_Using_Relays_around_NAT

```javascript
// Use XEP-0215 to fetch STUN and TURN servers.
    // useStunTurn: true,
    useStunTurn: true,

// and

p2p: {
        // Enables peer to peer mode. When enabled the system will try to
        // establish a direct connection when there are exactly 2 participants
        // in the room. If that succeeds the conference will stop sending data
        // through the JVB and use the peer to peer connection instead. When a
        // 3rd participant joins the conference will be moved back to the JVB
        // connection.
        enabled: true,

        // Use XEP-0215 to fetch STUN and TURN servers.
        // useStunTurn: true,
        useStunTurn: true,

        // The STUN servers that will be used in the peer to peer connections
        stunServers: [

            // { urls: 'stun:meet.jitsi:4446' },
            { urls: 'stun.1und1.de:3478' },
            { urls: 'stun.t-online.de:3478' },
            { urls: 'stun.nextcloud.com:443' }
        ],
```

## disable Third Party Requests

- edit in $CONFIG/.jitsi-meet-cfg/web/config.js
- uncomment
- found here https://www.kuketz-blog.de/jitsi-meet-server-einstellungen-fuer-einen-datenschutzfreundlichen-betrieb/


```javascript
// Privacy
    //

    // If third party requests are disabled, no other server will be contacted.
    // This means avatars will be locally generated and callstats integration
    // will not function.
    disableThirdPartyRequests: true,
```

## increase log level / Logging reduzieren

- edit in $CONFIG/.jitsi-meet-cfg/jvb/logging.properties
- found here https://www.kuketz-blog.de/jitsi-meet-server-einstellungen-fuer-einen-datenschutzfreundlichen-betrieb/
- docker version
- not log ip address of clients

```java
# .level=INFO
.level=WARNING

# org.jitsi.videobridge.xmpp.ComponentImpl.level=FINE
org.jitsi.videobridge.xmpp.ComponentImpl.level=WARNING
```

## AVOID MOBILE_APP_PROMO


- edit in $CONFIG/.jitsi-meet-cfg/web/interfaces-config.js
- found here - found here https://www.kuketz-blog.de/jitsi-meet-server-einstellungen-fuer-einen-datenschutzfreundlichen-betrieb/

```javascript
/**
     * Whether the mobile app Jitsi Meet is to be promoted to participants
     * attempting to join a conference in a mobile Web browser. If
     * {@code undefined}, defaults to {@code true}.
     *
     * @type {boolean}
     */
    //MOBILE_APP_PROMO: false,
    MOBILE_APP_PROMO: true,
```

## change link form play strore to the f-droid downlaown page

- edit in $CONFIG/.jitsi-meet-cfg/web/interfaces-config.js
- found here - found here https://www.kuketz-blog.de/jitsi-meet-server-einstellungen-fuer-einen-datenschutzfreundlichen-betrieb/
- f-droid version tracker free

```javascript
/**
     * Specify custom URL for downloading android mobile app.
     */
    // MOBILE_DOWNLOAD_LINK_ANDROID: 'https://play.google.com/store/apps/details?id=org.jitsi.meet',
    MOBILE_DOWNLOAD_LINK_ANDROID: 'https://f-droid.org/en/packages/org.jitsi.meet/'

```

## add Datenschutzerklärung

- edit serveral place
- found here - found here https://www.kuketz-blog.de/jitsi-meet-server-einstellungen-fuer-einen-datenschutzfreundlichen-betrieb/

1) start docker-compose

2) copy /usr/share/jitsi-meet/css/all.css to $CONFIG/.jitsi-meet-cfg/web/

```bash

# change to config/web dir
cd && cd $CONFIG/.jitsi-meet-cfg/web

# docker cp <container_id od web container>:/usr/share/jitsi-meet/css/all.css all.css
# e.g.
docker cp 476:/usr/share/jitsi-meet/css/all.css all.css
```

3) add add footer cell

```bash
echo ".welcome .welcome-watermark{position:absolute;width:100%;height:auto}
#footer{margin-top:20px;margin-bottom:20px;font-size:14px}" | tee -a  all.css 
```

4) copy welcome page (optional)

```bash
cd && cd $CONFIG/.jitsi-meet-cfg/web
# docker cp <container_id od web container>:/usr/share/jitsi-meet/static/welcomePageAdditionalContent.html welcomePageAdditionalContent.html
# e.g.
docker cp 476:/usr/share/jitsi-meet/static/welcomePageAdditionalContent.html welcomePageAdditionalContent.html

```

5) edit welcome page

```bash
 cat << EOF >welcomePageAdditionalContent.html
 <template id = "welcome-page-additional-content-template">
   <div id="footer"> 
      <center>Betrieben von Mathias Stadler | <a href="">Impressum</a> | <a href="">Datenschutzhinweis</a> | <a href="">Erste Hilfe bei Problemen</a></center>
      <center>Diese Jitsi-Instanz ist <a href="">datenschutzfreundlich</a> und nutzt <strong>nicht</strong> die Google STUN-Server.</center>
   </div>
</template>
EOF
```
6) edit the link for page and create the pages if necessary 

7) add in meet location alias

- edit $CONFIG/.jitsi-meet-cfg/web/nginx/meet.conf
- add to the end of file

```bash
location = /css/all.css {
        alias /config/all.css;
        }

location = /static/welcomePageAdditionalContent.html {
        alias /config/welcomePageAdditionalContent.html;
        }
```

8 ) clear cache on your browser

9) (optional) check with curl it is work

## set default user name

- edit in $CONFIG/.jitsi-meet-cfg/web/interface_config.js

```javascript
// DEFAULT_REMOTE_DISPLAY_NAME: 'Fellow Jitster',
    DEFAULT_REMOTE_DISPLAY_NAME: 'NEW USER',
```

## set local display name

- edit in $CONFIG/.jitsi-meet-cfg/web/interface_config.js

```javascript
// DEFAULT_LOCAL_DISPLAY_NAME: 'me',
    DEFAULT_LOCAL_DISPLAY_NAME: 'ich',
```

## set off auto generate room name

- edit in $CONFIG/.jitsi-meet-cfg/web/interface_config.js

```javascript
// GENERATE_ROOMNAMES_ON_WELCOME_PAGE: true,
    GENERATE_ROOMNAMES_ON_WELCOME_PAGE: false,
```

## enable  lang detection 

- edit in $CONFIG/.jitsi-meet-cfg/web/interface_config.js

```javascript
// LANG_DETECTION: false, // Allow i18n to detect the system language
LANG_DETECTION: true, // Allow i18n to detect the system language
```

## set default lang

- edit in $CONFIG/.jitsi-meet-cfg/web/config.js

```javascript
// Default language for the user interface.
    defaultLanguage: 'de',
```

## hide jitsi logo

- edit in $CONFIG/.jitsi-meet-cfg/web/interface_config.js
- found here https://blog.wydler.eu/2020/03/22/einrichten-von-jitsi-meet-kostenlose-videokonferenzen-fuer-alle/#footer-auf-der-startseite-ergaenzen

```javascript
// SHOW_JITSI_WATERMARK: true,
SHOW_JITSI_WATERMARK: false,

// if watermark is disabled by default, it can be shown only for guests
    //SHOW_WATERMARK_FOR_GUESTS: true,
    SHOW_WATERMARK_FOR_GUESTS: false,
```

## change title in browser window

- edit in $CONFIG/.jitsi-meet-cfg/web/interface_config.js
- found here https://blog.wydler.eu/2020/03/22/einrichten-von-jitsi-meet-kostenlose-videokonferenzen-fuer-alle/#footer-auf-der-startseite-ergaenzen

```javascript
// APP_NAME: 'Jitsi Meet',
APP_NAME: 'Familie Stadler Ottobrunn',
// NATIVE_APP_NAME: 'Jitsi Meet',
NATIVE_APP_NAME: 'Familie Stadler Ottobrunn',
```

## disable button in menu

- edit in $CONFIG/.jitsi-meet-cfg/web/interface_config.js
- delete not wanted button
- e.g livestreaming and recording

```javascript
/**
     * The name of the toolbar buttons to display in the toolbar. If present,
     * the button will display. Exceptions are "livestreaming" and "recording"
     * which also require being a moderator and some values in config.js to be
     * enabled. Also, the "profile" button will not display for user's with a
     * jwt.
     */
    //TOOLBAR_BUTTONS: [
    //    'microphone', 'camera', 'desktop', 'fullscreen',
    //    'fodeviceselection', 'hangup', 'profile', 'info', 'chat', 'recording',
    //    'livestreaming', 'etherpad', 'sharedvideo', 'settings', 'raisehand',
    //    'videoquality', 'filmstrip', 'invite', 'feedback', 'stats', 'shortcuts',
    //    'tileview', 'videobackgroundblur', 'download', 'help', 'mute-everyone',
    //    'e2ee'
    //],

    TOOLBAR_BUTTONS: [
        'microphone', 'camera', 'desktop', 'fullscreen',
        'fodeviceselection', 'hangup', 'profile', 'info', 'chat',
        'etherpad', 'sharedvideo', 'settings', 'raisehand',
        'videoquality', 'filmstrip', 'invite', 'feedback', 'stats', 'shortcuts',
        'tileview', 'download', 'help', 'mute-everyone',
        'e2ee'
    ],

```

## force to set a nick name at start of videoconferencing

- edit in $CONFIG/.jitsi-meet-cfg/web/config.js
- uncomment

```javascript
// Require users to always specify a display name.
    requireDisplayName: true,
```


## enable layer suspension

- edit in $CONFIG/.jitsi-meet-cfg/web/config.js
- description here https://jitsi.org/blog/new-off-stage-layer-suppression-feature/
- save cpu and bandwidth

```javascript
// Enable / disable layer suspension.  If enabled, endpoints whose HD
    // layers are not in use will be suspended (no longer sent) until they
    // are requested again.
    enableLayerSuspension: true,
```

## enable close page

- edit in $CONFIG/.jitsi-meet-cfg/web/config.js
- uncomment

```javascript
// Enabling the close page will ignore the welcome page redirection when
    // a call is hangup.
    // enableClosePage: false,
    enableClosePage: true,
```

## disable invite function
- edit in $CONFIG/.jitsi-meet-cfg/web/config.js
- uncomment

```javascript
// Disables all invite functions from the app (share, invite, dial out...etc)
disableInviteFunctions: true,
```

## disable store old name in recents list

- edit in $CONFIG/.jitsi-meet-cfg/web/config.js
- uncomment

```javascript
// Disables storing the room name to the recents list
doNotStoreRoom: true,
```

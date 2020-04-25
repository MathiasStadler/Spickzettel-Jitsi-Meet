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
ls -l $HOME/.jitsi-meet-cfg
```



## change resolution / framerate / format for 

- edit in $HOME/.jitsi-meet-cfg/web/config.js
- sample for 8 frame per second 360 x 180 save some bandwidth
-  form here @TODO found link

```bash
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

- edit in $HOME/.jitsi-meet-cfg/web/config.js
- found here https://www.kuketz-blog.de/jitsi-meet-server-einstellungen-fuer-einen-datenschutzfreundlichen-betrieb/
- explain here https://community.jitsi.org/t/what-exactly-is-the-effect-of-channellastn/25244
- save bandwidth 

```txt
channelLastN: -1 = show all participant videos
channelLastN: 4 = show videos of last 4 active speakers only although everyone has video enabled
```


```text
// Default value for the channel "last N" attribute. -1 for unlimited.
    //channelLastN: -1,
    channelLastN: 4,
```

## use STUN and TURN server
- edit in $HOME/.jitsi-meet-cfg/web/config.js

```txt
// Use XEP-0215 to fetch STUN and TURN servers.
    // useStunTurn: true,
    useStunTurn: true,
```
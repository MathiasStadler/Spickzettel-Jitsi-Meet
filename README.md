# spickzettel-jitsi-meet

## housekeeping

```bash
docker run -it -p 8080:8080 -v "/home/trapapa/playground/Spickzettel-jitsi-meet:/home/coder/project" -u "$(id -u):$(id -g)" codercom/code-server:latest
git config --global user.name "Mathias Stadler"
git config --global user.EMAIL "email@mathias-stadler.de"
# https://help.github.com/en/github/using-git/caching-your-github-password-in-git
git config --global credential.helper 'cache --timeout=3600'


``
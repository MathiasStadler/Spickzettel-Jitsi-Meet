# spickzettel-jitsi-meet

docker run -it -p 8080:8080 -v "/home/trapapa/playground/b83154c3863e99542b0a1ed6:/home/coder/project" -u "$(id -u):$(id -g)" codercom/code-server:latest
git config --global user.name "Mathias Stadler"
git config --global user.EMAIL "email@mathias-stadler.de"
# https://help.github.com/en/github/using-git/caching-your-github-password-in-git
git config --global credential.helper 'cache --timeout=3600'
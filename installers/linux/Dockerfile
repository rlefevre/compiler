# Create: https://gist.github.com/rlefevre/1523f47e75310e28eee243c9c5651ac9
# Delete: docker system prune -a ; docker images -a

FROM alpine:3.10

# repo author/name
ARG repo=elm/compiler
# branch
ARG branch=master
# commit or tag
ARG commit=HEAD

# Install required packages
RUN apk add --update ghc cabal git musl-dev zlib-dev ncurses-dev ncurses-static wget

# Checkout elm compiler
WORKDIR /tmp
RUN git clone -b $branch https://github.com/${repo}.git

# Build a statically linked elm binary
WORKDIR /tmp/compiler
RUN git checkout $commit
RUN rm worker/elm.cabal
RUN cabal new-update
RUN cabal new-configure \
        --disable-executable-dynamic \
        --ghc-option=-optl=-static \
        --ghc-option=-optl=-pthread \
        --ghc-option=-split-sections
RUN cabal new-build
RUN cp ./dist-newstyle/build/x86_64-linux/ghc-*/elm-*/x/elm/build/elm/elm /usr/local/bin/elm
RUN strip -s /usr/local/bin/elm

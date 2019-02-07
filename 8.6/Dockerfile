## Dockerfile for a haskell environment
FROM       debian:stretch

## ensure locale is set during build
ENV LANG            C.UTF-8

RUN apt-get update 
RUN apt-get install -y --no-install-recommends gnupg ca-certificates dirmngr curl git 
RUN echo 'deb http://downloads.haskell.org/debian stretch main' > /etc/apt/sources.list.d/ghc.list 
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA3CBA3FFE22B574 
RUN apt-get update 
RUN apt-get install -y --no-install-recommends ghc-8.6.3 cabal-install-2.4 zlib1g-dev libtinfo-dev libsqlite3-dev g++ netbase xz-utils make 
RUN curl -fSL https://github.com/commercialhaskell/stack/releases/download/v1.9.1/stack-1.9.1-linux-x86_64.tar.gz -o stack.tar.gz 
RUN curl -fSL https://github.com/commercialhaskell/stack/releases/download/v1.9.1/stack-1.9.1-linux-x86_64.tar.gz.asc -o stack.tar.gz.asc 
RUN export GNUPGHOME="$(mktemp -d)" 
RUN gpg --batch --keyserver ha.pool.sks-keyservers.net --recv-keys C5705533DA4F78D8664B5DC0575159689BEFB442 
RUN gpg --batch --verify stack.tar.gz.asc stack.tar.gz 
RUN tar -xf stack.tar.gz -C /usr/local/bin --strip-components=1 
RUN /usr/local/bin/stack config set system-ghc --global true 
RUN /usr/local/bin/stack config set install-ghc --global false 
RUN rm -rf "$GNUPGHOME" /var/lib/apt/lists/* /stack.tar.gz.asc /stack.tar.gz

ENV PATH /root/.cabal/bin:/root/.local/bin:/opt/cabal/2.4/bin:/opt/ghc/8.6.3/bin:$PATH

## run ghci by default unless a command is specified

#CMD ["ghci"]
COPY ./entrypoint.sh /entrypoint.sh
CMD ["/bin/sh", "-c", "/entrypoint.sh 2>&1 | tee /var/logs/containner.log"]

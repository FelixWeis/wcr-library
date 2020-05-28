FROM buildpack-deps:buster-curl
LABEL maintainer="Felix Weis <twitter.com/felixweis>"

ENV BITCOIN_VERSION 0.20.0
ENV BITCOIN_DOWNLOAD_PATH https://bitcoincore.org/bin/bitcoin-core-${BITCOIN_VERSION}/test.rc2/
ENV BITCOIN_DOWNLOAD_FILENAME bitcoin-${BITCOIN_VERSION}rc2-x86_64-linux-gnu.tar.gz
ENV BITCOIN_RELEASES_KEY 01EA5486DE18A882D4C2684590C8019E36C2E964

RUN wget $BITCOIN_DOWNLOAD_PATH/$BITCOIN_DOWNLOAD_FILENAME \
	&& wget $BITCOIN_DOWNLOAD_PATH/SHA256SUMS.asc \
	&& wget https://bitcoin.org/laanwj-releases.asc \
	&& gpg --import laanwj-releases.asc \
	&& gpg --fingerprint $BITCOIN_RELEASES_KEY \
	&& gpg --verify SHA256SUMS.asc \
	&& grep -o "$(sha256sum $BITCOIN_DOWNLOAD_FILENAME)" SHA256SUMS.asc \
	&& tar -xzvf $BITCOIN_DOWNLOAD_FILENAME \
	&& cd bitcoin-*/bin \
	&& rm bitcoin-qt \
	&& rm test_bitcoin \
	&& mv * /usr/local/bin 
	

FROM debian:10-slim
COPY --from=0 /usr/local/bin/* /usr/local/bin/
ENTRYPOINT ["bitcoind"]
CMD ["-printtoconsole", "-disablewallet"]
EXPOSE 8333 8332
VOLUME /root/.bitcoin

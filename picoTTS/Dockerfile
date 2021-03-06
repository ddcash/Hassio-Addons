ARG BUILD_FROM
FROM $BUILD_FROM AS BUILD

RUN apk update && \
    apk add --no-cache \
		'git=2.26.2-r0' \
		'make=4.3-r0' \
		'g++=9.3.0-r2' \
		'autoconf=2.69-r2' \
		'libtool=2.4.6-r7' \
		'popt-dev=1.16-r7' \
		'automake=1.16.2-r0'

ENV COMMIT_SHA '2f86050dc5da9ab68fc61510b594d8e6975c4d2d'
# hadolint ignore=DL3003
RUN git clone https://github.com/naggety/picotts.git -b master /build && \
    cd build && git checkout ${COMMIT_SHA} &&\
    cd pico && ./autogen.sh && ./configure && make install

FROM $BUILD_FROM AS RUNNING

RUN apk add --no-cache \
			'python3=3.8.5-r0' \
			'popt=1.16-r7' && \
	python3 -m ensurepip

COPY --from=BUILD /usr/local/bin/pico2wave /usr/local/bin/pico2wave
COPY --from=BUILD /usr/local/lib/libttspico.* /usr/local/lib/
COPY --from=BUILD /usr/local/share/pico /usr/local/share/pico

COPY wrapper/ /app
WORKDIR /app
RUN pip3 install -r requirements.txt

COPY root /

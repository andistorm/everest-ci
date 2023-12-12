# syntax=docker/dockerfile:1
FROM python:3.11.4-alpine3.18

ARG CLANG_FORMAT_RELEASE_TAG=llvmorg-15.0.6
ARG TARGETPLATFORM
RUN echo "Building for $TARGETPLATFORM" && \
    if [ "$TARGETPLATFORM" = "linux/amd64" ]; then \
        suffix=x86_64-linux-gnu-ubuntu-18.04; \
    elif [ "$TARGETPLATFORM" = "linux/arm64" ]; then \
        suffix=aarch64-linux-gnu; \
    elif [ "$TARGETPLATFORM" = "linux/arm/v7" ]; then \
        suffix=armv7a-linux-gnueabihf; \
    else \
        echo "Unsupported platform: $TARGETPLATFORM"; \
        exit 1; \
    fi && \
    version=$(echo ${CLANG_FORMAT_RELEASE_TAG} | cut -d'-' -f2) && \
    clang_format_url=https://github.com/andistorm/everest-ci/releases/download/${CLANG_FORMAT_RELEASE_TAG}/clang-format-clang+llvm-${version}-${suffix} && \
    echo "url: ${clang_format_url}" && \
    wget -O /usr/bin/clang-format ${clang_format_url} && \
    chmod a+x /usr/bin/clang-format

COPY ./run-clang-format.py /usr/bin/run-clang-format.py

ENTRYPOINT ["/usr/bin/run-clang-format.py"]

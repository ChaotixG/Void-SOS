# aemu_postoffice: the PSP ad hoc server and relay that PPSSPP, PSPs and
# Vitas connect to (MULTIPLAYER.md §5), built from its release's source.
# Published as ghcr.io/chaotixg/aemu-postoffice by the public repo's
# workflow; RomM's catalogue entry pins it by digest.

# Release 2026-09-06-hotfix_1. The commit is checked, so the source is.
ARG COMMIT=f0aefbed1d34d4e593532f37c1e120f9e172fc93

FROM docker.io/library/debian:bookworm@sha256:f37a335e82bca302e955fa39f9dfe28f1be618f016f8a2b56318e5a5111afc26 AS build
ARG COMMIT
RUN apt-get update && apt-get install -y --no-install-recommends g++ git ca-certificates \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /src
RUN git init -q . && git fetch -q --depth 1 https://github.com/Kethen/aemu_postoffice.git "$COMMIT" \
 && git checkout -q FETCH_HEAD && test "$(git rev-parse HEAD)" = "$COMMIT"
WORKDIR /src/server_cpp
RUN bash build_linux.sh && strip aemu_postoffice

# The same Debian release's C++ runtime, nothing else: no shell.
FROM gcr.io/distroless/cc-debian12:nonroot@sha256:9dac0a79194e45a7da0158a9c6da57b217585af0786db3845d1f0ec1a0dd182f
ARG COMMIT
LABEL org.opencontainers.image.source="https://github.com/ChaotixG/Void-SOS" \
      org.opencontainers.image.description="aemu_postoffice ${COMMIT}: PSP ad hoc server and relay, for Void SOS" \
      org.opencontainers.image.licenses="GPL-3.0-only"
WORKDIR /app
COPY --from=build /src/server_cpp/aemu_postoffice /src/server_cpp/game_db.json /src/gpl-3.0.txt /app/
COPY --from=build /src/server_cpp/http_assets /app/http_assets
COPY config.json /app/config.json
USER 65532:65532
EXPOSE 27312/tcp 27313/tcp
ENTRYPOINT ["/app/aemu_postoffice"]

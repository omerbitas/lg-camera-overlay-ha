# Builds upstream mbsintegrations/LG-Camera-Overlay v1.2.1 from a pinned commit.
# The TV app is packaged from source here; no prebuilt binaries are downloaded.
FROM node:22-bookworm-slim AS build
RUN apt-get update && apt-get install -y --no-install-recommends git ca-certificates && rm -rf /var/lib/apt/lists/*
WORKDIR /src
RUN git clone --depth 1 --branch v1.2.1 https://github.com/mbsintegrations/LG-Camera-Overlay.git . \
 && test "$(git rev-parse HEAD)" = "3c27605a93123e87244a277b49c464a763274f34"
RUN npm ci --prefix tooling --ignore-scripts --no-audit --no-fund \
 && node scripts/build.cjs \
 && node scripts/verify-package.cjs \
 && cd dist && sha256sum com.mbs.cameraoverlay_1.2.1_all.ipk > SHA256SUMS.txt

FROM node:22-bookworm-slim
RUN apt-get update && apt-get install -y --no-install-recommends ffmpeg ca-certificates && rm -rf /var/lib/apt/lists/*
WORKDIR /app
COPY --from=build /src/package.json /src/package-lock.json ./
RUN npm ci --omit=dev --ignore-scripts --no-audit --no-fund && npm cache clean --force
COPY --from=build /src/relay ./relay
COPY --from=build /src/setup ./setup
COPY --from=build /src/dist ./dist
COPY --from=build /src/LICENSE /src/NOTICE ./
COPY --from=build /src/packaging/ha-entrypoint.sh /entrypoint.sh
RUN chmod 755 /entrypoint.sh
ENV OVERLAY_DATA_DIR=/data
ENTRYPOINT ["/entrypoint.sh"]

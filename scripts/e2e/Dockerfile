# syntax=docker/dockerfile:1.7

FROM node:24-bookworm@sha256:9f3b13503acdf9bc1e0213ccb25ebe86ac881cad17636733a1da1be1d44509df

RUN corepack enable

WORKDIR /app

ENV NODE_OPTIONS="--disable-warning=ExperimentalWarning"

COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./
COPY ui/package.json ./ui/package.json
COPY extensions/discord/package.json ./extensions/discord/package.json
COPY extensions/imessage/package.json ./extensions/imessage/package.json
COPY extensions/memory-core/package.json ./extensions/memory-core/package.json
COPY extensions/signal/package.json ./extensions/signal/package.json
COPY extensions/slack/package.json ./extensions/slack/package.json
COPY extensions/telegram/package.json ./extensions/telegram/package.json
COPY extensions/whatsapp/package.json ./extensions/whatsapp/package.json
COPY patches ./patches

RUN --mount=type=cache,id=openclaw-pnpm-store,target=/root/.local/share/pnpm/store,sharing=locked \
    pnpm install --frozen-lockfile

COPY tsconfig.json tsconfig.plugin-sdk.dts.json tsdown.config.ts vitest.config.ts vitest.e2e.config.ts vitest.unit.config.ts vitest.channels.config.ts vitest.extensions.config.ts vitest.gateway.config.ts vitest.live.config.ts vitest.scoped-config.ts openclaw.mjs ./
COPY src ./src
COPY test ./test
COPY scripts ./scripts
COPY docs ./docs
COPY skills ./skills
COPY ui ./ui
COPY extensions/discord ./extensions/discord
COPY extensions/imessage ./extensions/imessage
COPY extensions/memory-core ./extensions/memory-core
COPY extensions/signal ./extensions/signal
COPY extensions/slack ./extensions/slack
COPY extensions/shared ./extensions/shared
COPY extensions/telegram ./extensions/telegram
COPY extensions/whatsapp ./extensions/whatsapp
COPY vendor/a2ui/renderers/lit ./vendor/a2ui/renderers/lit
COPY apps/shared/OpenClawKit/Sources/OpenClawKit/Resources ./apps/shared/OpenClawKit/Sources/OpenClawKit/Resources
COPY apps/shared/OpenClawKit/Tools/CanvasA2UI ./apps/shared/OpenClawKit/Tools/CanvasA2UI

RUN pnpm build
RUN pnpm ui:build

RUN useradd --create-home --shell /bin/bash appuser \
  && chown -R appuser:appuser /app
USER appuser

CMD ["bash"]

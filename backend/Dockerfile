FROM ghcr.io/astral-sh/uv:python3.12-bookworm-slim AS base

ENV UV_COMPILE_BYTECODE=1 PYTHONUNBUFFERED=1
ENV UV_PROJECT_ENVIRONMENT=/backend_app/.venv
ENV PATH="/backend_app/.venv/bin:$PATH"

WORKDIR /backend_app

# 非ルートユーザーの作成 (UID 1000)
RUN groupadd -g 1000 appgroup && \
    useradd -u 1000 -g appgroup -s /bin/sh -m appuser

COPY pyproject.toml uv.lock ./
RUN uv sync --frozen --no-install-project --no-dev

COPY . .
RUN chown -R appuser:appgroup /backend_app

USER appuser

EXPOSE 8000

FROM base AS production
CMD ["fastapi", "run", "main.py", "--host", "0.0.0.0", "--port", "8000", "--proxy-headers"]



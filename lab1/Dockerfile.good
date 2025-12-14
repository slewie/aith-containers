FROM jupyterhub/jupyterhub:3.1.1

WORKDIR /app

RUN apt-get update && apt-get install -y --no-install-recommends \
    python3-dev \
    git \
    vim \
    nano \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && addgroup --gid 1001 appgroup \
    && useradd --uid 1001 --gid 1001 --create-home --shell /bin/bash appuser \
    && chown -R appuser:appgroup /app

USER appuser

VOLUME /app/data

ENTRYPOINT ["jupyterhub", "--log-level=DEBUG"]
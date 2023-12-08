FROM ruby:3.2.2-alpine

ARG WORKDIR
ARG RUNTIME_PACKAGES="nodejs tzdata postgresql-dev postgresql git"
ARG DEV_PACKAGES="build-base curl-dev"

ENV HOME=/${WORKDIR} \
    TZ=Asia/Tokyo

WORKDIR ${HOME}

# インストールに必要なパッケージを追加
RUN apk update \
    && apk upgrade \
    && apk add --no-cache ${RUNTIME_PACKAGES} \
    && apk add --virtual build-dependencies --no-cache ${DEV_PACKAGES}

COPY Gemfile* ./

# bundle install はここから先のコマンドで実行されるようにしてキャッシュを効率的に使う
RUN bundle install -j4 \
    && apk del build-dependencies

COPY . ./

CMD ["rails", "server", "-b", "0.0.0.0"]

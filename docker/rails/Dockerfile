FROM node:10-alpine as assets-builder
RUN mkdir -p /app
WORKDIR /app
COPY package.json yarn.lock ./
RUN yarn install
COPY webpack.config.js ./
COPY app/javascript ./app/javascript
RUN NODE_ENV=production yarn run build

FROM ruby:2.7.1
ENV RAILS_ENV production
ENV RAILS_SERVE_STATIC_FILES=1
RUN mkdir -p /app
WORKDIR /app
RUN gem install bundler -v 2.0.2
COPY Gemfile /app/Gemfile
COPY Gemfile.lock /app/Gemfile.lock
RUN bundle install -j4 --deployment --without 'development test'
COPY . /app
COPY --from=assets-builder /app/public/packs ./public/packs

CMD ["bundle", "exec", "rails", "server"]

FROM ruby:2.4.1

RUN apt-get update && apt-get install -y nodejs --no-install-recommends && rm -rf /var/lib/apt/lists/*

ENV NODE_VERSION 7.6.0
RUN curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
&& tar -xJf "node-v$NODE_VERSION-linux-x64.tar.xz" -C /usr/local --strip-components=1 \
&& rm "node-v$NODE_VERSION-linux-x64.tar.xz" \
&& ln -s /usr/local/bin/node /usr/local/bin/nodejs

ENV APP_HOME /app
RUN useradd app

RUN mkdir $APP_HOME
WORKDIR $APP_HOME

ADD Gemfile $APP_HOME/
ADD Gemfile.lock $APP_HOME/
ADD vendor $APP_HOME/vendor
RUN bundle install -j 8

ADD . $APP_HOME
RUN ln -sf /app/bin/phantomjs /usr/local/bin && phantomjs --version
RUN npm install
RUN chown -R app:app $APP_HOME/
USER app

RUN rake assets:precompile RAILS_ENV=production
RUN ln -sf /app/node_modules/semantic-ui-css/themes/ /app/public/assets

EXPOSE 3000
CMD ["rails", "server", "-b", "0.0.0.0"]

FROM ruby

RUN apt-get update -y && apt-get upgrade -y && \
	apt-get install wget nodejs sqlite3 -y --no-install-recommends

ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app
ONBUILD EXPOSE 3000
ONBUILD RUN bundle install
ONBUILD RUN bundle exec rake db:migrate
ONBUILD RUN bundle exec rake db:seed

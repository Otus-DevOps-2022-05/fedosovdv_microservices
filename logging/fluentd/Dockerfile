FROM fluent/fluentd:v0.12
RUN fluent-gem install elasticsearch -v 7.5
RUN fluent-gem install elasticsearch-transport -v 7.5.0
RUN fluent-gem install fluent-plugin-elasticsearch -v 1.13.0 --no-rdoc --no-ri
RUN gem install fluent-plugin-grok-parser --no-rdoc --no-ri --version 1.0.0
COPY fluent.conf /fluentd/etc

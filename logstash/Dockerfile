# https://github.com/elastic/logstash-docker
FROM docker.elastic.co/logstash/logstash:5.6.3

# Add your logstash plugins setup here
# Example: RUN logstash-plugin install logstash-filter-json
RUN \
  logstash-plugin install logstash-patterns-core && \
  logstash-plugin install logstash-filter-grok && \
  mkdir -p /opt/logstash/patterns && \
  curl -sS https://gist.githubusercontent.com/elijahpaul/f5f32d4e914dcb7fedd2/raw/d0a7b892db8d4f010f15f7946da6eb5f7000dcb7/pfsense2-2.grok | sed 's/,(%{INT:sub_rule}),/,(%{INT:sub_rule})?,/' | sed 's/,(%{WORD:iface}),/,(?<iface>\\b[._a-zA-Z0-9]+\\b),/' | sed 's/%{BASE16NUM:tos}),,(%{INT:ttl})/%{BASE16NUM:tos}),(%{INT:ecn})?,(%{INT:ttl})/' > /opt/logstash/patterns/pfsense.grok

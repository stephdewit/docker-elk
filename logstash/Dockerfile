# https://github.com/elastic/logstash-docker
FROM docker.elastic.co/logstash/logstash-oss:6.0.0

# Add your logstash plugins setup here
# Example: RUN logstash-plugin install logstash-filter-json
RUN \
  logstash-plugin install logstash-filter-mutate && \
  logstash-plugin install logstash-patterns-core && \
  logstash-plugin install logstash-filter-grok

ARG pfsense_grok_uri=https://gist.githubusercontent.com/elijahpaul/f5f32d4e914dcb7fedd2/raw/d0a7b892db8d4f010f15f7946da6eb5f7000dcb7/pfsense2-2.grok

RUN \
  mkdir -p /opt/logstash/patterns && \
  curl -sS $pfsense_grok_uri | \
    sed '/^LOG_DATA/ i\\IFACE \\b[._a-zA-Z0-9]+\\b\n' | \
    sed 's/,(%{INT:sub_rule}),/,(%{INT:sub_rule})?,/' | \
    sed 's/,(%{WORD:iface}),/,(%{IFACE:iface}),/' | \
    ( cat - ; echo ; echo "OTHER_PROTOCOLS_DATA (datalength=%{INT:data_length})" ) | \
    sed 's/|%{ICMP_DATA})/|%{ICMP_DATA}|%{OTHER_PROTOCOLS_DATA})/' | \
    sed 's/|timeexceed|/|timexceed|/' | \
    sed 's/%{BASE16NUM:tos}),,(%{INT:ttl})/%{BASE16NUM:tos}),(%{WORD:ecn})?,(%{INT:ttl})/' \
    > /opt/logstash/patterns/pfsense.grok

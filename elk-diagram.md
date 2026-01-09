```mermaid
graph TD
         A[send_log_to_elk.py] -- TCP port 5000 --> B(Logstash);
        B -- HTTP port 9200 --> C(Elasticsearch);
        D(Kibana) -- HTTP port 9200 --> C;
        subgraph "Docker Network (elk)"
            B;
            C;
            D;
   end

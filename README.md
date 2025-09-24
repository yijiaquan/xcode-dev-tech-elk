# xcode-dev-tech-elk

本项目是一个基于 Spring Boot 的技术栈示例项目，集成了 ELK（Elasticsearch、Logstash、Kibana）日志收集和分析系统。

## 项目结构

- `xcode-dev-tech-app`: Spring Boot 应用程序
- `docs/dev-ops`: Docker Compose 配置文件和相关配置
  - `elasticsearch`: Elasticsearch 配置和数据目录
  - `logstash`: Logstash 配置文件
  - `kibana`: Kibana 配置文件

## 快速开始

1. 使用 Docker Compose 启动 ELK 栈：
```bash
cd docs/dev-ops
docker-compose up -d
```

2. 启动 Spring Boot 应用：
```bash
cd xcode-dev-tech-app
mvn spring-boot:run
```

3. 访问 Kibana：

- URL: http://localhost:5601
- 在 Management > Stack Management > Index Patterns 中创建索引模式

## 注意事项

- Elasticsearch 在单机模式下运行，需要将索引的 `number_of_replicas` 设置为 0，以避免副本分片无法分配的问题。可以通过以下方式之一设置：

1. 在创建索引时指定配置：
    ```json
    {
      "index.blocks.read_only_allow_delete": "false",
      "index.priority": "1",
      "index.query.default_field": [
        "*"
      ],
      "index.refresh_interval": "1s",
      "index.write.wait_for_active_shards": "1",
      "index.routing.allocation.include._tier_preference": "data_content",
      "index.number_of_replicas": "0"
    }
    ```
2. 或者更新现有索引的设置：
    ```bash
    PUT /your-index-name/_settings
    {
        "index": {
         "number_of_replicas": "0"
        }
    }
    ```
- Logstash 配置在 `docs/dev-ops/logstash/logstash.conf`
- 应用日志配置在 `xcode-dev-tech-app/src/main/resources/logback-spring.xml`
- 默认使用 dev 环境配置，如需更改请修改 [application.yml](file://D:\yijiaquan\tech\road-map\Monitor\xcode-dev-tech-elk\xcode-dev-tech-app\target\classes\application.yml) 中的 `spring.profiles.active` 属性

## 端口信息

- 应用程序: 8091
- Elasticsearch: 9200, 9300
- Logstash: 4560, 50000 (TCP/UDP), 9600
- Kibana: 5601

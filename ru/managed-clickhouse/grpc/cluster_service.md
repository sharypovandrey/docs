---
editable: false
---

# ClusterService

Набор методов для управления кластерами ClickHouse.

| Вызов | Описание |
| --- | --- |
| [Get](#Get) | Возвращает указанный ClickHouse-кластер. |
| [List](#List) | Получает список ClickHouse-кластеров, принадлежащих указанному каталогу. |
| [Create](#Create) | Создает кластер ClickHouse в указанном каталоге. |
| [Update](#Update) | Изменяет указанный кластер ClickHouse. |
| [Delete](#Delete) | Удаляет указанный кластер ClickHouse. |
| [Start](#Start) | Запускает указанный кластер ClickHouse. |
| [Stop](#Stop) | Останавливает указанный кластер ClickHouse. |
| [Move](#Move) | Перемещает кластер ClickHouse в указанный каталог. |
| [AddZookeeper](#AddZookeeper) | Добавляет подкластер ZooKeeper в указанный кластер ClickHouse. |
| [Backup](#Backup) | Создает резервную копию для указанного кластера ClickHouse. |
| [Restore](#Restore) | Создает новый кластер ClickHouse с использованием указанной резервной копии. |
| [ListLogs](#ListLogs) | Получает логи для указанного кластера ClickHouse. |
| [ListOperations](#ListOperations) | Получает список ресурсов Operation для указанного кластера. |
| [ListBackups](#ListBackups) | Получает список доступных резервных копий для указанного кластера ClickHouse. |
| [ListHosts](#ListHosts) | Получает список хостов для указанного кластера. |
| [AddHosts](#AddHosts) | Создает новые хосты для кластера. |
| [DeleteHosts](#DeleteHosts) | Удаляет указанные хосты кластера. |
| [GetShard](#GetShard) | Возвращает указанный шард. |
| [ListShards](#ListShards) | Получает список шардов, принадлежащих указанному кластеру. |
| [AddShard](#AddShard) | Создает новый шард в указанном кластере. |
| [UpdateShard](#UpdateShard) | Изменяет указанный шард. |
| [DeleteShard](#DeleteShard) | Удаляет указанный шард. |
| [CreateExternalDictionary](#CreateExternalDictionary) | Создает внешний словарь для указанного кластера ClickHouse. |
| [DeleteExternalDictionary](#DeleteExternalDictionary) | Удаляет указанный внешний словарь. |

## Вызовы ClusterService {#calls}

## Get {#Get}

Возвращает указанный ClickHouse-кластер. <br>Чтобы получить список доступных кластеров ClickHouse, выполните запрос [List](#List).

**rpc Get ([GetClusterRequest](#GetClusterRequest)) returns ([Cluster](#Cluster))**

### GetClusterRequest {#GetClusterRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор возвращаемого ресурса Cluster для ClickHouse. Чтобы получить идентификатор кластера, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.


### Cluster {#Cluster}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор кластера ClickHouse. Этот идентификатор генерирует MDB при создании. 
folder_id | **string**<br>Идентификатор каталога, которому принадлежит кластер ClickHouse. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) . 
name | **string**<br>Имя кластера ClickHouse. Имя уникально в рамках каталога. Длина 1-63 символов. 
description | **string**<br>Описание кластера ClickHouse. Длина описания должна быть от 0 до 256 символов. 
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse в виде пар ``key:value``. Максимум 64 на ресурс. 
environment | enum **Environment**<br>Среда развертывания кластера ClickHouse. <ul><li>`PRODUCTION`: Стабильная среда с осторожной политикой обновления: во время регулярного обслуживания применяются только срочные исправления.</li><li>`PRESTABLE`: Среда с более агрессивной политикой обновления: новые версии развертываются независимо от обратной совместимости.</li><ul/>
monitoring[] | **[Monitoring](#Monitoring)**<br>Описание систем мониторинга, относящихся к данному кластеру ClickHouse. 
config | **[ClusterConfig](#ClusterConfig)**<br>Конфигурация кластера ClickHouse. 
network_id | **string**<br>Идентификатор сети, к которой принадлежит кластер. 
health | enum **Health**<br>Агрегированная работоспособность кластера. <ul><li>`HEALTH_UNKNOWN`: Состояние кластера неизвестно ([Host.health](#Host) для каждого хоста в кластере — UNKNOWN).</li><li>`ALIVE`: Кластер работает нормально ([Host.health](#Host) для каждого хоста в кластере — ALIVE).</li><li>`DEAD`: Кластер не работает ([Host.health](#Host) для каждого узла в кластере — DEAD).</li><li>`DEGRADED`: Кластер работает неоптимально ([Host.health](#Host) по крайней мере для одного узла в кластере не ALIVE).</li><ul/>
status | enum **Status**<br>Текущее состояние кластера. <ul><li>`STATUS_UNKNOWN`: Состояние кластера неизвестно.</li><li>`CREATING`: Кластер создается.</li><li>`RUNNING`: Кластер работает нормально.</li><li>`ERROR`: На кластере произошла ошибка, блокирующая работу.</li><li>`UPDATING`: Кластер изменяется.</li><li>`STOPPING`: Кластер останавливается.</li><li>`STOPPED`: Кластер остановлен.</li><li>`STARTING`: Кластер запускается.</li><ul/>


### Monitoring {#Monitoring}

Поле | Описание
--- | ---
name | **string**<br>Название системы мониторинга. 
description | **string**<br>Описание системы мониторинга. 
link | **string**<br>Ссылка на графики системы мониторинга для данного кластера ClickHouse. 


### ClusterConfig {#ClusterConfig}

Поле | Описание
--- | ---
version | **string**<br>Версия серверного программного обеспечения ClickHouse. 
clickhouse | **[Clickhouse](#Clickhouse)**<br>Конфигурация и распределение ресурсов для хостов ClickHouse. 
zookeeper | **[Zookeeper](#Zookeeper)**<br>Конфигурация и распределение ресурсов для хостов ZooKeeper. 
backup_window_start | **[google.type.TimeOfDay](https://github.com/googleapis/googleapis/blob/master/google/type/timeofday.proto)**<br>Время запуска ежедневного резервного копирования, в часовом поясе UTC. 


### Clickhouse {#Clickhouse}

Поле | Описание
--- | ---
config | **`config.ClickhouseConfigSet`**<br>Параметры конфигурации сервера ClickHouse. 
resources | **[Resources](#Resources)**<br>Ресурсы, выделенные хостам ClickHouse. 


### Zookeeper {#Zookeeper}

Поле | Описание
--- | ---
resources | **[Resources](#Resources)**<br>Ресурсы, выделенные хостам ZooKeeper. 


## List {#List}

Получает список ClickHouse-кластеров, принадлежащих указанному каталогу.

**rpc List ([ListClustersRequest](#ListClustersRequest)) returns ([ListClustersResponse](#ListClustersResponse))**

### ListClustersRequest {#ListClustersRequest}

Поле | Описание
--- | ---
folder_id | **string**<br>Обязательное поле. Идентификатор каталога для вывода списка кластеров ClickHouse. Чтобы получить идентификатор каталога, используйте запрос [yandex.cloud.resourcemanager.v1.FolderService.List](/docs/resource-manager/grpc/folder_service#List).  Максимальная длина строки в символах — 50.
page_size | **int64**<br>Максимальное количество результатов на странице ответа на запрос. Если количество результатов больше чем `page_size`, сервис вернет значение [ListClustersResponse.next_page_token](#ListClustersResponse), которое можно использовать для получения следующей страницы. Максимальное значение — 1000.
page_token | **string**<br>Токен страницы. Установите значение `page_token` равным значению поля [ListClustersResponse.next_page_token](#ListClustersResponse) предыдущего запроса, чтобы получить следующую страницу результатов. Максимальная длина строки в символах — 100.
filter | **string**<br><ol><li>Имя поля. В настоящее время фильтрацию можно использовать только с полем [Cluster.name](#Cluster1). </li><li>Оператор. Операторы `=` или `!=` для одиночных значений, `IN` или `NOT IN` для списков значений. </li><li>Значение. Должен содержать от 1 до 63 символов и соответствовать регулярному выражению `^[a-zA-Z0-9_-]+$`.</li></ol> Максимальная длина строки в символах — 1000.


### ListClustersResponse {#ListClustersResponse}

Поле | Описание
--- | ---
clusters[] | **[Cluster](#Cluster1)**<br>Список ресурсов Cluster для ClickHouse. 
next_page_token | **string**<br>Токен для получения следующей страницы результатов в ответе. Если количество результатов больше чем [ListClustersRequest.page_size](#ListClustersRequest1), используйте `next_page_token` в качестве значения параметра [ListClustersRequest.page_token](#ListClustersRequest1) в следующем запросе списка ресурсов. Все последующие запросы будут получать свои значения `next_page_token` для перебора страниц результатов. 


### Cluster {#Cluster}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор кластера ClickHouse. Этот идентификатор генерирует MDB при создании. 
folder_id | **string**<br>Идентификатор каталога, которому принадлежит кластер ClickHouse. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) . 
name | **string**<br>Имя кластера ClickHouse. Имя уникально в рамках каталога. Длина 1-63 символов. 
description | **string**<br>Описание кластера ClickHouse. Длина описания должна быть от 0 до 256 символов. 
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse в виде пар ``key:value``. Максимум 64 на ресурс. 
environment | enum **Environment**<br>Среда развертывания кластера ClickHouse. <ul><li>`PRODUCTION`: Стабильная среда с осторожной политикой обновления: во время регулярного обслуживания применяются только срочные исправления.</li><li>`PRESTABLE`: Среда с более агрессивной политикой обновления: новые версии развертываются независимо от обратной совместимости.</li><ul/>
monitoring[] | **[Monitoring](#Monitoring1)**<br>Описание систем мониторинга, относящихся к данному кластеру ClickHouse. 
config | **[ClusterConfig](#ClusterConfig1)**<br>Конфигурация кластера ClickHouse. 
network_id | **string**<br>Идентификатор сети, к которой принадлежит кластер. 
health | enum **Health**<br>Агрегированная работоспособность кластера. <ul><li>`HEALTH_UNKNOWN`: Состояние кластера неизвестно ([Host.health](#Host) для каждого хоста в кластере — UNKNOWN).</li><li>`ALIVE`: Кластер работает нормально ([Host.health](#Host) для каждого хоста в кластере — ALIVE).</li><li>`DEAD`: Кластер не работает ([Host.health](#Host) для каждого узла в кластере — DEAD).</li><li>`DEGRADED`: Кластер работает неоптимально ([Host.health](#Host) по крайней мере для одного узла в кластере не ALIVE).</li><ul/>
status | enum **Status**<br>Текущее состояние кластера. <ul><li>`STATUS_UNKNOWN`: Состояние кластера неизвестно.</li><li>`CREATING`: Кластер создается.</li><li>`RUNNING`: Кластер работает нормально.</li><li>`ERROR`: На кластере произошла ошибка, блокирующая работу.</li><li>`UPDATING`: Кластер изменяется.</li><li>`STOPPING`: Кластер останавливается.</li><li>`STOPPED`: Кластер остановлен.</li><li>`STARTING`: Кластер запускается.</li><ul/>


### Monitoring {#Monitoring}

Поле | Описание
--- | ---
name | **string**<br>Название системы мониторинга. 
description | **string**<br>Описание системы мониторинга. 
link | **string**<br>Ссылка на графики системы мониторинга для данного кластера ClickHouse. 


### ClusterConfig {#ClusterConfig}

Поле | Описание
--- | ---
version | **string**<br>Версия серверного программного обеспечения ClickHouse. 
clickhouse | **[Clickhouse](#Clickhouse1)**<br>Конфигурация и распределение ресурсов для хостов ClickHouse. 
zookeeper | **[Zookeeper](#Zookeeper1)**<br>Конфигурация и распределение ресурсов для хостов ZooKeeper. 
backup_window_start | **[google.type.TimeOfDay](https://github.com/googleapis/googleapis/blob/master/google/type/timeofday.proto)**<br>Время запуска ежедневного резервного копирования, в часовом поясе UTC. 


### Clickhouse {#Clickhouse}

Поле | Описание
--- | ---
config | **`config.ClickhouseConfigSet`**<br>Параметры конфигурации сервера ClickHouse. 
resources | **[Resources](#Resources)**<br>Ресурсы, выделенные хостам ClickHouse. 


### Zookeeper {#Zookeeper}

Поле | Описание
--- | ---
resources | **[Resources](#Resources)**<br>Ресурсы, выделенные хостам ZooKeeper. 


## Create {#Create}

Создает кластер ClickHouse в указанном каталоге.

**rpc Create ([CreateClusterRequest](#CreateClusterRequest)) returns ([operation.Operation](#Operation))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[CreateClusterMetadata](#CreateClusterMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[Cluster](#Cluster2)<br>

### CreateClusterRequest {#CreateClusterRequest}

Поле | Описание
--- | ---
folder_id | **string**<br>Обязательное поле. Идентификатор каталога, в котором нужно создать кластер ClickHouse.  Максимальная длина строки в символах — 50.
name | **string**<br>Обязательное поле. Имя кластера ClickHouse. Имя должно быть уникальным в каталоге.  Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_-]* `.
description | **string**<br>Описание кластера ClickHouse. Максимальная длина строки в символах — 256.
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse как пары `key:value`. Максимум 64 на ресурс. Например, "project": "mvp" или "source": "dictionary". Не более 64 на ресурс. Максимальная длина строки в символах для каждого значения — 63. Каждое значение должно соответствовать регулярному выражению ` [-_0-9a-z]* `. Максимальная длина строки в символах для каждого ключа — 63. Каждый ключ должен соответствовать регулярному выражению ` [a-z][-_0-9a-z]* `.
environment | **[Cluster.Environment](#Cluster2)**<br>Обязательное поле. Среда развертывания кластера ClickHouse. 
config_spec | **[ConfigSpec](#ConfigSpec)**<br>Обязательное поле. Конфигурация и ресурсы для хостов, которые должны быть созданы для кластера ClickHouse. 
database_specs[] | **[DatabaseSpec](#DatabaseSpec)**<br>Описания баз данных, которые нужно создать в кластере ClickHouse. Количество элементов должно быть больше 0.
user_specs[] | **[UserSpec](#UserSpec)**<br>Описания пользователей базы данных, которых нужно создать в кластере ClickHouse. Количество элементов должно быть больше 0.
host_specs[] | **[HostSpec](#HostSpec)**<br>Конфигурации для отдельных хостов, которые должны быть созданы для кластера ClickHouse. Количество элементов должно быть больше 0.
network_id | **string**<br>Обязательное поле. Идентификатор сети, в которой нужно создать кластер.  Максимальная длина строки в символах — 50.
shard_name | **string**<br>Имя первого шарда в кластере. Если параметр не указан, используется значение `shard1`. Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_-]* `.


### ConfigSpec {#ConfigSpec}

Поле | Описание
--- | ---
version | **string**<br>Версия серверного программного обеспечения ClickHouse. 
clickhouse | **[Clickhouse](#Clickhouse2)**<br>Конфигурация и ресурсы для сервера ClickHouse. 
zookeeper | **[Zookeeper](#Zookeeper2)**<br>Конфигурация и ресурсы для сервера ZooKeeper. 
backup_window_start | **[google.type.TimeOfDay](https://github.com/googleapis/googleapis/blob/master/google/type/timeofday.proto)**<br>Время запуска ежедневного резервного копирования, в часовом поясе UTC. 


### Clickhouse {#Clickhouse}

Поле | Описание
--- | ---
config | **`config.ClickhouseConfig`**<br>Конфигурация для сервера ClickHouse. 
resources | **[Resources](#Resources)**<br>Ресурсы, выделенные хостам ClickHouse. 


### Zookeeper {#Zookeeper}

Поле | Описание
--- | ---
resources | **[Resources](#Resources)**<br>Ресурсы, выделенные хостам ZooKeeper. Если не задано, будет использоваться минимальный доступный набор ресурсов. Все доступные наборы ресурсов можно получить с помощью запроса [ResourcePresetService.List](./resource_preset_service#List). 


### DatabaseSpec {#DatabaseSpec}

Поле | Описание
--- | ---
name | **string**<br>Обязательное поле. Имя базы данных ClickHouse. Длина 1-63 символов.  Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_-]* `.


### UserSpec {#UserSpec}

Поле | Описание
--- | ---
name | **string**<br>Обязательное поле. Имя пользователя базы данных ClickHouse.  Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_]* `.
password | **string**<br>Обязательное поле. Пароль пользователя ClickHouse.  Длина строки в символах должна быть от 8 до 128.
permissions[] | **[Permission](#Permission)**<br>Набор разрешений, которые следует предоставить пользователю. 
settings | **[UserSettings](#UserSettings)**<br> 


### Permission {#Permission}

Поле | Описание
--- | ---
database_name | **string**<br>Имя базы данных, к которой предоставляет доступ разрешение. 


### UserSettings {#UserSettings}

Поле | Описание
--- | ---
readonly | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/permissions_for_queries/#settings_readonly). Допустимые значения — от 0 до 2 включительно.
allow_ddl | **[google.protobuf.BoolValue](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/bool-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/permissions_for_queries/#settings_allow_ddl). 
insert_quorum | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/settings/#settings-insert_quorum). Минимальная значение — 0.
insert_quorum_timeout | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br><ol><li>См. </li></ol> Минимальная значение — 1000.
select_sequential_consistency | **[google.protobuf.BoolValue](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/bool-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/settings/#settings-select_sequential_consistency). 
max_replica_delay_for_distributed_queries | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br><ol><li>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/settings/#settings-max_replica_delay_for_distributed_queries).</li></ol> Минимальная значение — 1000.
fallback_to_stale_replicas_for_distributed_queries | **[google.protobuf.BoolValue](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/bool-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/settings/#settings-fallback_to_stale_replicas_for_distributed_queries). 
max_threads | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/settings/#settings-max_threads). Значение должно быть больше 0.
max_block_size | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/settings/#max-block-size). Значение должно быть больше 0.
max_insert_block_size | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/settings/#settings-max_insert_block_size). Значение должно быть больше 0.
max_memory_usage | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#settings_max_memory_usage). Минимальная значение — 0.
max_memory_usage_for_user | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-memory-usage-for-user). Минимальная значение — 0.
max_rows_to_read | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-rows-to-read). Минимальная значение — 0.
max_bytes_to_read | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-bytes-to-read). Минимальная значение — 0.
read_overflow_mode | enum **OverflowMode**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#read-overflow-mode). <ul><ul/>
max_rows_to_group_by | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-rows-to-group-by). Минимальная значение — 0.
group_by_overflow_mode | enum **GroupByOverflowMode**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#group-by-overflow-mode). <ul><ul/>
max_rows_to_sort | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-rows-to-sort). Минимальная значение — 0.
max_bytes_to_sort | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-bytes-to-sort). Минимальная значение — 0.
sort_overflow_mode | enum **OverflowMode**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#sort-overflow-mode). <ul><ul/>
max_result_rows | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-result-rows). Минимальная значение — 0.
max_result_bytes | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-result-bytes). Минимальная значение — 0.
result_overflow_mode | enum **OverflowMode**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#result-overflow-mode). <ul><ul/>
max_rows_in_distinct | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-rows-in-distinct). Минимальная значение — 0.
max_bytes_in_distinct | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-bytes-in-distinct). Минимальная значение — 0.
distinct_overflow_mode | enum **OverflowMode**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#distinct-overflow-mode). <ul><ul/>
max_rows_to_transfer | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-rows-to-transfer). Минимальная значение — 0.
max_bytes_to_transfer | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-bytes-to-transfer). Минимальная значение — 0.
transfer_overflow_mode | enum **OverflowMode**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#transfer-overflow-mode). <ul><ul/>
max_execution_time | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>Максимальное время выполнения запроса в миллисекундах. См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-execution-time). Минимальная значение — 0.
timeout_overflow_mode | enum **OverflowMode**<br>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#timeout-overflow-mode). <ul><ul/>
max_columns_to_read | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>Максимальное количество столбцов, которые можно прочитать из таблицы в одном запросе. См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-columns-to-read). Минимальная значение — 0.
max_temporary_columns | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>Максимальное количество временных столбцов, которые должны храниться в оперативной памяти одновременно при выполнении запроса, включая постоянные столбцы. См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-temporary-columns). Минимальная значение — 0.
max_temporary_non_const_columns | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>Максимальное количество временных столбцов, которые должны храниться в оперативной памяти одновременно при выполнении запроса, за исключением постоянных столбцов. См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-temporary-non-const-columns). Минимальная значение — 0.
max_query_size | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br><ol><li>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/settings/#settings-max_query_size).</li></ol> Значение должно быть больше 0.
max_ast_depth | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br><ol><li>См. </li></ol> Значение должно быть больше 0.
max_ast_elements | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br><ol><li>См. подробное описание в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/settings/query_complexity/#max-ast-elements).</li></ol> Значение должно быть больше 0.
max_expanded_ast_elements | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>Максимальный размер синтаксического дерева запроса в количестве узлов после раскрытия псевдонимов и звездочки. Значение по умолчанию: 500000. Значение должно быть больше 0.


### HostSpec {#HostSpec}

Поле | Описание
--- | ---
zone_id | **string**<br>Идентификатор зоны доступности, в которой находится хост. Чтобы получить список доступных зон, используйте запрос [yandex.cloud.compute.v1.ZoneService.List](/docs/compute/grpc/zone_service#List). Максимальная длина строки в символах — 50.
type | **[Host.Type](#Host)**<br>Обязательное поле. Тип развертываемого хоста. 
subnet_id | **string**<br>Идентификатор подсети, к которой должен принадлежать хост. Эта подсеть должна быть частью сети, к которой принадлежит кластер. Идентификатор сети задается в поле [Cluster.network_id](#Cluster2). Максимальная длина строки в символах — 50.
assign_public_ip | **bool**<br><ul><li>false — не назначать хосту публичный IP-адрес. </li><li>true — у хоста должен быть публичный IP-адрес.</li></ul> 
shard_name | **string**<br>Имя шарда, которому принадлежит хост. Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_-]* `.


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[CreateClusterMetadata](#CreateClusterMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[Cluster](#Cluster2)>**<br>в случае успешного выполнения операции. 


### CreateClusterMetadata {#CreateClusterMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор создаваемого кластера ClickHouse. 


### Cluster {#Cluster}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор кластера ClickHouse. Этот идентификатор генерирует MDB при создании. 
folder_id | **string**<br>Идентификатор каталога, которому принадлежит кластер ClickHouse. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) . 
name | **string**<br>Имя кластера ClickHouse. Имя уникально в рамках каталога. Длина 1-63 символов. 
description | **string**<br>Описание кластера ClickHouse. Длина описания должна быть от 0 до 256 символов. 
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse в виде пар ``key:value``. Максимум 64 на ресурс. 
environment | enum **Environment**<br>Среда развертывания кластера ClickHouse. <ul><li>`PRODUCTION`: Стабильная среда с осторожной политикой обновления: во время регулярного обслуживания применяются только срочные исправления.</li><li>`PRESTABLE`: Среда с более агрессивной политикой обновления: новые версии развертываются независимо от обратной совместимости.</li><ul/>
monitoring[] | **[Monitoring](#Monitoring2)**<br>Описание систем мониторинга, относящихся к данному кластеру ClickHouse. 
config | **[ClusterConfig](#ClusterConfig2)**<br>Конфигурация кластера ClickHouse. 
network_id | **string**<br>Идентификатор сети, к которой принадлежит кластер. 
health | enum **Health**<br>Агрегированная работоспособность кластера. <ul><li>`HEALTH_UNKNOWN`: Состояние кластера неизвестно ([Host.health](#Host) для каждого хоста в кластере — UNKNOWN).</li><li>`ALIVE`: Кластер работает нормально ([Host.health](#Host) для каждого хоста в кластере — ALIVE).</li><li>`DEAD`: Кластер не работает ([Host.health](#Host) для каждого узла в кластере — DEAD).</li><li>`DEGRADED`: Кластер работает неоптимально ([Host.health](#Host) по крайней мере для одного узла в кластере не ALIVE).</li><ul/>
status | enum **Status**<br>Текущее состояние кластера. <ul><li>`STATUS_UNKNOWN`: Состояние кластера неизвестно.</li><li>`CREATING`: Кластер создается.</li><li>`RUNNING`: Кластер работает нормально.</li><li>`ERROR`: На кластере произошла ошибка, блокирующая работу.</li><li>`UPDATING`: Кластер изменяется.</li><li>`STOPPING`: Кластер останавливается.</li><li>`STOPPED`: Кластер остановлен.</li><li>`STARTING`: Кластер запускается.</li><ul/>


## Update {#Update}

Изменяет указанный кластер ClickHouse.

**rpc Update ([UpdateClusterRequest](#UpdateClusterRequest)) returns ([operation.Operation](#Operation1))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[UpdateClusterMetadata](#UpdateClusterMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[Cluster](#Cluster3)<br>

### UpdateClusterRequest {#UpdateClusterRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор ресурса Cluster для ClickHouse, который следует обновить. Чтобы получить идентификатор кластера ClickHouse, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.
update_mask | **[google.protobuf.FieldMask](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/field-mask)**<br>Маска, которая указывает, какие поля ресурса Cluster для ClickHouse должны быть изменены. 
description | **string**<br>Новое описание кластера ClickHouse. Максимальная длина строки в символах — 256.
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse как пары `key:value`. Максимум 64 на ресурс. Например, "project": "mvp" или "source": "dictionary". <br>Новый набор меток полностью заменит старый. Чтобы добавить метку, запросите текущий набор меток с помощью метода [ClusterService.Get](#Get), затем отправьте запрос [ClusterService.Update](#Update), добавив новую метку в этот набор. Не более 64 на ресурс. Максимальная длина строки в символах для каждого значения — 63. Каждое значение должно соответствовать регулярному выражению ` [-_0-9a-z]* `. Максимальная длина строки в символах для каждого ключа — 63. Каждый ключ должен соответствовать регулярному выражению ` [a-z][-_0-9a-z]* `.
config_spec | **[ConfigSpec](#ConfigSpec1)**<br>Новая конфигурация и ресурсы для хостов кластера. 
name | **string**<br>Новое имя кластера. Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_-]* `.


### ConfigSpec {#ConfigSpec}

Поле | Описание
--- | ---
version | **string**<br>Версия серверного программного обеспечения ClickHouse. 
clickhouse | **[Clickhouse](#Clickhouse3)**<br>Конфигурация и ресурсы для сервера ClickHouse. 
zookeeper | **[Zookeeper](#Zookeeper3)**<br>Конфигурация и ресурсы для сервера ZooKeeper. 
backup_window_start | **[google.type.TimeOfDay](https://github.com/googleapis/googleapis/blob/master/google/type/timeofday.proto)**<br>Время запуска ежедневного резервного копирования, в часовом поясе UTC. 


### Clickhouse {#Clickhouse}

Поле | Описание
--- | ---
config | **`config.ClickhouseConfig`**<br>Конфигурация для сервера ClickHouse. 
resources | **[Resources](#Resources)**<br>Ресурсы, выделенные хостам ClickHouse. 


### Zookeeper {#Zookeeper}

Поле | Описание
--- | ---
resources | **[Resources](#Resources)**<br>Ресурсы, выделенные хостам ZooKeeper. Если не задано, будет использоваться минимальный доступный набор ресурсов. Все доступные наборы ресурсов можно получить с помощью запроса [ResourcePresetService.List](./resource_preset_service#List). 


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[UpdateClusterMetadata](#UpdateClusterMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[Cluster](#Cluster3)>**<br>в случае успешного выполнения операции. 


### UpdateClusterMetadata {#UpdateClusterMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор изменяемого ресурса Cluster для ClickHouse. 


### Cluster {#Cluster}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор кластера ClickHouse. Этот идентификатор генерирует MDB при создании. 
folder_id | **string**<br>Идентификатор каталога, которому принадлежит кластер ClickHouse. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) . 
name | **string**<br>Имя кластера ClickHouse. Имя уникально в рамках каталога. Длина 1-63 символов. 
description | **string**<br>Описание кластера ClickHouse. Длина описания должна быть от 0 до 256 символов. 
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse в виде пар ``key:value``. Максимум 64 на ресурс. 
environment | enum **Environment**<br>Среда развертывания кластера ClickHouse. <ul><li>`PRODUCTION`: Стабильная среда с осторожной политикой обновления: во время регулярного обслуживания применяются только срочные исправления.</li><li>`PRESTABLE`: Среда с более агрессивной политикой обновления: новые версии развертываются независимо от обратной совместимости.</li><ul/>
monitoring[] | **[Monitoring](#Monitoring2)**<br>Описание систем мониторинга, относящихся к данному кластеру ClickHouse. 
config | **[ClusterConfig](#ClusterConfig2)**<br>Конфигурация кластера ClickHouse. 
network_id | **string**<br>Идентификатор сети, к которой принадлежит кластер. 
health | enum **Health**<br>Агрегированная работоспособность кластера. <ul><li>`HEALTH_UNKNOWN`: Состояние кластера неизвестно ([Host.health](#Host) для каждого хоста в кластере — UNKNOWN).</li><li>`ALIVE`: Кластер работает нормально ([Host.health](#Host) для каждого хоста в кластере — ALIVE).</li><li>`DEAD`: Кластер не работает ([Host.health](#Host) для каждого узла в кластере — DEAD).</li><li>`DEGRADED`: Кластер работает неоптимально ([Host.health](#Host) по крайней мере для одного узла в кластере не ALIVE).</li><ul/>
status | enum **Status**<br>Текущее состояние кластера. <ul><li>`STATUS_UNKNOWN`: Состояние кластера неизвестно.</li><li>`CREATING`: Кластер создается.</li><li>`RUNNING`: Кластер работает нормально.</li><li>`ERROR`: На кластере произошла ошибка, блокирующая работу.</li><li>`UPDATING`: Кластер изменяется.</li><li>`STOPPING`: Кластер останавливается.</li><li>`STOPPED`: Кластер остановлен.</li><li>`STARTING`: Кластер запускается.</li><ul/>


## Delete {#Delete}

Удаляет указанный кластер ClickHouse.

**rpc Delete ([DeleteClusterRequest](#DeleteClusterRequest)) returns ([operation.Operation](#Operation2))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[DeleteClusterMetadata](#DeleteClusterMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[google.protobuf.Empty](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#google.protobuf.Empty)<br>

### DeleteClusterRequest {#DeleteClusterRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse, который нужно удалить. Чтобы получить идентификатор кластера ClickHouse, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[DeleteClusterMetadata](#DeleteClusterMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[google.protobuf.Empty](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#google.protobuf.Empty)>**<br>в случае успешного выполнения операции. 


### DeleteClusterMetadata {#DeleteClusterMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор удаляемого кластера ClickHouse. 


## Start {#Start}

Запускает указанный кластер ClickHouse.

**rpc Start ([StartClusterRequest](#StartClusterRequest)) returns ([operation.Operation](#Operation3))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[StartClusterMetadata](#StartClusterMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[Cluster](#Cluster4)<br>

### StartClusterRequest {#StartClusterRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse, который следует запустить.  Максимальная длина строки в символах — 50.


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[StartClusterMetadata](#StartClusterMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[Cluster](#Cluster4)>**<br>в случае успешного выполнения операции. 


### StartClusterMetadata {#StartClusterMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор запускаемого кластера ClickHouse. 


### Cluster {#Cluster}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор кластера ClickHouse. Этот идентификатор генерирует MDB при создании. 
folder_id | **string**<br>Идентификатор каталога, которому принадлежит кластер ClickHouse. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) . 
name | **string**<br>Имя кластера ClickHouse. Имя уникально в рамках каталога. Длина 1-63 символов. 
description | **string**<br>Описание кластера ClickHouse. Длина описания должна быть от 0 до 256 символов. 
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse в виде пар ``key:value``. Максимум 64 на ресурс. 
environment | enum **Environment**<br>Среда развертывания кластера ClickHouse. <ul><li>`PRODUCTION`: Стабильная среда с осторожной политикой обновления: во время регулярного обслуживания применяются только срочные исправления.</li><li>`PRESTABLE`: Среда с более агрессивной политикой обновления: новые версии развертываются независимо от обратной совместимости.</li><ul/>
monitoring[] | **[Monitoring](#Monitoring2)**<br>Описание систем мониторинга, относящихся к данному кластеру ClickHouse. 
config | **[ClusterConfig](#ClusterConfig2)**<br>Конфигурация кластера ClickHouse. 
network_id | **string**<br>Идентификатор сети, к которой принадлежит кластер. 
health | enum **Health**<br>Агрегированная работоспособность кластера. <ul><li>`HEALTH_UNKNOWN`: Состояние кластера неизвестно ([Host.health](#Host) для каждого хоста в кластере — UNKNOWN).</li><li>`ALIVE`: Кластер работает нормально ([Host.health](#Host) для каждого хоста в кластере — ALIVE).</li><li>`DEAD`: Кластер не работает ([Host.health](#Host) для каждого узла в кластере — DEAD).</li><li>`DEGRADED`: Кластер работает неоптимально ([Host.health](#Host) по крайней мере для одного узла в кластере не ALIVE).</li><ul/>
status | enum **Status**<br>Текущее состояние кластера. <ul><li>`STATUS_UNKNOWN`: Состояние кластера неизвестно.</li><li>`CREATING`: Кластер создается.</li><li>`RUNNING`: Кластер работает нормально.</li><li>`ERROR`: На кластере произошла ошибка, блокирующая работу.</li><li>`UPDATING`: Кластер изменяется.</li><li>`STOPPING`: Кластер останавливается.</li><li>`STOPPED`: Кластер остановлен.</li><li>`STARTING`: Кластер запускается.</li><ul/>


## Stop {#Stop}

Останавливает указанный кластер ClickHouse.

**rpc Stop ([StopClusterRequest](#StopClusterRequest)) returns ([operation.Operation](#Operation4))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[StopClusterMetadata](#StopClusterMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[Cluster](#Cluster5)<br>

### StopClusterRequest {#StopClusterRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse, который следует остановить.  Максимальная длина строки в символах — 50.


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[StopClusterMetadata](#StopClusterMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[Cluster](#Cluster5)>**<br>в случае успешного выполнения операции. 


### StopClusterMetadata {#StopClusterMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор останавливаемого кластера ClickHouse. 


### Cluster {#Cluster}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор кластера ClickHouse. Этот идентификатор генерирует MDB при создании. 
folder_id | **string**<br>Идентификатор каталога, которому принадлежит кластер ClickHouse. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) . 
name | **string**<br>Имя кластера ClickHouse. Имя уникально в рамках каталога. Длина 1-63 символов. 
description | **string**<br>Описание кластера ClickHouse. Длина описания должна быть от 0 до 256 символов. 
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse в виде пар ``key:value``. Максимум 64 на ресурс. 
environment | enum **Environment**<br>Среда развертывания кластера ClickHouse. <ul><li>`PRODUCTION`: Стабильная среда с осторожной политикой обновления: во время регулярного обслуживания применяются только срочные исправления.</li><li>`PRESTABLE`: Среда с более агрессивной политикой обновления: новые версии развертываются независимо от обратной совместимости.</li><ul/>
monitoring[] | **[Monitoring](#Monitoring2)**<br>Описание систем мониторинга, относящихся к данному кластеру ClickHouse. 
config | **[ClusterConfig](#ClusterConfig2)**<br>Конфигурация кластера ClickHouse. 
network_id | **string**<br>Идентификатор сети, к которой принадлежит кластер. 
health | enum **Health**<br>Агрегированная работоспособность кластера. <ul><li>`HEALTH_UNKNOWN`: Состояние кластера неизвестно ([Host.health](#Host) для каждого хоста в кластере — UNKNOWN).</li><li>`ALIVE`: Кластер работает нормально ([Host.health](#Host) для каждого хоста в кластере — ALIVE).</li><li>`DEAD`: Кластер не работает ([Host.health](#Host) для каждого узла в кластере — DEAD).</li><li>`DEGRADED`: Кластер работает неоптимально ([Host.health](#Host) по крайней мере для одного узла в кластере не ALIVE).</li><ul/>
status | enum **Status**<br>Текущее состояние кластера. <ul><li>`STATUS_UNKNOWN`: Состояние кластера неизвестно.</li><li>`CREATING`: Кластер создается.</li><li>`RUNNING`: Кластер работает нормально.</li><li>`ERROR`: На кластере произошла ошибка, блокирующая работу.</li><li>`UPDATING`: Кластер изменяется.</li><li>`STOPPING`: Кластер останавливается.</li><li>`STOPPED`: Кластер остановлен.</li><li>`STARTING`: Кластер запускается.</li><ul/>


## Move {#Move}

Перемещает кластер ClickHouse в указанный каталог.

**rpc Move ([MoveClusterRequest](#MoveClusterRequest)) returns ([operation.Operation](#Operation5))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[MoveClusterMetadata](#MoveClusterMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[Cluster](#Cluster6)<br>

### MoveClusterRequest {#MoveClusterRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse, который следует переместить.  Максимальная длина строки в символах — 50.
destination_folder_id | **string**<br>Обязательное поле. Идентификатор каталога, в который следует переместить кластер.  Максимальная длина строки в символах — 50.


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[MoveClusterMetadata](#MoveClusterMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[Cluster](#Cluster6)>**<br>в случае успешного выполнения операции. 


### MoveClusterMetadata {#MoveClusterMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор перемещаемого кластера ClickHouse. 
source_folder_id | **string**<br>Идентификатор исходного каталога. 
destination_folder_id | **string**<br>Идентификатор каталога, в который следует переместить кластер. 


### Cluster {#Cluster}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор кластера ClickHouse. Этот идентификатор генерирует MDB при создании. 
folder_id | **string**<br>Идентификатор каталога, которому принадлежит кластер ClickHouse. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) . 
name | **string**<br>Имя кластера ClickHouse. Имя уникально в рамках каталога. Длина 1-63 символов. 
description | **string**<br>Описание кластера ClickHouse. Длина описания должна быть от 0 до 256 символов. 
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse в виде пар ``key:value``. Максимум 64 на ресурс. 
environment | enum **Environment**<br>Среда развертывания кластера ClickHouse. <ul><li>`PRODUCTION`: Стабильная среда с осторожной политикой обновления: во время регулярного обслуживания применяются только срочные исправления.</li><li>`PRESTABLE`: Среда с более агрессивной политикой обновления: новые версии развертываются независимо от обратной совместимости.</li><ul/>
monitoring[] | **[Monitoring](#Monitoring2)**<br>Описание систем мониторинга, относящихся к данному кластеру ClickHouse. 
config | **[ClusterConfig](#ClusterConfig2)**<br>Конфигурация кластера ClickHouse. 
network_id | **string**<br>Идентификатор сети, к которой принадлежит кластер. 
health | enum **Health**<br>Агрегированная работоспособность кластера. <ul><li>`HEALTH_UNKNOWN`: Состояние кластера неизвестно ([Host.health](#Host) для каждого хоста в кластере — UNKNOWN).</li><li>`ALIVE`: Кластер работает нормально ([Host.health](#Host) для каждого хоста в кластере — ALIVE).</li><li>`DEAD`: Кластер не работает ([Host.health](#Host) для каждого узла в кластере — DEAD).</li><li>`DEGRADED`: Кластер работает неоптимально ([Host.health](#Host) по крайней мере для одного узла в кластере не ALIVE).</li><ul/>
status | enum **Status**<br>Текущее состояние кластера. <ul><li>`STATUS_UNKNOWN`: Состояние кластера неизвестно.</li><li>`CREATING`: Кластер создается.</li><li>`RUNNING`: Кластер работает нормально.</li><li>`ERROR`: На кластере произошла ошибка, блокирующая работу.</li><li>`UPDATING`: Кластер изменяется.</li><li>`STOPPING`: Кластер останавливается.</li><li>`STOPPED`: Кластер остановлен.</li><li>`STARTING`: Кластер запускается.</li><ul/>


## AddZookeeper {#AddZookeeper}

Добавляет подкластер ZooKeeper в указанный кластер ClickHouse.

**rpc AddZookeeper ([AddClusterZookeeperRequest](#AddClusterZookeeperRequest)) returns ([operation.Operation](#Operation6))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[AddClusterZookeeperMetadata](#AddClusterZookeeperMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[Cluster](#Cluster7)<br>

### AddClusterZookeeperRequest {#AddClusterZookeeperRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse, который следует изменить.  Максимальная длина строки в символах — 50.
resources | **[Resources](#Resources)**<br>Ресурсы, выделенные хостам ZooKeeper. 
host_specs[] | **[HostSpec](#HostSpec1)**<br>Конфигурация хостов ZooKeeper. 


### Resources {#Resources}

Поле | Описание
--- | ---
resource_preset_id | **string**<br>Идентификатор набора вычислительных ресурсов, доступных хосту (процессор, память и т. д.). Все доступные наборы ресурсов перечислены в [документации](/docs/managed-clickhouse/concepts/instance-types). 
disk_size | **int64**<br>Объем хранилища, доступного хосту, в байтах. 
disk_type_id | **string**<br><ul><li>network-hdd — стандартное сетевое хранилище; </li><li>network-ssd — быстрое сетевое хранилище; </li><li>local-ssd — быстрое локальное хранилище.</li></ul> 


### HostSpec {#HostSpec}

Поле | Описание
--- | ---
zone_id | **string**<br>Идентификатор зоны доступности, в которой находится хост. Чтобы получить список доступных зон, используйте запрос [yandex.cloud.compute.v1.ZoneService.List](/docs/compute/grpc/zone_service#List). Максимальная длина строки в символах — 50.
type | **[Host.Type](#Host)**<br>Обязательное поле. Тип развертываемого хоста. 
subnet_id | **string**<br>Идентификатор подсети, к которой должен принадлежать хост. Эта подсеть должна быть частью сети, к которой принадлежит кластер. Идентификатор сети задается в поле [Cluster.network_id](#Cluster7). Максимальная длина строки в символах — 50.
assign_public_ip | **bool**<br><ul><li>false — не назначать хосту публичный IP-адрес. </li><li>true — у хоста должен быть публичный IP-адрес.</li></ul> 
shard_name | **string**<br>Имя шарда, которому принадлежит хост. Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_-]* `.


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[AddClusterZookeeperMetadata](#AddClusterZookeeperMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[Cluster](#Cluster7)>**<br>в случае успешного выполнения операции. 


### AddClusterZookeeperMetadata {#AddClusterZookeeperMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор кластера ClickHouse. 


### Cluster {#Cluster}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор кластера ClickHouse. Этот идентификатор генерирует MDB при создании. 
folder_id | **string**<br>Идентификатор каталога, которому принадлежит кластер ClickHouse. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) . 
name | **string**<br>Имя кластера ClickHouse. Имя уникально в рамках каталога. Длина 1-63 символов. 
description | **string**<br>Описание кластера ClickHouse. Длина описания должна быть от 0 до 256 символов. 
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse в виде пар ``key:value``. Максимум 64 на ресурс. 
environment | enum **Environment**<br>Среда развертывания кластера ClickHouse. <ul><li>`PRODUCTION`: Стабильная среда с осторожной политикой обновления: во время регулярного обслуживания применяются только срочные исправления.</li><li>`PRESTABLE`: Среда с более агрессивной политикой обновления: новые версии развертываются независимо от обратной совместимости.</li><ul/>
monitoring[] | **[Monitoring](#Monitoring2)**<br>Описание систем мониторинга, относящихся к данному кластеру ClickHouse. 
config | **[ClusterConfig](#ClusterConfig2)**<br>Конфигурация кластера ClickHouse. 
network_id | **string**<br>Идентификатор сети, к которой принадлежит кластер. 
health | enum **Health**<br>Агрегированная работоспособность кластера. <ul><li>`HEALTH_UNKNOWN`: Состояние кластера неизвестно ([Host.health](#Host) для каждого хоста в кластере — UNKNOWN).</li><li>`ALIVE`: Кластер работает нормально ([Host.health](#Host) для каждого хоста в кластере — ALIVE).</li><li>`DEAD`: Кластер не работает ([Host.health](#Host) для каждого узла в кластере — DEAD).</li><li>`DEGRADED`: Кластер работает неоптимально ([Host.health](#Host) по крайней мере для одного узла в кластере не ALIVE).</li><ul/>
status | enum **Status**<br>Текущее состояние кластера. <ul><li>`STATUS_UNKNOWN`: Состояние кластера неизвестно.</li><li>`CREATING`: Кластер создается.</li><li>`RUNNING`: Кластер работает нормально.</li><li>`ERROR`: На кластере произошла ошибка, блокирующая работу.</li><li>`UPDATING`: Кластер изменяется.</li><li>`STOPPING`: Кластер останавливается.</li><li>`STOPPED`: Кластер остановлен.</li><li>`STARTING`: Кластер запускается.</li><ul/>


## Backup {#Backup}

Создает резервную копию для указанного кластера ClickHouse.

**rpc Backup ([BackupClusterRequest](#BackupClusterRequest)) returns ([operation.Operation](#Operation7))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[BackupClusterMetadata](#BackupClusterMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[Cluster](#Cluster8)<br>

### BackupClusterRequest {#BackupClusterRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse, для которого следует создать резервную копию. Чтобы получить идентификатор кластера ClickHouse, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[BackupClusterMetadata](#BackupClusterMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[Cluster](#Cluster8)>**<br>в случае успешного выполнения операции. 


### BackupClusterMetadata {#BackupClusterMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор кластера ClickHouse, для которого выполняется резервное копирование. 


### Cluster {#Cluster}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор кластера ClickHouse. Этот идентификатор генерирует MDB при создании. 
folder_id | **string**<br>Идентификатор каталога, которому принадлежит кластер ClickHouse. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) . 
name | **string**<br>Имя кластера ClickHouse. Имя уникально в рамках каталога. Длина 1-63 символов. 
description | **string**<br>Описание кластера ClickHouse. Длина описания должна быть от 0 до 256 символов. 
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse в виде пар ``key:value``. Максимум 64 на ресурс. 
environment | enum **Environment**<br>Среда развертывания кластера ClickHouse. <ul><li>`PRODUCTION`: Стабильная среда с осторожной политикой обновления: во время регулярного обслуживания применяются только срочные исправления.</li><li>`PRESTABLE`: Среда с более агрессивной политикой обновления: новые версии развертываются независимо от обратной совместимости.</li><ul/>
monitoring[] | **[Monitoring](#Monitoring2)**<br>Описание систем мониторинга, относящихся к данному кластеру ClickHouse. 
config | **[ClusterConfig](#ClusterConfig2)**<br>Конфигурация кластера ClickHouse. 
network_id | **string**<br>Идентификатор сети, к которой принадлежит кластер. 
health | enum **Health**<br>Агрегированная работоспособность кластера. <ul><li>`HEALTH_UNKNOWN`: Состояние кластера неизвестно ([Host.health](#Host) для каждого хоста в кластере — UNKNOWN).</li><li>`ALIVE`: Кластер работает нормально ([Host.health](#Host) для каждого хоста в кластере — ALIVE).</li><li>`DEAD`: Кластер не работает ([Host.health](#Host) для каждого узла в кластере — DEAD).</li><li>`DEGRADED`: Кластер работает неоптимально ([Host.health](#Host) по крайней мере для одного узла в кластере не ALIVE).</li><ul/>
status | enum **Status**<br>Текущее состояние кластера. <ul><li>`STATUS_UNKNOWN`: Состояние кластера неизвестно.</li><li>`CREATING`: Кластер создается.</li><li>`RUNNING`: Кластер работает нормально.</li><li>`ERROR`: На кластере произошла ошибка, блокирующая работу.</li><li>`UPDATING`: Кластер изменяется.</li><li>`STOPPING`: Кластер останавливается.</li><li>`STOPPED`: Кластер остановлен.</li><li>`STARTING`: Кластер запускается.</li><ul/>


## Restore {#Restore}

Создает новый кластер ClickHouse с использованием указанной резервной копии.

**rpc Restore ([RestoreClusterRequest](#RestoreClusterRequest)) returns ([operation.Operation](#Operation8))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[RestoreClusterMetadata](#RestoreClusterMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[Cluster](#Cluster9)<br>

### RestoreClusterRequest {#RestoreClusterRequest}

Поле | Описание
--- | ---
backup_id | **string**<br>Обязательное поле. Идентификатор резервной копии, из которой следует создать кластер. Чтобы получить идентификатор резервной копии, используйте запрос [ClusterService.ListBackups](#ListBackups). 
name | **string**<br>Обязательное поле. Имя нового кластера ClickHouse. Имя должно быть уникальным в каталоге.  Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_-]* `.
description | **string**<br>Описание нового кластера ClickHouse. Максимальная длина строки в символах — 256.
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse `key:value` пары. Максимум 64 на ресурс. Например, "project": "mvp" или "source": "dictionary". Не более 64 на ресурс. Максимальная длина строки в символах для каждого значения — 63. Каждое значение должно соответствовать регулярному выражению ` [-_0-9a-z]* `. Максимальная длина строки в символах для каждого ключа — 63. Каждый ключ должен соответствовать регулярному выражению ` [a-z][-_0-9a-z]* `.
environment | **[Cluster.Environment](#Cluster9)**<br>Обязательное поле. Среда развертывания для нового кластера ClickHouse. 
config_spec | **[ConfigSpec](#ConfigSpec2)**<br>Обязательное поле. Конфигурация для создаваемого кластера ClickHouse. 
host_specs[] | **[HostSpec](#HostSpec2)**<br>Конфигурации для хостов ClickHouse, которые должны быть созданы для кластера, создаваемого из резервной копии. Количество элементов должно быть больше 0.
network_id | **string**<br>Обязательное поле. Идентификатор сети, в которой нужно создать кластер.  Максимальная длина строки в символах — 50.


### ConfigSpec {#ConfigSpec}

Поле | Описание
--- | ---
version | **string**<br>Версия серверного программного обеспечения ClickHouse. 
clickhouse | **[Clickhouse](#Clickhouse4)**<br>Конфигурация и ресурсы для сервера ClickHouse. 
zookeeper | **[Zookeeper](#Zookeeper4)**<br>Конфигурация и ресурсы для сервера ZooKeeper. 
backup_window_start | **[google.type.TimeOfDay](https://github.com/googleapis/googleapis/blob/master/google/type/timeofday.proto)**<br>Время запуска ежедневного резервного копирования, в часовом поясе UTC. 


### Clickhouse {#Clickhouse}

Поле | Описание
--- | ---
config | **`config.ClickhouseConfig`**<br>Конфигурация для сервера ClickHouse. 
resources | **[Resources](#Resources1)**<br>Ресурсы, выделенные хостам ClickHouse. 


### Zookeeper {#Zookeeper}

Поле | Описание
--- | ---
resources | **[Resources](#Resources1)**<br>Ресурсы, выделенные хостам ZooKeeper. Если не задано, будет использоваться минимальный доступный набор ресурсов. Все доступные наборы ресурсов можно получить с помощью запроса [ResourcePresetService.List](./resource_preset_service#List). 


### HostSpec {#HostSpec}

Поле | Описание
--- | ---
zone_id | **string**<br>Идентификатор зоны доступности, в которой находится хост. Чтобы получить список доступных зон, используйте запрос [yandex.cloud.compute.v1.ZoneService.List](/docs/compute/grpc/zone_service#List). Максимальная длина строки в символах — 50.
type | **[Host.Type](#Host)**<br>Обязательное поле. Тип развертываемого хоста. 
subnet_id | **string**<br>Идентификатор подсети, к которой должен принадлежать хост. Эта подсеть должна быть частью сети, к которой принадлежит кластер. Идентификатор сети задается в поле [Cluster.network_id](#Cluster9). Максимальная длина строки в символах — 50.
assign_public_ip | **bool**<br><ul><li>false — не назначать хосту публичный IP-адрес. </li><li>true — у хоста должен быть публичный IP-адрес.</li></ul> 
shard_name | **string**<br>Имя шарда, которому принадлежит хост. Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_-]* `.


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[RestoreClusterMetadata](#RestoreClusterMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[Cluster](#Cluster9)>**<br>в случае успешного выполнения операции. 


### RestoreClusterMetadata {#RestoreClusterMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор нового кластера ClickHouse, создаваемого из резервной копии. 
backup_id | **string**<br>Идентификатор резервной копии, используемой для создания кластера. 


### Cluster {#Cluster}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор кластера ClickHouse. Этот идентификатор генерирует MDB при создании. 
folder_id | **string**<br>Идентификатор каталога, которому принадлежит кластер ClickHouse. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) . 
name | **string**<br>Имя кластера ClickHouse. Имя уникально в рамках каталога. Длина 1-63 символов. 
description | **string**<br>Описание кластера ClickHouse. Длина описания должна быть от 0 до 256 символов. 
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse в виде пар ``key:value``. Максимум 64 на ресурс. 
environment | enum **Environment**<br>Среда развертывания кластера ClickHouse. <ul><li>`PRODUCTION`: Стабильная среда с осторожной политикой обновления: во время регулярного обслуживания применяются только срочные исправления.</li><li>`PRESTABLE`: Среда с более агрессивной политикой обновления: новые версии развертываются независимо от обратной совместимости.</li><ul/>
monitoring[] | **[Monitoring](#Monitoring2)**<br>Описание систем мониторинга, относящихся к данному кластеру ClickHouse. 
config | **[ClusterConfig](#ClusterConfig2)**<br>Конфигурация кластера ClickHouse. 
network_id | **string**<br>Идентификатор сети, к которой принадлежит кластер. 
health | enum **Health**<br>Агрегированная работоспособность кластера. <ul><li>`HEALTH_UNKNOWN`: Состояние кластера неизвестно ([Host.health](#Host) для каждого хоста в кластере — UNKNOWN).</li><li>`ALIVE`: Кластер работает нормально ([Host.health](#Host) для каждого хоста в кластере — ALIVE).</li><li>`DEAD`: Кластер не работает ([Host.health](#Host) для каждого узла в кластере — DEAD).</li><li>`DEGRADED`: Кластер работает неоптимально ([Host.health](#Host) по крайней мере для одного узла в кластере не ALIVE).</li><ul/>
status | enum **Status**<br>Текущее состояние кластера. <ul><li>`STATUS_UNKNOWN`: Состояние кластера неизвестно.</li><li>`CREATING`: Кластер создается.</li><li>`RUNNING`: Кластер работает нормально.</li><li>`ERROR`: На кластере произошла ошибка, блокирующая работу.</li><li>`UPDATING`: Кластер изменяется.</li><li>`STOPPING`: Кластер останавливается.</li><li>`STOPPED`: Кластер остановлен.</li><li>`STARTING`: Кластер запускается.</li><ul/>


## ListLogs {#ListLogs}

Получает логи для указанного кластера ClickHouse.

**rpc ListLogs ([ListClusterLogsRequest](#ListClusterLogsRequest)) returns ([ListClusterLogsResponse](#ListClusterLogsResponse))**

### ListClusterLogsRequest {#ListClusterLogsRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse, для которого следует запросить логи. Чтобы получить идентификатор кластера ClickHouse, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.
column_filter[] | **string**<br>Столбцы из таблицы логов, которые нужно запросить. Если столбцы не указаны, записи логов возвращаются целиком. 
service_type | enum **ServiceType**<br>Тип сервиса, для которого следует запросить логи. <ul><li>`CLICKHOUSE`: Логи работы ClickHouse.</li><ul/>
from_time | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Начало периода, для которого следует запросить логи, в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
to_time | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Конец периода, для которого следует запросить логи, в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
page_size | **int64**<br>Максимальное количество результатов на странице ответа на запрос. Если количество результатов больше чем `page_size`, сервис вернет значение [ListClusterLogsResponse.next_page_token](#ListClusterLogsResponse), которое можно использовать для получения следующей страницы. Максимальное значение — 1000.
page_token | **string**<br>Токен страницы. Установите значение `page_token` равным значению поля [ListClusterLogsResponse.next_page_token](#ListClusterLogsResponse) предыдущего запроса, чтобы получить следующую страницу результатов. Максимальная длина строки в символах — 100.


### ListClusterLogsResponse {#ListClusterLogsResponse}

Поле | Описание
--- | ---
logs[] | **[LogRecord](#LogRecord)**<br>Запрошенные записи логов. 
next_page_token | **string**<br>Токен для получения следующей страницы результатов в ответе. Если количество результатов больше чем [ListClusterLogsRequest.page_size](#ListClusterLogsRequest1), используйте `next_page_token` в качестве значения параметра [ListClusterLogsRequest.page_token](#ListClusterLogsRequest1) в следующем запросе списка ресурсов. Все последующие запросы будут получать свои значения `next_page_token` для перебора страниц результатов. 


### LogRecord {#LogRecord}

Поле | Описание
--- | ---
timestamp | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Отметка времени для записи журнала в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) текстовом формате. 
message | **map<string,string>**<br>Содержание записи журнала. 


## ListOperations {#ListOperations}

Получает список ресурсов Operation для указанного кластера.

**rpc ListOperations ([ListClusterOperationsRequest](#ListClusterOperationsRequest)) returns ([ListClusterOperationsResponse](#ListClusterOperationsResponse))**

### ListClusterOperationsRequest {#ListClusterOperationsRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор ресурса Cluster для ClickHouse, для которого запрашивается список операций.  Максимальная длина строки в символах — 50.
page_size | **int64**<br>Максимальное количество результатов на странице ответа на запрос. Если количество результатов больше чем `page_size`, сервис вернет значение [ListClusterOperationsResponse.next_page_token](#ListClusterOperationsResponse), которое можно использовать для получения следующей страницы. Максимальное значение — 1000.
page_token | **string**<br>Токен страницы. Установите значение `page_token` равным значению поля [ListClusterOperationsResponse.next_page_token](#ListClusterOperationsResponse) предыдущего запроса, чтобы получить следующую страницу результатов. Максимальная длина строки в символах — 100.


### ListClusterOperationsResponse {#ListClusterOperationsResponse}

Поле | Описание
--- | ---
operations[] | **[operation.Operation](#Operation9)**<br>Список ресурсов Operation для указанного кластера ClickHouse. 
next_page_token | **string**<br>Токен для получения следующей страницы результатов в ответе. Если количество результатов больше чем [ListClusterOperationsRequest.page_size](#ListClusterOperationsRequest1), используйте `next_page_token` в качестве значения параметра [ListClusterOperationsRequest.page_token](#ListClusterOperationsRequest1) в следующем запросе списка ресурсов. Все последующие запросы будут получать свои значения `next_page_token` для перебора страниц результатов. 


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)**<br>Результат операции в случае успешного завершения. Если исходный метод не возвращает никаких данных при успешном завершении, например метод Delete, поле содержит объект [google.protobuf.Empty](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#google.protobuf.Empty). Если исходный метод — это стандартный метод Create / Update, поле содержит целевой ресурс операции. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `response`. 


## ListBackups {#ListBackups}

Получает список доступных резервных копий для указанного кластера ClickHouse.

**rpc ListBackups ([ListClusterBackupsRequest](#ListClusterBackupsRequest)) returns ([ListClusterBackupsResponse](#ListClusterBackupsResponse))**

### ListClusterBackupsRequest {#ListClusterBackupsRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse. Чтобы получить идентификатор кластера ClickHouse, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.
page_size | **int64**<br>Максимальное количество результатов на странице ответа на запрос. Если количество результатов больше чем `page_size`, сервис вернет значение [ListClusterBackupsResponse.next_page_token](#ListClusterBackupsResponse), которое можно использовать для получения следующей страницы. Максимальное значение — 1000.
page_token | **string**<br>Токен страницы. Установите значение `page_token` равным значению поля [ListClusterBackupsResponse.next_page_token](#ListClusterBackupsResponse) предыдущего запроса, чтобы получить следующую страницу результатов. Максимальная длина строки в символах — 100.


### ListClusterBackupsResponse {#ListClusterBackupsResponse}

Поле | Описание
--- | ---
backups[] | **[Backup](#Backup)**<br>Список ресурсов Backup для ClickHouse. 
next_page_token | **string**<br>Токен для получения следующей страницы результатов в ответе. Если количество результатов больше чем [ListClusterBackupsRequest.page_size](#ListClusterBackupsRequest1), используйте `next_page_token` в качестве значения параметра [ListClusterBackupsRequest.page_token](#ListClusterBackupsRequest1) в следующем запросе списка ресурсов. Все последующие запросы будут получать свои значения `next_page_token` для перебора страниц результатов. 


### Backup {#Backup}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор резервной копии. 
folder_id | **string**<br>Идентификатор каталога, которому принадлежит резервная копия. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) (т. е. когда операция резервного копирования была завершена). 
source_cluster_id | **string**<br>Идентификатор кластера ClickHouse, для которого была создана резервная копия. 
source_shard_names[] | **string**<br>Имена шардов, включенных в резервную копию. 
started_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время запуска операции резервного копирования. 


## ListHosts {#ListHosts}

Получает список хостов для указанного кластера.

**rpc ListHosts ([ListClusterHostsRequest](#ListClusterHostsRequest)) returns ([ListClusterHostsResponse](#ListClusterHostsResponse))**

### ListClusterHostsRequest {#ListClusterHostsRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse. Чтобы получить идентификатор кластера ClickHouse, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.
page_size | **int64**<br>Максимальное количество результатов на странице ответа на запрос. Если количество результатов больше чем `page_size`, сервис вернет значение [ListClusterHostsResponse.next_page_token](#ListClusterHostsResponse), которое можно использовать для получения следующей страницы. Максимальное значение — 1000.
page_token | **string**<br>Токен страницы. Установите значение `page_token` равным значению поля [ListClusterHostsResponse.next_page_token](#ListClusterHostsResponse) предыдущего запроса, чтобы получить следующую страницу результатов. Максимальная длина строки в символах — 100.


### ListClusterHostsResponse {#ListClusterHostsResponse}

Поле | Описание
--- | ---
hosts[] | **[Host](#Host)**<br>Запрошенный список хостов для кластера. 
next_page_token | **string**<br>Токен для получения следующей страницы результатов в ответе. Если количество результатов больше чем [ListClusterHostsRequest.page_size](#ListClusterHostsRequest1), используйте `next_page_token` в качестве значения параметра [ListClusterHostsRequest.page_token](#ListClusterHostsRequest1) в следующем запросе списка ресурсов. Все последующие запросы будут получать свои значения `next_page_token` для перебора страниц результатов. 


### Host {#Host}

Поле | Описание
--- | ---
name | **string**<br>Имя хоста ClickHouse. Имя хоста назначается MDB во время создания и не может быть изменено. Длина 1-63 символов. <br>Имя уникально для всех существующих хостов MDB в Яндекс.Облаке, так как оно определяет полное доменное имя (FQDN) хоста. 
cluster_id | **string**<br>Идентификатор хоста ClickHouse. Этот идентификатор генерирует MDB при создании. 
zone_id | **string**<br>Идентификатор зоны доступности, в которой находится хост ClickHouse. 
type | enum **Type**<br>Тип хоста. <ul><li>`CLICKHOUSE`: Хост ClickHouse.</li><li>`ZOOKEEPER`: Хост ZooKeeper.</li><ul/>
resources | **[Resources](#Resources1)**<br>Ресурсы, выделенные хосту ClickHouse. 
health | enum **Health**<br>Код работоспособности хоста. <ul><li>`UNKNOWN`: Состояние хоста неизвестно.</li><li>`ALIVE`: Хозяин выполняет все свои функции нормально.</li><li>`DEAD`: Хост не работает и не может выполнять свои основные функции.</li><li>`DEGRADED`: Хост деградировал, и может выполнять только некоторые из своих основных функций.</li><ul/>
services[] | **[Service](#Service)**<br>Сервисы, предоставляемые хостом. 
subnet_id | **string**<br>Идентификатор подсети, к которой принадлежит хост. 
assign_public_ip | **bool**<br>Флаг, показывающий статус публичного IP-адреса для этого хоста. 
shard_name | **string**<br> 


### Resources {#Resources}

Поле | Описание
--- | ---
resource_preset_id | **string**<br>Идентификатор набора вычислительных ресурсов, доступных хосту (процессор, память и т. д.). Все доступные наборы ресурсов перечислены в [документации](/docs/managed-clickhouse/concepts/instance-types). 
disk_size | **int64**<br>Объем хранилища, доступного хосту, в байтах. 
disk_type_id | **string**<br><ul><li>network-hdd — стандартное сетевое хранилище; </li><li>network-ssd — быстрое сетевое хранилище; </li><li>local-ssd — быстрое локальное хранилище.</li></ul> 


### Service {#Service}

Поле | Описание
--- | ---
type | enum **Type**<br>Тип сервиса, предоставляемого хостом. <ul><li>`CLICKHOUSE`: Хост — это сервер ClickHouse.</li><li>`ZOOKEEPER`: Хост — сервер ZooKeeper.</li><ul/>
health | enum **Health**<br>Код состояния доступности сервера. <ul><li>`UNKNOWN`: Работоспособность сервера неизвестна.</li><li>`ALIVE`: Сервер работает нормально.</li><li>`DEAD`: Сервер отключен или не отвечает.</li><ul/>


## AddHosts {#AddHosts}

Создает новые хосты для кластера.

**rpc AddHosts ([AddClusterHostsRequest](#AddClusterHostsRequest)) returns ([operation.Operation](#Operation10))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[AddClusterHostsMetadata](#AddClusterHostsMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[google.protobuf.Empty](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#google.protobuf.Empty)<br>

### AddClusterHostsRequest {#AddClusterHostsRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse, для которого следует добавить хосты. Чтобы получить идентификатор кластера ClickHouse, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.
host_specs[] | **[HostSpec](#HostSpec3)**<br>Конфигурации для хостов ClickHouse, которые должны быть добавлены в кластер. Количество элементов должно быть больше 0.


### HostSpec {#HostSpec}

Поле | Описание
--- | ---
zone_id | **string**<br>Идентификатор зоны доступности, в которой находится хост. Чтобы получить список доступных зон, используйте запрос [yandex.cloud.compute.v1.ZoneService.List](/docs/compute/grpc/zone_service#List). Максимальная длина строки в символах — 50.
type | **[Host.Type](#Host1)**<br>Обязательное поле. Тип развертываемого хоста. 
subnet_id | **string**<br>Идентификатор подсети, к которой должен принадлежать хост. Эта подсеть должна быть частью сети, к которой принадлежит кластер. Идентификатор сети задается в поле [Cluster.network_id](#Cluster10). Максимальная длина строки в символах — 50.
assign_public_ip | **bool**<br><ul><li>false — не назначать хосту публичный IP-адрес. </li><li>true — у хоста должен быть публичный IP-адрес.</li></ul> 
shard_name | **string**<br>Имя шарда, которому принадлежит хост. Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_-]* `.


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[AddClusterHostsMetadata](#AddClusterHostsMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[google.protobuf.Empty](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#google.protobuf.Empty)>**<br>в случае успешного выполнения операции. 


### AddClusterHostsMetadata {#AddClusterHostsMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор кластера ClickHouse, в который добавляются хосты. 
host_names[] | **string**<br>Имена хостов, добавляемых в кластер. 


## DeleteHosts {#DeleteHosts}

Удаляет указанные хосты кластера.

**rpc DeleteHosts ([DeleteClusterHostsRequest](#DeleteClusterHostsRequest)) returns ([operation.Operation](#Operation11))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[DeleteClusterHostsMetadata](#DeleteClusterHostsMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[google.protobuf.Empty](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#google.protobuf.Empty)<br>

### DeleteClusterHostsRequest {#DeleteClusterHostsRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse из которого следует удалить хосты. Чтобы получить идентификатор кластера ClickHouse, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.
host_names[] | **string**<br>Имена хостов, которые следует удалить. Количество элементов должно быть больше 0. Максимальная длина строки в символах для каждого значения — 253.


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[DeleteClusterHostsMetadata](#DeleteClusterHostsMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[google.protobuf.Empty](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#google.protobuf.Empty)>**<br>в случае успешного выполнения операции. 


### DeleteClusterHostsMetadata {#DeleteClusterHostsMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор кластера ClickHouse из которого следует удалить хосты. 
host_names[] | **string**<br>Имена удаляемых хостов. 


## GetShard {#GetShard}

Возвращает указанный шард.

**rpc GetShard ([GetClusterShardRequest](#GetClusterShardRequest)) returns ([Shard](#Shard))**

### GetClusterShardRequest {#GetClusterShardRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера, к которому принадлежит шард. Чтобы получить идентификатор кластера, используйте запрос [ClusterService.List](#List)(#List). Чтобы получить имя базы данных, используйте запрос [ClusterService.List].  Максимальная длина строки в символах — 50.
shard_name | **string**<br>Обязательное поле. Имя шарда, информацию о котором нужно запросить. Чтобы получить имя шарда, используйте запрос [ClusterService.ListShards](#ListShards).  Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_-]* `.


### Shard {#Shard}

Поле | Описание
--- | ---
name | **string**<br>Имя шарда. 
cluster_id | **string**<br>Идентификатор кластера, к которому принадлежит шард. 
config | **[ShardConfig](#ShardConfig)**<br>Конфигурация шарда. 


### ShardConfig {#ShardConfig}

Поле | Описание
--- | ---
clickhouse | **[Clickhouse](#Clickhouse5)**<br>Конфигурация ClickHouse для шарда. 


### Clickhouse {#Clickhouse}

Поле | Описание
--- | ---
config | **`config.ClickhouseConfigSet`**<br>Настройки ClickHouse для шарда. 
resources | **[Resources](#Resources2)**<br>Вычислительные ресурсы для шарда. 
weight | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>Относительный вес шарда, который учитывается при записи данных в кластер. Подробнее см. в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/table_engines/distributed/). 


## ListShards {#ListShards}

Получает список шардов, принадлежащих указанному кластеру.

**rpc ListShards ([ListClusterShardsRequest](#ListClusterShardsRequest)) returns ([ListClusterShardsResponse](#ListClusterShardsResponse))**

### ListClusterShardsRequest {#ListClusterShardsRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse, для которого нужно вывести список шардов. Чтобы получить идентификатор кластера, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.
page_size | **int64**<br>Максимальное количество результатов на странице ответа на запрос. Если количество результатов больше чем `page_size`, сервис вернет значение [ListClusterShardsResponse.next_page_token](#ListClusterShardsResponse), которое можно использовать для получения следующей страницы. Допустимые значения — от 0 до 1000 включительно.
page_token | **string**<br>Номер страницы. Чтобы получить следующую страницу результатов, установите значение `page_token` равным значению поля [ListClusterShardsResponse.next_page_token](#ListClusterShardsResponse) прошлого запроса. Максимальная длина строки в символах — 100.


### ListClusterShardsResponse {#ListClusterShardsResponse}

Поле | Описание
--- | ---
shards[] | **[Shard](#Shard1)**<br>Список шардов ClickHouse. 
next_page_token | **string**<br>Токен для получения следующей страницы результатов в ответе. Если количество результатов больше чем [ListClusterShardsRequest.page_size](#ListClusterShardsRequest1), используйте `next_page_token` в качестве значения параметра [ListClusterShardsRequest.page_token](#ListClusterShardsRequest1) в следующем запросе списка ресурсов. Все последующие запросы будут получать свои значения `next_page_token` для перебора страниц результатов. 


### Shard {#Shard}

Поле | Описание
--- | ---
name | **string**<br>Имя шарда. 
cluster_id | **string**<br>Идентификатор кластера, к которому принадлежит шард. 
config | **[ShardConfig](#ShardConfig1)**<br>Конфигурация шарда. 


### ShardConfig {#ShardConfig}

Поле | Описание
--- | ---
clickhouse | **[Clickhouse](#Clickhouse6)**<br>Конфигурация ClickHouse для шарда. 


### Clickhouse {#Clickhouse}

Поле | Описание
--- | ---
config | **`config.ClickhouseConfigSet`**<br>Настройки ClickHouse для шарда. 
resources | **[Resources](#Resources2)**<br>Вычислительные ресурсы для шарда. 
weight | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>Относительный вес шарда, который учитывается при записи данных в кластер. Подробнее см. в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/table_engines/distributed/). 


## AddShard {#AddShard}

Создает новый шард в указанном кластере.

**rpc AddShard ([AddClusterShardRequest](#AddClusterShardRequest)) returns ([operation.Operation](#Operation12))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[AddClusterShardMetadata](#AddClusterShardMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[Shard](#Shard2)<br>

### AddClusterShardRequest {#AddClusterShardRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse, к которому нужно добавить шард. Чтобы получить идентификатор кластера ClickHouse, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.
shard_name | **string**<br>Обязательное поле. Имя нового шарда.  Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_-]* `.
config_spec | **[ShardConfigSpec](#ShardConfigSpec)**<br>Конфигурация нового шарда. 
host_specs[] | **[HostSpec](#HostSpec4)**<br>Конфигурации для хостов ClickHouse, которые должны быть созданы вместе с шардом. Количество элементов должно быть больше 0.


### ShardConfigSpec {#ShardConfigSpec}

Поле | Описание
--- | ---
clickhouse | **[Clickhouse](#Clickhouse7)**<br>Конфигурация ClickHouse для шарда. 


### Clickhouse {#Clickhouse}

Поле | Описание
--- | ---
config | **`config.ClickhouseConfig`**<br>Настройки ClickHouse для шарда. 
resources | **[Resources](#Resources2)**<br>Вычислительные ресурсы для шарда. 
weight | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>Относительный вес шарда, который учитывается при записи данных в кластер. Подробнее см. в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/table_engines/distributed/). 


### HostSpec {#HostSpec}

Поле | Описание
--- | ---
zone_id | **string**<br>Идентификатор зоны доступности, в которой находится хост. Чтобы получить список доступных зон, используйте запрос [yandex.cloud.compute.v1.ZoneService.List](/docs/compute/grpc/zone_service#List). Максимальная длина строки в символах — 50.
type | **[Host.Type](#Host1)**<br>Обязательное поле. Тип развертываемого хоста. 
subnet_id | **string**<br>Идентификатор подсети, к которой должен принадлежать хост. Эта подсеть должна быть частью сети, к которой принадлежит кластер. Идентификатор сети задается в поле [Cluster.network_id](#Cluster10). Максимальная длина строки в символах — 50.
assign_public_ip | **bool**<br><ul><li>false — не назначать хосту публичный IP-адрес. </li><li>true — у хоста должен быть публичный IP-адрес.</li></ul> 
shard_name | **string**<br>Имя шарда, которому принадлежит хост. Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_-]* `.


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[AddClusterShardMetadata](#AddClusterShardMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[Shard](#Shard2)>**<br>в случае успешного выполнения операции. 


### AddClusterShardMetadata {#AddClusterShardMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор кластера, в который добавляется шард. 
shard_name | **string**<br>Имя создаваемого шарда. 


### Shard {#Shard}

Поле | Описание
--- | ---
name | **string**<br>Имя шарда. 
cluster_id | **string**<br>Идентификатор кластера, к которому принадлежит шард. 
config | **[ShardConfig](#ShardConfig2)**<br>Конфигурация шарда. 


## UpdateShard {#UpdateShard}

Изменяет указанный шард.

**rpc UpdateShard ([UpdateClusterShardRequest](#UpdateClusterShardRequest)) returns ([operation.Operation](#Operation13))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[UpdateClusterShardMetadata](#UpdateClusterShardMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[Shard](#Shard3)<br>

### UpdateClusterShardRequest {#UpdateClusterShardRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse, которому принадлежит шард. Чтобы получить идентификатор кластера, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.
shard_name | **string**<br>Обязательное поле. Имя шарда, который следует изменить. Чтобы получить имя шарда, используйте запрос [ClusterService.ListShards](#ListShards).  Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_-]* `.
update_mask | **[google.protobuf.FieldMask](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/field-mask)**<br>Маска, которая указывает, какие атрибуты шарда должны быть изменены. 
config_spec | **[ShardConfigSpec](#ShardConfigSpec1)**<br>Новая конфигурация для указанного шарда. 


### ShardConfigSpec {#ShardConfigSpec}

Поле | Описание
--- | ---
clickhouse | **[Clickhouse](#Clickhouse8)**<br>Конфигурация ClickHouse для шарда. 


### Clickhouse {#Clickhouse}

Поле | Описание
--- | ---
config | **`config.ClickhouseConfig`**<br>Настройки ClickHouse для шарда. 
resources | **[Resources](#Resources2)**<br>Вычислительные ресурсы для шарда. 
weight | **[google.protobuf.Int64Value](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/int64-value)**<br>Относительный вес шарда, который учитывается при записи данных в кластер. Подробнее см. в [документации ClickHouse](https://clickhouse.yandex/docs/ru/operations/table_engines/distributed/). 


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[UpdateClusterShardMetadata](#UpdateClusterShardMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[Shard](#Shard3)>**<br>в случае успешного выполнения операции. 


### UpdateClusterShardMetadata {#UpdateClusterShardMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор кластера, содержащего обновляемый шард. 
shard_name | **string**<br>Имя обновляемого шарда. 


### Shard {#Shard}

Поле | Описание
--- | ---
name | **string**<br>Имя шарда. 
cluster_id | **string**<br>Идентификатор кластера, к которому принадлежит шард. 
config | **[ShardConfig](#ShardConfig2)**<br>Конфигурация шарда. 


## DeleteShard {#DeleteShard}

Удаляет указанный шард.

**rpc DeleteShard ([DeleteClusterShardRequest](#DeleteClusterShardRequest)) returns ([operation.Operation](#Operation14))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[DeleteClusterShardMetadata](#DeleteClusterShardMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[google.protobuf.Empty](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#google.protobuf.Empty)<br>

### DeleteClusterShardRequest {#DeleteClusterShardRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse, которому принадлежит шард. Чтобы получить идентификатор кластера, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.
shard_name | **string**<br>Обязательное поле. Имя удаляемого шарда. Чтобы получить имя шарда, используйте запрос [ClusterService.ListShards](#ListShards).  Максимальная длина строки в символах — 63. Значение должно соответствовать регулярному выражению ` [a-zA-Z0-9_-]* `.


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[DeleteClusterShardMetadata](#DeleteClusterShardMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[google.protobuf.Empty](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#google.protobuf.Empty)>**<br>в случае успешного выполнения операции. 


### DeleteClusterShardMetadata {#DeleteClusterShardMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор кластера, содержащего удаляемый шард. 
shard_name | **string**<br>Имя удаляемого шарда. 


## CreateExternalDictionary {#CreateExternalDictionary}

Создает внешний словарь для указанного кластера ClickHouse.

**rpc CreateExternalDictionary ([CreateClusterExternalDictionaryRequest](#CreateClusterExternalDictionaryRequest)) returns ([operation.Operation](#Operation15))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[CreateClusterExternalDictionaryMetadata](#CreateClusterExternalDictionaryMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[Cluster](#Cluster10)<br>

### CreateClusterExternalDictionaryRequest {#CreateClusterExternalDictionaryRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse, для которого следует создать внешний словарь. Чтобы получить идентификатор кластера, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.
external_dictionary | **[config.ClickhouseConfig.ExternalDictionary](#ClickhouseConfig)**<br>Конфигурация внешнего словаря. 


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[CreateClusterExternalDictionaryMetadata](#CreateClusterExternalDictionaryMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[Cluster](#Cluster10)>**<br>в случае успешного выполнения операции. 


### CreateClusterExternalDictionaryMetadata {#CreateClusterExternalDictionaryMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор кластера, для которого создается внешний словарь. 


### Cluster {#Cluster}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор кластера ClickHouse. Этот идентификатор генерирует MDB при создании. 
folder_id | **string**<br>Идентификатор каталога, которому принадлежит кластер ClickHouse. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) . 
name | **string**<br>Имя кластера ClickHouse. Имя уникально в рамках каталога. Длина 1-63 символов. 
description | **string**<br>Описание кластера ClickHouse. Длина описания должна быть от 0 до 256 символов. 
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse в виде пар ``key:value``. Максимум 64 на ресурс. 
environment | enum **Environment**<br>Среда развертывания кластера ClickHouse. <ul><li>`PRODUCTION`: Стабильная среда с осторожной политикой обновления: во время регулярного обслуживания применяются только срочные исправления.</li><li>`PRESTABLE`: Среда с более агрессивной политикой обновления: новые версии развертываются независимо от обратной совместимости.</li><ul/>
monitoring[] | **[Monitoring](#Monitoring2)**<br>Описание систем мониторинга, относящихся к данному кластеру ClickHouse. 
config | **[ClusterConfig](#ClusterConfig2)**<br>Конфигурация кластера ClickHouse. 
network_id | **string**<br>Идентификатор сети, к которой принадлежит кластер. 
health | enum **Health**<br>Агрегированная работоспособность кластера. <ul><li>`HEALTH_UNKNOWN`: Состояние кластера неизвестно ([Host.health](#Host1) для каждого хоста в кластере — UNKNOWN).</li><li>`ALIVE`: Кластер работает нормально ([Host.health](#Host1) для каждого хоста в кластере — ALIVE).</li><li>`DEAD`: Кластер не работает ([Host.health](#Host1) для каждого узла в кластере — DEAD).</li><li>`DEGRADED`: Кластер работает неоптимально ([Host.health](#Host1) по крайней мере для одного узла в кластере не ALIVE).</li><ul/>
status | enum **Status**<br>Текущее состояние кластера. <ul><li>`STATUS_UNKNOWN`: Состояние кластера неизвестно.</li><li>`CREATING`: Кластер создается.</li><li>`RUNNING`: Кластер работает нормально.</li><li>`ERROR`: На кластере произошла ошибка, блокирующая работу.</li><li>`UPDATING`: Кластер изменяется.</li><li>`STOPPING`: Кластер останавливается.</li><li>`STOPPED`: Кластер остановлен.</li><li>`STARTING`: Кластер запускается.</li><ul/>


## DeleteExternalDictionary {#DeleteExternalDictionary}

Удаляет указанный внешний словарь.

**rpc DeleteExternalDictionary ([DeleteClusterExternalDictionaryRequest](#DeleteClusterExternalDictionaryRequest)) returns ([operation.Operation](#Operation16))**

Метаданные и результат операции:<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.metadata:[DeleteClusterExternalDictionaryMetadata](#DeleteClusterExternalDictionaryMetadata)<br>
	&nbsp;&nbsp;&nbsp;&nbsp;Operation.response:[Cluster](#Cluster11)<br>

### DeleteClusterExternalDictionaryRequest {#DeleteClusterExternalDictionaryRequest}

Поле | Описание
--- | ---
cluster_id | **string**<br>Обязательное поле. Идентификатор кластера ClickHouse, для которого следует удалить внешний словарь. Чтобы получить идентификатор кластера, используйте запрос [ClusterService.List](#List).  Максимальная длина строки в символах — 50.
external_dictionary_name | **string**<br>Имя удаляемого внешнего словаря. 


### Operation {#Operation}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор операции. 
description | **string**<br>Описание операции. Длина описания должна быть от 0 до 256 символов. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания ресурса в формате в [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
created_by | **string**<br>Идентификатор пользователя или сервисного аккаунта, инициировавшего операцию. 
modified_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время, когда ресурс Operation последний раз обновлялся. Значение в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt). 
done | **bool**<br>Если значение равно `false` — операция еще выполняется. Если `true` — операция завершена, и задано значение одного из полей `error` или `response`. 
metadata | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[DeleteClusterExternalDictionaryMetadata](#DeleteClusterExternalDictionaryMetadata)>**<br>Метаданные операции. Обычно в поле содержится идентификатор ресурса, над которым выполняется операция. Если метод возвращает ресурс Operation, в описании метода приведена структура соответствующего ему поля `metadata`. 
result | **oneof:** `error` или `response`<br>Результат операции. Если `done == false` и не было выявлено ошибок — значения полей `error` и `response` не заданы. Если `done == false` и была выявлена ошибка — задано значение поля `error`. Если `done == true` — задано значение ровно одного из полей `error` или `response`.
&nbsp;&nbsp;error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**<br>Описание ошибки в случае сбоя или отмены операции. 
&nbsp;&nbsp;response | **[google.protobuf.Any](https://developers.google.com/protocol-buffers/docs/proto3#any)<[Cluster](#Cluster11)>**<br>в случае успешного выполнения операции. 


### DeleteClusterExternalDictionaryMetadata {#DeleteClusterExternalDictionaryMetadata}

Поле | Описание
--- | ---
cluster_id | **string**<br>Идентификатор кластера, для которого удаляется внешний словарь. 


### Cluster {#Cluster}

Поле | Описание
--- | ---
id | **string**<br>Идентификатор кластера ClickHouse. Этот идентификатор генерирует MDB при создании. 
folder_id | **string**<br>Идентификатор каталога, которому принадлежит кластер ClickHouse. 
created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**<br>Время создания в формате [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) . 
name | **string**<br>Имя кластера ClickHouse. Имя уникально в рамках каталога. Длина 1-63 символов. 
description | **string**<br>Описание кластера ClickHouse. Длина описания должна быть от 0 до 256 символов. 
labels | **map<string,string>**<br>Пользовательские метки для кластера ClickHouse в виде пар ``key:value``. Максимум 64 на ресурс. 
environment | enum **Environment**<br>Среда развертывания кластера ClickHouse. <ul><li>`PRODUCTION`: Стабильная среда с осторожной политикой обновления: во время регулярного обслуживания применяются только срочные исправления.</li><li>`PRESTABLE`: Среда с более агрессивной политикой обновления: новые версии развертываются независимо от обратной совместимости.</li><ul/>
monitoring[] | **[Monitoring](#Monitoring2)**<br>Описание систем мониторинга, относящихся к данному кластеру ClickHouse. 
config | **[ClusterConfig](#ClusterConfig2)**<br>Конфигурация кластера ClickHouse. 
network_id | **string**<br>Идентификатор сети, к которой принадлежит кластер. 
health | enum **Health**<br>Агрегированная работоспособность кластера. <ul><li>`HEALTH_UNKNOWN`: Состояние кластера неизвестно ([Host.health](#Host1) для каждого хоста в кластере — UNKNOWN).</li><li>`ALIVE`: Кластер работает нормально ([Host.health](#Host1) для каждого хоста в кластере — ALIVE).</li><li>`DEAD`: Кластер не работает ([Host.health](#Host1) для каждого узла в кластере — DEAD).</li><li>`DEGRADED`: Кластер работает неоптимально ([Host.health](#Host1) по крайней мере для одного узла в кластере не ALIVE).</li><ul/>
status | enum **Status**<br>Текущее состояние кластера. <ul><li>`STATUS_UNKNOWN`: Состояние кластера неизвестно.</li><li>`CREATING`: Кластер создается.</li><li>`RUNNING`: Кластер работает нормально.</li><li>`ERROR`: На кластере произошла ошибка, блокирующая работу.</li><li>`UPDATING`: Кластер изменяется.</li><li>`STOPPING`: Кластер останавливается.</li><li>`STOPPED`: Кластер остановлен.</li><li>`STARTING`: Кластер запускается.</li><ul/>



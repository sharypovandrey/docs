# Репликация

В кластерах {{ mmy-name }} используется [полусинхронная репликация](https://dev.mysql.com/doc/refman/5.7/en/replication-semisync.html): по умолчанию мастер ожидает завершения транзакции хотя бы на одной реплике. Отличие полусинхронной репликации {{mmy-name}} от стандартной схемы в том, что при отказе всех полусинхронных реплик кластер не переключается на асинхронную репликацию, а отключает репликацию совсем пока хотя бы одна реплика не станет доступна.

Также при репликации в {{ mmy-name }} используются глобальные идентификаторы транзакций ([GTID](https://dev.mysql.com/doc/refman/5.7/en/replication-gtids-concepts.html)), которые помогают поддерживать консистентность данных между хостами кластера.

{% include [non-replicating-hosts](../../_includes/mdb/non-replicating-hosts.md) %}


## Особенности репликации в {{mmy-name}}

Если кластер состоит из единственного хоста, или все реплики, кроме мастера, полностью недоступны (хосты в состоянии `DEAD`), репликация не работает. Как только в кластере появляется или становится доступной какая-либо реплика, полусинхронная репликация запускается поэтапно:

1. Когда реплика становится доступной, для нее сразу включается асинхронная репликация с мастера. Мастер при этом полностью доступен для чтения и записи, а отставание реплики постепенно сокращается.
1. Как только отставание становится меньше 100 МБ, включается синхронная репликация. Мастер становится недоступным для записи, пока данные не синхронизируются с репликой полностью — этот процесс может занимать от единиц до десятков секунд, в зависимости от производительности сети.
1. Как только данные на мастере и реплике полностью синхронизируются, хосты снова становятся полностью доступны и реплицируются синхронно.

https://developers.redhat.com/blog/2020/03/23/ceph-storage-monitoring-with-zabbix/

```
ceph -s
ceph mgr module ls
ceph mgr module enable zabbix
wget https://repo.zabbix.com/zabbix/5.1/debian/pool/main/z/zabbix-release/zabbix-release_5.1-1+debian9_all.deb
dpkg -i zabbix-release_5.1-1+debian9_all.deb
apt update
apt install zabbix-agent
apt install zabbix-sender
ceph zabbix config-set zabbix_host 172.16.34.89
ceph zabbix config-set identifier CEPH
ceph zabbix config-set interval 120
ceph zabbix config-show
```
Ручная отправка
будет работать тлько на ноде в состоянии active. 

ceph zabbix send

Поэтому на всех нодах нужно запустить модуль zabbix  с одинаковым identifier, а на
zabbix создать хост с таким именем


Проверить совсем вручную

/usr/bin/zabbix_sender -z 172.16.34.89 -p 10051 -s "CEPH" -k ceph.total_objects -o 43 -v

--------------------------------------------------------------------------------------------------

Примерный вывод

--------------------------------------------------------------------------------------------------

root@prox2:~# ceph -s
  cluster:
    id:     e81fcdec-da83-4d8a-afe3-30653272aa74
    health: HEALTH_ERR
            20 scrub errors
            Possible data damage: 15 pgs inconsistent

  services:
    mon: 4 daemons, quorum prox1,prox2,prox3,prox4
    mgr: prox2(active), standbys: prox1, prox4, prox3
    osd: 48 osds: 47 up, 47 in

  data:
    pools:   1 pools, 2048 pgs
    objects: 971.90k objects, 3.69TiB
    usage:   7.67TiB used, 30.8TiB / 38.5TiB avail
    pgs:     2032 active+clean
             15   active+clean+inconsistent
             1    active+clean+scrubbing+deep

  io:
    client:   750KiB/s rd, 4.02MiB/s wr, 20op/s rd, 241op/s wr


--------------------------------------------------------------------------------------------------


root@prox2:~# ceph mgr module ls
{
    "enabled_modules": [
        "balancer",
        "restful",
        "status",
        "zabbix"
    ],
    "disabled_modules": [
        "dashboard",
        "influx",
        "localpool",
        "prometheus",
        "selftest",
        "telemetry"
    ]
}


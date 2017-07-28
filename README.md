
##Journalctl na prática

Vou mostrar alguns exemplos de verificação de LOGs com o comando Journaulctl.

O Comando sem parâmetros irá mostrar TODOS os logs de todos os serviços e na primeira linha mostrará a data de início dos LOGs

```
# journalctl
-- Logs begin at Qui 2017-07-27 12:12:16 -03, end at Sex 2017-07-28 10:01:01 -03. --
```

Quando precisamos especificar um tempo de verificação dos LOGs de início:

Me mostre os LOGs de hoje desde às 09h da manhã:
```
# journalctl --since=09:00
```

Me mostre os LOGS desde o dia 27/07/2017:
```
# journalctl --since "2017-07-27 01:00:00"
```

Me mostre os LOGs desde o dia 27/07/2017 até o dia 28/07/2017 às 15h:
```
# journalctl --since "2017-07-27 01:00:00" --until "2017-07-28 15:00:00"
```

Obs: No parâmetro "--since", pode se usar: yesterday, today, tomorow, etc. A seguir a descrição do manual para consulta.

-S, --since=, -U, --until=
Start showing entries on or newer than the specified date, or on or older than the specified date, respectively. Date specifications should be of the format "2012-10-30 18:17:16". If the time part is omitted, "00:00:00" is assumed. If only the seconds component is omitted, ":00" is assumed. If the date component is omitted, the current day is assumed. Alternatively the strings "yesterday", "today", "tomorrow" are understood, which refer to 00:00:00 of the day before the current day, the current day, or the day after the current day, respectively. "now" refers to the current time. Finally, relative times may be specified, prefixed with "-" or "+", referring to times before or after the current time, respectively. For complete time and date specification, see systemd.time(7). Note that --output=short-full prints timestamps that follow precisely this format.

Você pode especificar o serviço que quer monitorar:
```
# journalctl -u nginx.service
```

Pode se especificar mais de um serviço:
```
# journalctl -u nginx.service -u sshd.service
```

Pode ser intercalado com o tempo:
```
# journalctl -u nginx.service --since today
```

Pode visualizar em tempo real, semelhante ao conhecido comando "tail":
```
# journalctl -u nginx.service -f
```

Pode também se usar com parâmetros de PID, UID, GID:
```
# journalctl _PID=5000
# journalctl _UID=33
```

Para mostrar LOGs do Kernel:
```
# journaulctl -k
````

Se especializando nos formatos de saída:

Também existe uma opção muito legal de formatos de saída com a opção -o:

Json = Exibe no formato json

```
# journalctl -u nginx -o json
```
{ "__CURSOR" : "s=aa0c895996764a239d5fc0da56f4cc2c;i=705;b=b2a43bc18ce14ae69a059487bf43d41b;m=1096bbab15;t=5555e9425e404;x=2390c67e216bea6", "__REALTIME_TIMESTAMP" : "1501239584351236", "__MONOTONIC_TIMESTAMP" : "

Em um formato melhor:

# journalctl -u nginx -o json-pretty

        "__CURSOR" : "s=aa0c895996764a239d5fc0da56f4cc2c;i=34a;b=b2a43bc18ce14ae69a059487bf43d41b;m=f9a80c;t=5554dfe63e0fb;x=81d483852df82e6",
        "__REALTIME_TIMESTAMP" : "1501168352354555",
        "__MONOTONIC_TIMESTAMP" : "16361484",
        "_BOOT_ID" : "b2a43bc18ce14ae69a059487bf43d41b",

Verbose = Exibe os logs de forma mais detalhada:

```
# journalctl -u nginx -o verbose
```
    MESSAGE=Starting nginx - high performance web server...
    _SOURCE_REALTIME_TIMESTAMP=1501168351763138
Qui 2017-07-27 12:12:32.354555 -03 [s=aa0c895996764a239d5fc0da56f4cc2c;i=34a;b=b2a43bc18ce14ae69a059487bf43d41b;m=f9a80c;t=5554dfe63e0fb;x=81d483852df82e6]
    PRIORITY=6

Cat = Exibe em um formato mais curto:

```
# journalctl -u nginx -o cat
```
Starting nginx - high performance web server...
nginx: [warn] load balancing method redefined in /etc/nginx/conf.d/opac.conf:3
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

Short = Formato padrão de LOGs

```
# journalctl -u nginx -o short
```
Jul 28 08:42:06 nginx.scielo.org systemd[1]: Starting nginx - high performance web server...
Jul 28 08:42:06 nginx.scielo.org nginx[7685]: nginx: [warn] load balancing method redefined in /etc/nginx/conf.d/homolog-opac.conf:3
Jul 28 08:42:06 nginx.scielo.org nginx[7685]: nginx: [warn] load balancing method redefined in /etc/nginx/conf.d/opac.conf:3
Jul 28 08:42:06 nginx.scielo.org nginx[7685]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
Jul 28 08:42:06 nginx.scielo.org nginx[7685]: nginx: configuration file /etc/nginx/nginx.conf test is successful
Jul 28 08:42:06 nginx.scielo.org nginx[7687]: nginx: [warn] load balancing method redefined in /etc/nginx/conf.d/homolog-opac.conf:3
Jul 28 08:42:06 nginx.scielo.org nginx[7687]: nginx: [warn] load balancing method redefined in /etc/nginx/conf.d/opac.conf:3
Jul 28 08:42:06 nginx.scielo.org systemd[1]: Started nginx - high performance web server.

Para exibir os LOGs do Boot corrente:

```
# journalctl -b
```

Conclusão 

O SystemD veio para organizar de uma melhor forma os serviços e LOGs que no meu ponto de vista ficou muito melhor.


Referência:
https://www.digitalocean.com/community/tutorials/how-to-use-journalctl-to-view-and-manipulate-systemd-logs


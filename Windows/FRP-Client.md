# Windows ������ FRP �ͻ��˰�װ���� #

### ע�⿴�ҵı��⣡��������������� WinSW v0.51.3 �汾 amd64��CPU�ܹ� ###

> ��������/ִ��CMD�ű������һὫ���ض˿�ӳ�䵽������Windows Defender ���ܻᱨ���������Զ�ɾ��������͸�����򣬵����޷���͸��
> ������ͼ �ֶ���Ӱ���������������ɸ���ɱ������ٶ�������μ��������

## ���ص�ַ��

### 1���鿴ϵͳCPU�ܹ�

> `Win` �� + `R` �� �����д��� ���� `msinfo32` ���ȷ�� �������´���

![ϵͳժҪ](image/ϵͳժҪ.png)

> �鿴 ��� ��ʾ�� `ϵͳ����` Ȼ������±�ȷ������ϵͳ�ܹ�

|  ���   | ��Ӧ�ܹ�  |
|:-----:|:-----:|
|  x86  | i386  |
|  x64  | amd64 |
| ARM64 | ARM64 |

### 1�������Լ��� `CPU�ܹ�` ���ض�Ӧ�汾�� `FRP`

![FRP�����б�](image/FRP�����б�.png)

> ����֮��ֱ�ӽ�ѹ����Ҫ��װ��Ŀ¼��

[FRP �����б�](https://github.com/fatedier/frp/releases/download/v0.51.3/frp_0.51.3_windows_amd64.zip)

### 2��ɾ�����������ļ�

> ɾ����ͼ ����ڵ� `������ļ�` ���������ļ�

![ɾ��������ļ�](image/ɾ��������ļ�.png)

## �����ļ���

> [FRP �ͻ��� �������������ο�](https://gofrp.org/docs/)

```ini
# [common] ���岿��
[common]
# ����˵�ַ
server_addr = 0.0.0.0
# �����ע��˿� 
server_port = 7000
# ����̨����ʵ��־�ļ�·��
log_file = ./frpc.log
# ��־���� trace, debug, info, warn, error
log_level = info
# ��־����ʱ��
log_max_days = 3
# ָ��ʹ�ú��������֤������ �ͻ��� �� ����� ���������֤��
authentication_method = token
# ���ڷ��͸� ����� �������а��������֤����
authenticate_heartbeats = true
# �Ƿ��ڷ��͵� ����� ���¹��������а��������֤����
authenticate_new_work_conns = true
# token ֵ �ɷ�����ṩ
token = 12345678

# �Զ����������
[ssh]
# tcp | udp | http | https | stcp | xtcp
type = tcp
# ��Ҫ����� ���ص�ַ
local_ip = 127.0.0.1
# ��Ҫ����� ���ض˿�
local_port = 22
# �˴������ƴ�����λΪKB��MB
bandwidth_limit = 1MB
# ���ƴ����λ�ã���ѡ 'client' �ͻ��˻��� 'server' �����
bandwidth_limit_mode = client
# �ͻ���������֮�����Ϣ�Ƿ����
use_encryption = false
# ��Ϣ�Ƿ�ѹ��
use_compression = false
# ����� Զ�̶˿ڼ���
remote_port = 6001
```

## ע��Ϊϵͳ����

### 1��[������� WinSW](https://github.com/winsw/winsw/releases/download/v2.12.0/WinSW-x64.exe)

> ����֮��ֱ�ӷŵ� `FRP �ͻ���` ��װĿ¼�� ���޸�����Ϊ `FRP-Client.exe`

### 2���½� WinSW �����ļ� `FRP-Client.xml` �����������

```xml

<service>
    <!-- ����ID���ƣ�Ψһ�� -->
    <id>FRP-Client</id>
    <!-- ������ʾ���� -->
    <name>FRP-Client</name>
    <!-- �����������Ϣ -->
    <description>FRP-������͸�ͻ��ˣ�����ʵ�ֽ��˼����ӳ�䵽 ***.***.***.*** �ϣ��ṩ��������</description>
    <!-- �����û������� -->
    <env name="HOME" value="%BASE%"/>
    <!-- Ҫִ�еĿ�ִ���ļ� -->
    <executable>%BASE%\frpc.exe</executable>
    <!-- ��ִ���ļ����ݵĲ��� -->
    <arguments>-c .\frpc.ini</arguments>
    <!-- <logmode>rotate</logmode> -->
    <logpath>%BASE%\logs</logpath>
    <log mode="roll-by-size-time">
        <sizeThreshold>10240</sizeThreshold>
        <pattern>yyyyMMdd</pattern>
        <autoRollAtTime>00:00:00</autoRollAtTime>
        <zipOlderThanNumDays>5</zipOlderThanNumDays>
        <zipDateFormat>yyyyMMdd</zipDateFormat>
    </log>
</service>

```

### 3������Ŀ¼Ч������

![ע���Ϊϵͳ����](image/ע���Ϊϵͳ����.png)

### 4���Թ���Ա��ݺڴ���ִ�� `.\FRP-Client.exe install` ע���Ϊϵͳ����

![ע���������](image/ע���������.png)

### 5������Ч��

![����Ч��](image/����Ч��.png)

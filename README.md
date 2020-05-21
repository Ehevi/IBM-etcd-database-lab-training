
## ETCD LAB 

A distributed, reliable key-value store for the most critical data of a distributed system.  
Homepage: https://etcd.io/

Key features:

- Simple: well-defined, user-facing API (gRPC)
- Secure: automatic TLS with optional client cert authentication
- Fast: benchmarked 10,000 writes/sec
- Reliable: properly distributed using Raft


There are two major use cases: concurrency control in the distributed system and application configuration store. For example, CoreOS Container Linux uses etcd to achieve a global semaphore to avoid that all nodes in the cluster rebooting at the same time. Also, Kubernetes use etcd for their configuration store.

During this lab we will be using etcd3 python client.  
Homepage: https://pypi.org/project/etcd3/

Etcd credentials are shared on the slack channel: https://join.slack.com/t/ibm-agh-labs/shared_invite/zt-e8xfjgtd-8IDWmn912qPOflbM1yk6~Q

Please copy & paste them into the cell below:


```python
etcdCreds = {
  "connection": {
    "cli": {
      "arguments": [
        [
          "--cacert=45dc1d70-521a-11e9-8c84-3e25686eb210",
          "--endpoints=https://afc2bd38-f85c-4387-b5fc-f4642c7fcf7b.bc28ac43cf10402584b5f01db462d330.databases.appdomain.cloud:31190",
          "--user=ibm_cloud_f59f3a7b_7578_4cf8_ba20_6df3b352ab46:230064666d4fe6d81f7c53a2c364fb60fa079773e8f9adbc163cb0b2e3c58142"
        ]
      ],
      "bin": "etcdctl",
      "certificate": {
        "certificate_base64": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURIVENDQWdXZ0F3SUJBZ0lVVmlhMWZrWElsTXhGY2lob3lncWg2Yit6N0pNd0RRWUpLb1pJaHZjTkFRRUwKQlFBd0hqRWNNQm9HQTFVRUF3d1RTVUpOSUVOc2IzVmtJRVJoZEdGaVlYTmxjekFlRncweE9ERXdNVEV4TkRRNApOVEZhRncweU9ERXdNRGd4TkRRNE5URmFNQjR4SERBYUJnTlZCQU1NRTBsQ1RTQkRiRzkxWkNCRVlYUmhZbUZ6ClpYTXdnZ0VpTUEwR0NTcUdTSWIzRFFFQkFRVUFBNElCRHdBd2dnRUtBb0lCQVFESkYxMlNjbTJGUmpQb2N1bmYKbmNkUkFMZDhJRlpiWDhpbDM3MDZ4UEV2b3ZpMTRHNGVIRWZuT1JRY2g3VElPR212RWxITVllbUtFT3Z3K0VZUApmOXpqU1IxNFVBOXJYeHVaQmgvZDlRa2pjTkw2YmMvbUNUOXpYbmpzdC9qRzJSbHdmRU1lZnVIQWp1T3c4bkJuCllQeFpiNm1ycVN6T2FtSmpnVVp6c1RMeHRId21yWkxuOGhlZnhITlBrdGFVMUtFZzNQRkJxaWpDMG9uWFpnOGMKanpZVVVXNkpBOWZZcWJBL1YxMkFsT3AvUXhKUVVoZlB5YXozN0FEdGpJRkYybkxVMjBicWdyNWhqTjA4SjZQUwpnUk5hNXc2T1N1RGZiZ2M4V3Z3THZzbDQvM281akFVSHp2OHJMaWF6d2VPYzlTcDBKd3JHdUJuYTFPYm9mbHU5ClM5SS9BZ01CQUFHalV6QlJNQjBHQTFVZERnUVdCQlJGejFFckZFSU1CcmFDNndiQjNNMHpuYm1IMmpBZkJnTlYKSFNNRUdEQVdnQlJGejFFckZFSU1CcmFDNndiQjNNMHpuYm1IMmpBUEJnTlZIUk1CQWY4RUJUQURBUUgvTUEwRwpDU3FHU0liM0RRRUJDd1VBQTRJQkFRQ2t4NVJzbk9PMWg0dFJxRzh3R21ub1EwOHNValpsRXQvc2tmR0pBL2RhClUveEZMMndhNjljTTdNR1VMRitoeXZYSEJScnVOTCtJM1ROSmtVUEFxMnNhakZqWEtCeVdrb0JYYnRyc2ZKckkKQWhjZnlzN29tdjJmb0pHVGxJY0FybnBCL0p1bEZITmM1YXQzVk1rSTlidEh3ZUlYNFE1QmdlVlU5cjdDdlArSgpWRjF0YWxSUVpKandyeVhsWGJvQ0c0MTU2TUtwTDIwMUwyV1dqazBydlBVWnRKcjhmTmd6M24wb0x5MFZ0Zm93Ck1yUFh4THk5TlBqOGlzT3V0ckxEMjlJWTJBMFY0UmxjSXhTMEw3c1ZPeTB6RDZwbXpNTVFNRC81aWZ1SVg2YnEKbEplZzV4akt2TytwbElLTWhPU1F5dTRUME1NeTZmY2t3TVpPK0liR3JDZHIKLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=",
        "name": "45dc1d70-521a-11e9-8c84-3e25686eb210"
      },
      "composed": [
        "ETCDCTL_API=3 etcdctl --cacert=45dc1d70-521a-11e9-8c84-3e25686eb210 --endpoints=https://afc2bd38-f85c-4387-b5fc-f4642c7fcf7b.bc28ac43cf10402584b5f01db462d330.databases.appdomain.cloud:31190 --user=ibm_cloud_f59f3a7b_7578_4cf8_ba20_6df3b352ab46:230064666d4fe6d81f7c53a2c364fb60fa079773e8f9adbc163cb0b2e3c58142"
      ],
      "environment": {
        "ETCDCTL_API": "3"
      },
      "type": "cli"
    },
    "grpc": {
      "authentication": {
        "method": "direct",
        "password": "230064666d4fe6d81f7c53a2c364fb60fa079773e8f9adbc163cb0b2e3c58142",
        "username": "ibm_cloud_f59f3a7b_7578_4cf8_ba20_6df3b352ab46"
      },
      "certificate": {
        "certificate_base64": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURIVENDQWdXZ0F3SUJBZ0lVVmlhMWZrWElsTXhGY2lob3lncWg2Yit6N0pNd0RRWUpLb1pJaHZjTkFRRUwKQlFBd0hqRWNNQm9HQTFVRUF3d1RTVUpOSUVOc2IzVmtJRVJoZEdGaVlYTmxjekFlRncweE9ERXdNVEV4TkRRNApOVEZhRncweU9ERXdNRGd4TkRRNE5URmFNQjR4SERBYUJnTlZCQU1NRTBsQ1RTQkRiRzkxWkNCRVlYUmhZbUZ6ClpYTXdnZ0VpTUEwR0NTcUdTSWIzRFFFQkFRVUFBNElCRHdBd2dnRUtBb0lCQVFESkYxMlNjbTJGUmpQb2N1bmYKbmNkUkFMZDhJRlpiWDhpbDM3MDZ4UEV2b3ZpMTRHNGVIRWZuT1JRY2g3VElPR212RWxITVllbUtFT3Z3K0VZUApmOXpqU1IxNFVBOXJYeHVaQmgvZDlRa2pjTkw2YmMvbUNUOXpYbmpzdC9qRzJSbHdmRU1lZnVIQWp1T3c4bkJuCllQeFpiNm1ycVN6T2FtSmpnVVp6c1RMeHRId21yWkxuOGhlZnhITlBrdGFVMUtFZzNQRkJxaWpDMG9uWFpnOGMKanpZVVVXNkpBOWZZcWJBL1YxMkFsT3AvUXhKUVVoZlB5YXozN0FEdGpJRkYybkxVMjBicWdyNWhqTjA4SjZQUwpnUk5hNXc2T1N1RGZiZ2M4V3Z3THZzbDQvM281akFVSHp2OHJMaWF6d2VPYzlTcDBKd3JHdUJuYTFPYm9mbHU5ClM5SS9BZ01CQUFHalV6QlJNQjBHQTFVZERnUVdCQlJGejFFckZFSU1CcmFDNndiQjNNMHpuYm1IMmpBZkJnTlYKSFNNRUdEQVdnQlJGejFFckZFSU1CcmFDNndiQjNNMHpuYm1IMmpBUEJnTlZIUk1CQWY4RUJUQURBUUgvTUEwRwpDU3FHU0liM0RRRUJDd1VBQTRJQkFRQ2t4NVJzbk9PMWg0dFJxRzh3R21ub1EwOHNValpsRXQvc2tmR0pBL2RhClUveEZMMndhNjljTTdNR1VMRitoeXZYSEJScnVOTCtJM1ROSmtVUEFxMnNhakZqWEtCeVdrb0JYYnRyc2ZKckkKQWhjZnlzN29tdjJmb0pHVGxJY0FybnBCL0p1bEZITmM1YXQzVk1rSTlidEh3ZUlYNFE1QmdlVlU5cjdDdlArSgpWRjF0YWxSUVpKandyeVhsWGJvQ0c0MTU2TUtwTDIwMUwyV1dqazBydlBVWnRKcjhmTmd6M24wb0x5MFZ0Zm93Ck1yUFh4THk5TlBqOGlzT3V0ckxEMjlJWTJBMFY0UmxjSXhTMEw3c1ZPeTB6RDZwbXpNTVFNRC81aWZ1SVg2YnEKbEplZzV4akt2TytwbElLTWhPU1F5dTRUME1NeTZmY2t3TVpPK0liR3JDZHIKLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=",
        "name": "45dc1d70-521a-11e9-8c84-3e25686eb210"
      },
      "composed": [
        "https://ibm_cloud_f59f3a7b_7578_4cf8_ba20_6df3b352ab46:230064666d4fe6d81f7c53a2c364fb60fa079773e8f9adbc163cb0b2e3c58142@afc2bd38-f85c-4387-b5fc-f4642c7fcf7b.bc28ac43cf10402584b5f01db462d330.databases.appdomain.cloud:31190"
      ],
      "hosts": [
        {
          "hostname": "afc2bd38-f85c-4387-b5fc-f4642c7fcf7b.bc28ac43cf10402584b5f01db462d330.databases.appdomain.cloud",
          "port": 31190
        }
      ],
      "path": "",
      "query_options": {},
      "scheme": "https",
      "type": "uri"
    }
  },
  "instance_administration_api": {
    "deployment_id": "crn:v1:bluemix:public:databases-for-etcd:eu-de:a/a34b4e9ea7ab66770e048caf83277971:afc2bd38-f85c-4387-b5fc-f4642c7fcf7b::",
    "instance_id": "crn:v1:bluemix:public:databases-for-etcd:eu-de:a/a34b4e9ea7ab66770e048caf83277971:afc2bd38-f85c-4387-b5fc-f4642c7fcf7b::",
    "root": "https://api.eu-de.databases.cloud.ibm.com/v4/ibm"
  }
}  # copy and paste etcd credentials provided on the Slack channel here
```


```python
!pip install etcd3
```

    Requirement already satisfied: etcd3 in /opt/conda/envs/Python36/lib/python3.6/site-packages (0.12.0)
    Requirement already satisfied: tenacity>=6.1.0 in /opt/conda/envs/Python36/lib/python3.6/site-packages (from etcd3) (6.2.0)
    Requirement already satisfied: six>=1.12.0 in /opt/conda/envs/Python36/lib/python3.6/site-packages (from etcd3) (1.12.0)
    Requirement already satisfied: protobuf>=3.6.1 in /opt/conda/envs/Python36/lib/python3.6/site-packages (from etcd3) (3.6.1)
    Requirement already satisfied: grpcio>=1.27.1 in /opt/conda/envs/Python36/lib/python3.6/site-packages (from etcd3) (1.29.0)
    Requirement already satisfied: setuptools in /opt/conda/envs/Python36/lib/python3.6/site-packages (from protobuf>=3.6.1->etcd3) (40.8.0)


### How to connect to etcd using certyficate (part 1: prepare file with certificate)


```python
import base64
import tempfile

etcdHost = etcdCreds["connection"]["grpc"]["hosts"][0]["hostname"]
etcdPort = etcdCreds["connection"]["grpc"]["hosts"][0]["port"]
etcdUser = etcdCreds["connection"]["grpc"]["authentication"]["username"]
etcdPasswd = etcdCreds["connection"]["grpc"]["authentication"]["password"]
etcdCertBase64 = etcdCreds["connection"]["grpc"]["certificate"]["certificate_base64"]
                           
etcdCertDecoded = base64.b64decode(etcdCertBase64)
etcdCertPath = "{}/{}.cert".format(tempfile.gettempdir(), etcdUser)
                           
with open(etcdCertPath, 'wb') as f:
    f.write(etcdCertDecoded)

print(etcdCertPath)
```

    /home/dsxuser/.tmp/ibm_cloud_f59f3a7b_7578_4cf8_ba20_6df3b352ab46.cert


### Short Lab description

During the lab we will simulate system that keeps track of logged users
- All users will be stored under parent key (path): /logged_users
- Each user will be represented by key value pair
    - key /logged_users/name_of_the_user
    - value hostname of the machine (e.g. name_of_the_user-hostname)

### How to connect to etcd using certyficate (part 2: create client)


```python
import etcd3

etcd = etcd3.client(
    host=etcdHost,
    port=etcdPort,
    user=etcdUser,
    password=etcdPasswd,
    ca_cert=etcdCertPath
)

cfgRoot='/logged_users'
```

### Task 1 : Fetch username and hostname

define two variables
- username name of the logged user (tip: use getpass library)
- hostname hostname of your mcomputer (tip: use socket library)


```python
import getpass
import socket

# username = getpass.getuser()  # You can put your name here, while this code is run in the container and user name would be same for all students
username = 'Agata Cyra'
hostname = socket.gethostname()

userKey='{}/{}'.format(cfgRoot, username)
userKey, '->', hostname
etcd.put(userKey, hostname)
```




    header {
      cluster_id: 17394822126184162018
      member_id: 17586574424884576738
      revision: 361706
      raft_term: 3204
    }



### Task 2 : Register number of users 

etcd3 api: https://python-etcd3.readthedocs.io/en/latest/usage.html

for all names in table fixedUsers register the appropriate key value pairs


```python
fixedUsers = [
    'Adam',
    'Borys',
    'Cezary',
    'Damian',
    'Emil',
    'Filip',
    'Gustaw',
    'Henryk',
    'Ignacy',
    'Jacek',
    'Kamil',
    'Leon',
    'Marek',
    'Norbert',
    'Oskar',
    'Patryk',
    'Rafał',
    'Stefan',
    'Tadeusz'
]

for user in fixedUsers:
    userKey = '{}/{}'.format(cfgRoot, user)
    hostname = socket.gethostname()
    etcd.put(userKey, hostname)

```

### Task 3: List all users

etcd3 api: https://python-etcd3.readthedocs.io/en/latest/usage.html

List all registered user (tip: use common prefix)


```python
for key in etcd.get_prefix(cfgRoot):
    print(key)
```

    (b'/logged_users/Tadeusz - Registered', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-438cad81f4974877a0c254a6d6ff0145-58f95b7948-zfpxd', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'user-10secUser', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Adam-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Borys-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Cezary-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Damian-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Emil-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Filip-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Gustaw-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Henryk-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Ignacy-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Jacek-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Kamil-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Leon-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Marek-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Norbert-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Oskar-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Patryk-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Rafa\xc5\x82-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Stefan-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Tadeusz-notebook-809faba2080d4f76866c72d4e333de78-5d89c844b6-77d6k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-75760867122b4b86a5bbcd2f5ccc321b-66fd799d96-dc62c', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'updated_value', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-a44d382faeed459cb058c7e5432b8e8c-c5c54fd59-bxlrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'testmessage3', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'test_9', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Borys-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Cezary-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Damian-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Emil-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Filip-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Gustaw-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Henryk-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Ignacy-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Jacek-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Kamil-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Leon-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Marek-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Norbert-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Oskar-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Patryk-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Rafa\xc5\x82-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Stefan-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Tadeusz-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'kochana', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-a7823551d9b84fd3b085958036c5f597-d469f8875-ltf7p', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Adamnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Borysnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Cezarynotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Damiannotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Emilnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Filipnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Gustawnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Henryknotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Ignacynotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Jaceknotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Kamilnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Leonnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'new_hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Norbertnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Oskarnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Patryknotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Rafa\xc5\x82notebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Stefannotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Tadeusznotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-condafree1py3690879dede28a40ed88e530f26cab9341-6cpwqdw', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Henryk godlike', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'KamilKoczera-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'KamilKoczera-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Adam_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Borys_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Kamil_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Leon_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Marek_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Norbert_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Oskar_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Patryk_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Rafa\xc5\x82_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Stefan_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Tadeusz_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Cezary_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Damian_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Emil_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Filip_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Gustaw_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Henryk_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Ignacy_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Jacek_transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'test', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Emil_atomic', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Kazimierz', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Adam-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Borys-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Cezary-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Damian-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Emil-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Filip-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Gustaw-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Henryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Ignacy-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Jacek-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Kamil-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Leon-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Marek-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Norbert-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Oskar-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Patryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Rafa\xc5\x82-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Stefan-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Tadeusz-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py3681d3d2d480474a18b8276572c8600ae7-68hmpxx', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Adam-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Borys-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Cezary-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Damian-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Emil-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Filip-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Gustaw-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Henryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Ignacy-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Jacek-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Kamil-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Leon-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Marek-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Norbert-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Oskar-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Patryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Rafa\xc5\x82-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Stefan-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Tadeusz-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Adam-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Borys-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Cezary-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Damian-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Emil-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Filip-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Gustaw-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Henryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Ignacy-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Jacek-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Kamil-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Leon-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Marek-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Norbert-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Oskar-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Patryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Rafa\xc5\x82-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Stefan-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Tadeusz-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Adam-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Borys-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Cezary-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Damian-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Emil-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Filip-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Gustaw-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Henryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Ignacy-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Jacek-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Kamil-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Leon-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Marek-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Norbert-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Oskar-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Patryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Rafa\xc5\x82-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Stefan-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Tadeusz-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'condtion failed', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Adam-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Borys-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Cezary-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Damian-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Emil-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Filip-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Gustaw-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Henryk-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Ignacy-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Jacek-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Kamil-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Leon-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Marek-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Michau-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Norbert-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Oskar-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Patryk-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Rafa\xc5\x82-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Stefan-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Tadeusz-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'value_NewUser_from_another_notebook', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'value first', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'value', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'value_Adam', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'value_Borys', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'value_Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'value_Damian', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'value_Emil', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'value_Filip', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'value_Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'value_Henryk', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'value_Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'value_Jacek', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'value_Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'value_Leon', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'value_Marek', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'value_NewUser_from_another_notebook', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'value_Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'value_Oskar', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'value_Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'value_Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'value_Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'value_Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-3ab835e76fd842e79b5c4a0d4084ffd0-6778f69958-x4rqx', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-b1d4673d48614999b889c4f27a801961-749966946b-stpmd', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'user-3-Adam', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'user-3-Borys', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'user-3-Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'user-3-Damian', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'user-3-Emil', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'user-3-Filip', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'user-3-Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'user-3-Henryk', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'user-3-Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'user-3-Jacek', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'user-3-Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'user-3-Leon', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'user-3-Marek', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'user-3-Norbert', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'user-3-Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'user-3-Patryk', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'user-3-Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'user-3-Stefan', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'user-3-Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'token', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-condafree1py36ca7d2d12048646249d87c4564e316d78-78lfp26', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'normalEmil', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-cb0ffef2f3e240b4b8a72f0a17ed8bba-d57ff64d8-sj7qq', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'newUladzislau', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'val3', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'value second', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Wolski-Notebook', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-premium2py36f501191650b745849cf3df420b3c3644-567d55fwv', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Adam-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Borys-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Cezary-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Damian-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Emil-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Filip-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Gustaw-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Henryk-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Ignacy-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Jacek-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Kamil-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Leon-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Marek-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Norbert-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Oskar-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Patryk-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Rafa\xc5\x82-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Stefan-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Tadeusz-notebook-condafree1py365026a77c8dbf4b5392587b5447eabe87-6bcglbn', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'true', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Adam-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Borys-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Cezary-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Damian-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Emil-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Filip-acwikla-hostname_new', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Gustaw-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Henryk-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Ignacy-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Jacek-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Kamil-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Leon-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Marek-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Norbert-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Oskar-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Patryk-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Rafa\xc5\x82-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Stefan-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Tadeusz-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'temporary-acwikla-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Tadeusz-super-host', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Adam-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Borys-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Cezary-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Damian-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Emil-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Filip-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Gustaw-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Ignacy-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Jacek-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Kamil-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'asdf', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Marek-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Norbert-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Oskar-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Patryk-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Rafa\xc5\x82-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Stefan-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Tadeusz-notebook-condafree1py369fe46cb22f384d5d9bb015906bdbdbbf-6fcgqcp', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'1333', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'value', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'value', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'139', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'{callback_user}-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-0c0cad54e272445a8729f2a377a6def7-546c4b7d5f-ddgbp', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Adam-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Borys-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Cezary-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Damian-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Emil-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Filip-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Gustaw-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Henryk-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Ignacy-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Jacek-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Kamil-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Leon-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Marek-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Norbert-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Oskar-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Patryk-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Rafa\xc5\x82-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Stefan-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Tadeusz-notebook-condafree1py36a83819e79c3e4b399e4058c062729361-566n72k', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py36316d9510130e4ef2aa895ec34c3f8c96-5dpp5sp', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py369aa1e8cdc0ff47c7be259009f850542a-75xqfbc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'DESKTOP-UIJMA2K', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Adam-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Borys-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Cezary-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Damian-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Emil-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Filip-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Gustaw Atomic Replace', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Henryk-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Ignacy-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Jacek-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Kamil-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Leon-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Marek-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Norbert-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Oskar-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Patryk-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Rafa\xc5\x82-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Stefan-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Tadeusz-transaction', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Albert', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'konrad-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Adam-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Borys-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Cezary-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Damian-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'new_value', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Filip-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Gustaw-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Henryk-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Ignacy-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Jacek-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Kamil-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Leon-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Marek-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Norbert-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Oskar-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Patryk-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Rafa\xc5\x82-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Stefan-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Tadeusz-notebook-condafree1py369ae2dd2798eb42529d327e7e89b639a8-672lvkb', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'watch', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-0b34268899e14795b60b6d36d0883ae1-576b4b5d8d-w62xc', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'value', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'value was there', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Adam-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Borys-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Cezary-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Damian-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Emil-hostname123123', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Filip-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Gustaw-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Henryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Ignacy-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Jacek-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Kamil-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Leon-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Marek-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Norbert-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Oskar-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Patryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Rafa\xc5\x82-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Stefan-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Tadeusz-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'notebook-condafree1py36df71c07b4e3b4ebe8de418e54ed7b1b8-cbcxtkz', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'22', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-conda2py36df71c07b4e3b4ebe8de418e54ed7b1b8-8487f9626v8', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'lock_test', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py365bb341db1c18409987fd62af4cdf6777-68sw9z2', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'notebook-438cad81f4974877a0c254a6d6ff0145-75bbbcdfb9-nhh88', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Adam-hostname2', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Borys-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Cezary-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Damian-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Emil-hostname2', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Filip-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Gustaw-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Henryk-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Ignacy-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Jacek-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-condafree1py3690879dede28a40ed88e530f26cab9341-6cpwqdw', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Kamil-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Leon-hostname2', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Marek-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Norbert-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Oskar-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Patryk-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Rafa\xc5\x82-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Stefan-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Tadeusz-hostname2', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'{callback}-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'{callback}-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'notebook-db4dad02cab04ea3be358367155cc493-5cb65bd564-dgqgr', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Adam-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Borys-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Cezary-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Damian-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Dr. Manhattan', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Dr. Manhattan', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Emil-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Filip-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Gustaf-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Henryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Ignacy-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Jacek-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Kamil-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Leon-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Marek-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Norbert-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'Oskar-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Patryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Rafa\xc5\x82-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Stefan-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Tadeusz-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab3c8>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'updated-Adam', <etcd3.client.KVMetadata object at 0x7f7581d8da58>)
    (b'updated-Borys', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'updated-Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'updated-Damian', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'updated-Emil', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'updated-Filip', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'updated-Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'updated-Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'updated-Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'updated-Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'updated-Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'updated-Leon', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'updated-Marek', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'updated-Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'updated-Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'updated-Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'updated-Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'updated-Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'updated-Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Tadeusz-notebook-condafree1py362847d525672542009895af0d8d60e2f2-79xg5sk', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'notebook-condafree1py362847d525672542009895af0d8d60e2f2-86rblb8', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Success', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Adam-host', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Borys-host', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Cezary-host', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Damian-host', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Emil-host', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Filip-host', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Gustaw-host', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Henryk-host', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Ignacy-host', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Jacek-host', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Kamil-host', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Leon-host', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Marek-host', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Norbert-host', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Oskar-host', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Patryk-host', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Rafa\xc5\x82-host', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Stefan-host', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'Tadeusz-host', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'Tadeusz-host', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'{task10}', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'TempUser3-hostname', <etcd3.client.KVMetadata object at 0x7f7581dab0b8>)
    (b'TempUser5-hostname3', <etcd3.client.KVMetadata object at 0x7f7581dab358>)
    (b'for_watcher', <etcd3.client.KVMetadata object at 0x7f7581dab208>)
    (b'checkLease', <etcd3.client.KVMetadata object at 0x7f7581dab080>)
    (b'someValue 2', <etcd3.client.KVMetadata object at 0x7f7581dab208>)


### Task 4 : Same as Task2, but use transaction

etcd3 api: https://python-etcd3.readthedocs.io/en/latest/usage.html

for all names in table fixedUsers register the appropriate key value pairs, use transaction to make it a single request  
(Have you noticed any difference in execution time?)


```python
etcd.transaction(
    compare = [etcd.transactions.version(cfgRoot) > 0],
    success = [etcd.transactions.put('{}/{}'.format(cfgRoot, user), 'user-{}'.format(user)) for user in fixedUsers],
    failure = [etcd.transactions.put('/tmp/failure', 'condition failed')]
)
```




    (True, [response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }, response_put {
        header {
          revision: 361726
        }
      }])



### Task 5 : Get single key (e.g. status of transaction)

etcd3 api: https://python-etcd3.readthedocs.io/en/latest/usage.html

Check the key you are modifying in on-failure handler in previous task


```python
etcd.transaction(
        compare=[etcd.transactions.version(cfgRoot) == 0],
        success=[etcd.transactions.put('{}/{}'.format(cfgRoot, user), 'user-{}'.format(user)) for user in fixedUsers],
        failure=[etcd.transactions.put('/tmp/failure', 'condtion failed - users modification')]
    )

for key in etcd.get_prefix('/tmp/failure'):
    print(key)
```

    (b'condtion failed - users modification', <etcd3.client.KVMetadata object at 0x7f7581dace48>)


### Task 6 : Get range of Keys (Emil -> Oskar) 

etcd3 api: https://python-etcd3.readthedocs.io/en/latest/usage.html

- Get range of keys
- Is it inclusive / exclusive?
- Sort the resposne descending
- Sort the resposne descending by value not by key


```python
r_from = '{}/{}'.format(cfgRoot, 'Emil')
r_to = '{}/{}'.format(cfgRoot, 'Oskar')
for key in etcd.get_range(r_from, r_to, sort_order='descend', sort_target='value'):
    print(key)
```

    (b'value_NewUser_from_another_notebook', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'value first', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'user-Norbert', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'user-Marek', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'user-Leon', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'user-Kamil', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'user-Jacek', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'user-Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'user-Henryk', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'user-Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'user-Filip', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'user-Emil', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'testmessage3', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'test_9', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'test', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'notebook-condafree1py3690879dede28a40ed88e530f26cab9341-6cpwqdw', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'notebook-condafree1py3681d3d2d480474a18b8276572c8600ae7-68hmpxx', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'notebook-a7823551d9b84fd3b085958036c5f597-d469f8875-ltf7p', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'new_hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'kochana', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'condtion failed', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dacc88>)
    (b'condition failed', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Tadeusznotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Tadeusz_transaction', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Tadeusz-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Tadeusz-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Tadeusz-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Tadeusz-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacc88>)
    (b'Tadeusz-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Tadeusz-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacc88>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Tadeusz', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Stefannotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Stefan_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacc88>)
    (b'Stefan-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Stefan-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Stefan-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Stefan-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Stefan-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Stefan-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Stefan', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Rafa\xc5\x82notebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Rafa\xc5\x82_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacc88>)
    (b'Rafa\xc5\x82-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Rafa\xc5\x82-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Rafa\xc5\x82-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Rafa\xc5\x82-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Rafa\xc5\x82-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Rafa\xc5\x82-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Rafa\xc5\x82', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Patryknotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Patryk_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacc88>)
    (b'Patryk-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Patryk-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Patryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Patryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Patryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Patryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Patryk', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Oskarnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Oskar_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacc88>)
    (b'Oskar-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Oskar-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Oskar-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Oskar-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Oskar-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Oskar-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Oskar', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Norbertnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Norbert_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacc88>)
    (b'Norbert-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Norbert-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Norbert-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacc88>)
    (b'Norbert-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Norbert-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacc88>)
    (b'Norbert-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Norbert', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Michau-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Marek_transaction', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Marek-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Marek-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Marek-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Marek-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Marek-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Marek-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Marek', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Leonnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Leon_transaction', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Leon-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Leon-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Leon-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Leon-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Leon-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Leon-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Leon', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Kazimierz', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Kamilnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Kamil_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'KamilKoczera-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacc88>)
    (b'KamilKoczera-hostname', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Kamil-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Kamil-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Kamil-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Kamil-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Kamil-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Kamil-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Kamil', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Jaceknotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Jacek_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Jacek-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Jacek-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Jacek-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Jacek-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Jacek-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Jacek-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Jacek', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Ignacynotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Ignacy_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Ignacy-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Ignacy-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Ignacy-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Ignacy-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Ignacy-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Ignacy-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Ignacy', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Henryknotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Henryk_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Henryk-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Henryk-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Henryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Henryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Henryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Henryk-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Henryk godlike', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Henryk', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Gustawnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Gustaw_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Gustaw-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Gustaw-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Gustaw-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Gustaw-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Gustaw-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Gustaw-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Gustaw', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Filipnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Filip_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Filip-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Filip-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Filip-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Filip-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Filip-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Filip-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Filip', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Emilnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Emil_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Emil_atomic', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Emil-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Emil-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Emil-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Emil-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Emil-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Emil-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Emil', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Damiannotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Damian_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Damian-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Damian-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Damian-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Damian-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Damian-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Damian-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Damian', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Cezarynotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Cezary_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Cezary-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Cezary-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Cezary-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Cezary-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Cezary-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Cezary-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Cezary', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Borysnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581dacc50>)
    (b'Borys_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Borys-notebook-condafree1py3621c9aec184684c4586519a69becb7bf2-785r4k8', <etcd3.client.KVMetadata object at 0x7f7581dacc50>)
    (b'Borys-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Borys-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Borys-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Borys-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Borys-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dacc50>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Borys', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Adamnotebook-66c43e4471114fa295d284b8afdefbb0-58bc66c8d6-9bxrd', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Adam_transaction', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Adam-notebook-3dd95954209d46018d78a714da459b6a-545894974-srlx9', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Adam-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Adam-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Adam-hostname', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Adam-hostname', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dace48>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dacb38>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581daccc0>)
    (b'Adam', <etcd3.client.KVMetadata object at 0x7f7581dace48>)


### Task 7: Atomic Replace

etcd3 api: https://python-etcd3.readthedocs.io/en/latest/usage.html

Do it a few times, check if value has been replaced depending on condition


```python
etcd.transaction(
        compare=[],
        success=[etcd.transactions.put('{}/{}'.format(cfgRoot, user), 'user-2-{}'.format(user)) for user in fixedUsers],
        failure=[]
    )

print('First time:')
for user in fixedUsers:
    result = etcd.replace('{}/{}'.format(cfgRoot, user), 'user-2-{}'.format(user), 'user-3-{}'.format(user))
    print(f'{user}: {result}')
    
print('------------------------')
    
print('Second time:')
for user in fixedUsers:
    result = etcd.replace('{}/{}'.format(cfgRoot, user), 'user-2-{}'.format(user), 'user-3-{}'.format(user))
    print(f'{user}: {result}')
```

    First time:
    Adam: True
    Borys: True
    Cezary: True
    Damian: True
    Emil: True
    Filip: True
    Gustaw: True
    Henryk: True
    Ignacy: True
    Jacek: True
    Kamil: True
    Leon: True
    Marek: True
    Norbert: True
    Oskar: True
    Patryk: True
    Rafał: True
    Stefan: True
    Tadeusz: True
    ------------------------
    Second time:
    Adam: False
    Borys: False
    Cezary: False
    Damian: False
    Emil: False
    Filip: False
    Gustaw: False
    Henryk: False
    Ignacy: False
    Jacek: False
    Kamil: False
    Leon: False
    Marek: False
    Norbert: False
    Oskar: False
    Patryk: False
    Rafał: False
    Stefan: False
    Tadeusz: False


### Task 8 : Create lease - use it to create expiring key

etcd3 api: https://python-etcd3.readthedocs.io/en/latest/usage.html

You can create a key that will be for limited time
add user that will expire after a few seconds

Tip: Use lease



```python
import time

lease = etcd.lease(ttl=10)

userKey='{}/{}'.format(cfgRoot, '10secUser')
value='This user will disappear after 10 seconds!'

etcd.put(userKey, value, lease=lease)

print(etcd.get(userKey))

time.sleep(10)

print(etcd.get(userKey))
```

    (b'This user will disappear after 10 seconds!', <etcd3.client.KVMetadata object at 0x7f7581dacb70>)
    (None, None)


### Task 9 : Create key that will expire after you close the connection to etcd

Tip: use threading library to refresh your lease


```python
import threading

lease = etcd.lease(ttl=10)
WAIT_SECONDS = 1

def refreshLease():
    lease.refresh()
    threading.Timer(WAIT_SECONDS, refreshLease).start()
    
refreshLease()
```

### Task 10: Use lock to protect section of code

etcd3 api: https://python-etcd3.readthedocs.io/en/latest/usage.html


```python
import time

with etcd.lock('lock-2', ttl=20) as lock:
    # do something that requires the lock
    print(f'Is acquired? {lock.is_acquired()}')
    lock.acquire()
    print('Lock acquired')
    print(f'Is acquired? {lock.is_acquired()}')
    time.sleep(3)
    lock.release()
    print('Lock released')
    print(f'Is acquired? {lock.is_acquired()}')
```

    Is acquired? True
    Lock acquired
    Is acquired? True
    Lock released
    Is acquired? False


### Task 11: Watch key

etcd3 api: https://python-etcd3.readthedocs.io/en/latest/usage.html

This cell will lock this notebook on waiting  
After running it create a new notebook and try to add new user


```python
events_iterator, cancel = etcd.watch_prefix(cfgRoot)
for event in events_iterator:
    print('Event : {}'.format(event), vars(event))
```

    Event : <class 'etcd3.events.PutEvent'> key=b'/logged_users/Agata Cyra' value=b'notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp' {'key': b'/logged_users/Agata Cyra', '_event': kv {
      key: "/logged_users/Agata Cyra"
      create_revision: 357529
      mod_revision: 361752
      version: 26
      value: "notebook-b817ce8c6ae5412a9750676d8ba98fd6-7ff6d8f958-5dpkp"
    }
    }



```python

```


```python

```

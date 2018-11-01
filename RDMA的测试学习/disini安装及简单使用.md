## 1��DISINI��װ

1. git clone https://github.com/zrlio/disni

2. cd disni

3. sudo apt install maven

4. mvn -DskipTests install

5. cd libdisni

6. ./autoprepare.sh

7. configure --prefix=/usr/local/lib/libdisni/lib �Cwith-jdk=/opt/java/jdk8(������java��װ��λ�ã�����prefix��ָ��DISNI�İ�װλ��)

8. make install

��װʱ���˴��󣬴�ž����и������Ҳ��������������

��autoprepare.sh�ű��ı���ʾ�����Ӹ����ĳɵ����ͺ��ˡ�

��װ��֮�������`ibv_devinfo`�鿴��Ϣ��

## 2��ʹ��

1����target�ļ����µ�������disni��ִ��ǰ��Ҫִ�������������

***export LD_LIBRARY_PATH=/usr/local/lib/libdisni/lib***

LD_LIBRARY_PATH��Linux��������������Ҫ����ָ�����ҹ���⣨��̬���ӿ⣩ʱ����Ĭ��·��֮�������·����������������һ���Եģ���/etc/profileĩβ��������õģ�

��������û�����������ôҲ���������г����ʱ��ͨ��ָ��

***-Djava.library.path=/usr/local/lib/libdisni/lib***



2����node24��node25��װ��RDMA�ˣ���echo $LD_LIBRARY_PATH�鿴RDMA��װλ�ã���װ·����/usr/local/lib/libdisni/lib��

��node25���з���ˣ�ipΪ10.0.0.25��
***java -cp disni-1.6-jar-with-dependencies.jar:disni-1.6-tests.jar com.ibm.disni.examples.SendRecvServer -a 10.10.0.25***
��node25���пͻ��ˣ����ӷ�����ip��
***java -cp disni-1.6-jar-with-dependencies.jar:disni-1.6-tests.jar com.ibm.disni.examples.SendRecvClient -a 10.10.0.25***

���н����

![�ͻ���](assets/�ͻ���.png)

![�����](assets/�����.png)

�������Ĺ�����client������Ϣ��server����server�յ���Ϣ֮��server����һ����Ϣ��client���������ͼƬ���ǿ��Կ������еĹ��̡�

## 3����IDEA�ϱ�д������disni����

### 3.1 DISNI���ģ�ͽ���

DiSNI API ��һ��Group/Endpoint ģ�ͣ�����Ҫ�������ؼ��Ľӿڣ�

- DiSNIServerEndpoint: 
  - ��ʾ���Ǽ����ķ��������ȴ��ͻ�������
  - ����bind()���ڰ󶨶˿ڣ�accept()�������ڼ�������
- DiSNIEndpoint: 
  - ��ʾ����һ��Զ�̣��򱾵أ���Դ����RDMA��������
  - �ṩ�������ķ�������ȡ����д��Դ��read()��write()
- DiSNIGroup: 
  - client��server endpoint�������͹���



### 3.2 IDEA��ʹ��

��IDEA����MAVEN��Ŀ����pom�ļ��µ���������룺

```java
<dependency>
  <groupId>com.ibm.disni</groupId>
  <artifactId>disni</artifactId>
  <version>1.6</version>
</dependency>
```

�������������jar����


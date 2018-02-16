
This is used to test 2 ways SSL by adding the root CA certification into server side (trust store)and add sub CA certification in to client side(key store).
To make sure if client can access server without adding the sub CA certification into server


#### Try
In this way, anyone can access http://localhost:8080/door?uri=https://localhost:8443/user

#### ��֤�������
//all password is changeit
1. ���ɸ�֤��
<pre><code>
mkdir private 
openssl genrsa -aes256 -out private/rootca.key.pem 2048

2�����ɸ�֤��ǩ�����루ca.csr��
<pre><code>
openssl req -new -key private/rootca.key.pem -out private/rootca.csr -subj "/C=CN/ST=LiaoNing/L=DL/O=company/OU=center/CN=*.brotherhui.com"
req          ����֤��ǩ����������
-new         ��ʾ������
-key         ��Կ,����Ϊprivate/rootca.key.pem�ļ�
-out         ���·��,����Ϊprivate/rootca.csr�ļ�
-subj        ָ���û���Ϣ������ʹ�÷�����"*.brotherhui.com"
�õ���֤��ǩ�������ļ������ǿ��Խ��䷢����CA����ǩ������Ȼ����Ҳ��������ǩ����֤�顣

3��ǩ����֤�飨����ǩ����֤��, ��ǩ֤�飩
<pre><code>
mkdir certs
openssl x509 -req -days 10000 -sha1 -extensions v3_ca -signkey private/rootca.key.pem -in private/rootca.csr -out certs/rootca.cer -CAcreateserial
x509        ǩ��X.509��ʽ֤�����
-req        ��ʾ֤����������
-days       ��ʾ��Ч����,����Ϊ10000�졣
-shal       ��ʾ֤��ժҪ�㷨,����ΪSHA1�㷨��
-extensions ��ʾ��OpenSSL�����ļ�v3_ca�������չ��
-signkey    ��ʾ��ǩ����Կ,����Ϊprivate/rootca.key.pem��
-in         ��ʾ�����ļ�,����Ϊprivate/rootca.csr��
-out        ��ʾ����ļ�,����Ϊcerts/rootca.cer��




#### �м�֤�������
1. �����м�֤��
openssl genrsa -aes256 -out private/intermediate.key.pem 2048

2�������м�֤��ǩ�����루intermediate.csr��
openssl req -new -key private/intermediate.key.pem -out private/intermediate.csr -subj "/C=CN/ST=LiaoNing/L=DL/O=company/OU=center/CN=intermediate.brotherhui.com"

req          ����֤��ǩ����������
-new         ��ʾ������
-key         ��Կ,����Ϊprivate/intermediate.key.pem�ļ�
-out         ���·��,����Ϊprivate/intermediate.csr�ļ�
-subj        ָ���û���Ϣ������ʹ���м�����"intermediate.brotherhui.com"
�õ���֤��ǩ�������ļ������ǿ��Խ��䷢����CA����ǩ������Ȼ����Ҳ��������ǩ����֤�顣

3��ǩ���м�֤�飨��֤��ǩ����, �൱�ڹ�Կ
openssl x509 -req -days 10000 -sha1 -extensions v3_ca -in private/intermediate.csr -out certs/intermediate.cer -CA certs/rootca.cer -CAkey private/rootca.key.pem  -CAcreateserial 
x509        ǩ��X.509��ʽ֤�����
-req        ��ʾ֤����������
-days       ��ʾ��Ч����,����Ϊ10000�졣
-shal       ��ʾ֤��ժҪ�㷨,����ΪSHA1�㷨��
-extensions ��ʾ��OpenSSL�����ļ�v3_ca�������չ��
-signkey    ��ʾ��ǩ����Կ,����Ϊprivate/rootca.key.pem��
-in         ��ʾ�����ļ�,����Ϊprivate/intermediate.csr��
-out        ��ʾ����ļ�,����Ϊcerts/intermediate.cer��
-CA         ��ʾǩ���õ�֤��
-CAkey      ��ʾǩ���õ�֤���key



#### ����֤�������
1. ��������֤��
openssl genrsa -aes256 -out private/thirdlevel.key.pem 2048

2����������֤��ǩ�����루thirdlevel.csr��
openssl req -new -key private/thirdlevel.key.pem -out private/thirdlevel.csr -subj "/C=CN/ST=LiaoNing/L=DL/O=company/OU=center/CN=thirdlevel.intermediate.brotherhui.com"
req          ����֤��ǩ����������
-new         ��ʾ������
-key         ��Կ,����Ϊprivate/thirdlevel.key.pem�ļ�
-out         ���·��,����Ϊprivate/thirdlevel.csr�ļ�
-subj        ָ���û���Ϣ������ʹ���м�����"thirdlevel.intermediate.brotherhui.com"
�õ���֤��ǩ�������ļ������ǿ��Խ��䷢����CA����ǩ������Ȼ����Ҳ��������ǩ����֤�顣

3��ǩ������֤�飨�м�֤��ǩ����, �൱�ڹ�Կ
openssl x509 -req -days 10000 -sha1 -extensions v3_ca -in private/thirdlevel.csr -out certs/thirdlevel.cer -CA certs/intermediate.cer -CAkey private/intermediate.key.pem  -CAcreateserial 
x509        ǩ��X.509��ʽ֤�����
-req        ��ʾ֤����������
-days       ��ʾ��Ч����,����Ϊ10000�졣
-shal       ��ʾ֤��ժҪ�㷨,����ΪSHA1�㷨��
-extensions ��ʾ��OpenSSL�����ļ�v3_ca�������չ��
-in         ��ʾ�����ļ�,����Ϊprivate/thirdlevel.csr��
-out        ��ʾ����ļ�,����Ϊcerts/thirdlevel.cer��



#### keystore, truststore����
Ŀ�꣬ ʵ��2 way ssl (mutual auth), ��Ҫ��rootca��intermediate��֤�����ŵ��������˵�truststore, ������thirdlevel��֤�����Ӷ��ﵽ�ͻ��˿���2 wayssl��Ŀ��

1. Server-keystore ���Ҫ�Ը�֤���ṩ���� ����ת��Ϊkeystore��ת��ΪPKCS#12�����ʽ������������������
mkdir keystore
openssl pkcs12 -export -inkey private/rootca.key.pem -in certs/rootca.cer -out keystore/server-keystore.p12
pkcs12          PKCS#12�����ʽ֤�����
-export         ��ʾ����֤�顣
-cacerts        ��ʾ������CA֤�顣
-inkey          ��ʾ������Կ,����Ϊprivate/rootca.key.pem
-in             ��ʾ�����ļ�,����Ϊcerts/rootca.cer
-out            ��ʾ����ļ�,����Ϊcerts/rootca.p12
������Ϣ�����ļ���PKCS#12�� ������Ϊ��Կ������ο�ʹ�ã����ǿ���ͨ��KeyTool�鿴��Կ�����ϸ��Ϣ��


2���鿴��Կ����Ϣ
keytool -list -keystore keystore/server-keystore.p12 -storetype pkcs12 -v -storepass changeit
ע�⣬�������-storetypeֵΪ��pkcs12����
�����Ѿ������˸�֤�飨rootca.cer��,���ǿ���ʹ�ø�֤��ǩ��������֤��Ϳͻ�֤�顣
>
Keystore type: PKCS12
Keystore provider: SunJSSE
Your keystore contains 1 entry
Alias name: 1
Creation date: Feb 16, 2018
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=*.brotherhui.com, OU=center, O=company, L=DL, ST=LiaoNing, C=CN
Issuer: CN=*.brotherhui.com, OU=center, O=company, L=DL, ST=LiaoNing, C=CN
Serial number: a018f913d62f450d
Valid from: Fri Feb 16 10:21:25 CST 2018 until: Tue Jul 04 10:21:25 CST 2045
Certificate fingerprints:
         MD5:  AD:31:BA:A1:E5:B3:F9:41:32:4C:97:6C:D7:38:07:B0
         SHA1: AB:17:28:03:80:63:96:E7:0B:B0:56:23:E9:2B:78:7B:D4:EE:33:28
         SHA256: A1:06:2B:C6:0E:AB:EA:9D:3D:26:D8:B5:FA:71:41:F1:89:B3:06:2D:82:8A:C0:2F:CE:2B:C2:22:30:D8:99:45
         Signature algorithm name: SHA1withRSA
         Version: 1
*******************************************
*******************************************
>


3. Client-TrustStore ���Ҫ�Ը�֤���ṩ����˷��� �����client��˫��ssl���ʵĻ��� ��Ҫ�Ѹ�֤��Ĺ�Կ�ŵ�client�˵�client truststore
keytool -importcert -alias 1 -file certs/rootca.cer -keypass changeit -keystore keystore/client-truststore.jks -storepass changeit -noprompt


4. Client-Keystore ֻ����thirdlevel��clientkeystore
openssl pkcs12 -export  -aes256  -inkey private/intermediate.key.pem -in certs/intermediate.cer -out keystore/intermediate-keystore.p12
keytool -list -keystore keystore/intermediate-keystore.p12 -storetype pkcs12 -v -storepass changeit

>
Keystore type: PKCS12
Keystore provider: SunJSSE
Your keystore contains 1 entry
Alias name: 1
Creation date: Feb 16, 2018
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=intermediate.brotherhui.com, OU=center, O=company, L=DL, ST=LiaoNing, C=CN
Issuer: CN=*.brotherhui.com, OU=center, O=company, L=DL, ST=LiaoNing, C=CN
Serial number: a13aee4a9cd9e4cf
Valid from: Fri Feb 16 13:22:25 CST 2018 until: Tue Jul 04 13:22:25 CST 2045
Certificate fingerprints:
         MD5:  AD:F6:0E:47:7B:41:93:3D:9D:89:CD:52:4B:15:B9:82
         SHA1: A2:99:A9:F3:99:41:AA:A8:6D:2E:E0:AC:91:31:E4:4A:FA:48:9C:3C
         SHA256: BF:26:7E:7D:DC:BE:C4:C9:F1:BB:7E:77:29:A1:CC:FA:61:B5:96:4B:55:EF:45:81:41:73:AB:78:21:F0:A3:BA
         Signature algorithm name: SHA1withRSA
         Version: 1
*******************************************
>

openssl pkcs12 -export  -aes256  -inkey private/thirdlevel.key.pem -in certs/thirdlevel.cer -out keystore/client-keystore.p12
keytool -list -keystore keystore/client-keystore.p12 -storetype pkcs12 -v -storepass changeit
>
Keystore type: PKCS12
Keystore provider: SunJSSE
Your keystore contains 1 entry
Alias name: 1
Creation date: Feb 16, 2018
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=thirdlevel.intermediate.brotherhui.com, OU=center, O=company, L=DL, ST=LiaoNing, C=CN
Issuer: CN=intermediate.brotherhui.com, OU=center, O=company, L=DL, ST=LiaoNing, C=CN
Serial number: 8cdc5d74877a8bfe
Valid from: Fri Feb 16 13:22:58 CST 2018 until: Tue Jul 04 13:22:58 CST 2045
Certificate fingerprints:
         MD5:  DD:51:AA:A2:4B:3D:01:82:90:D5:95:4E:E8:F7:F3:E3
         SHA1: 2D:49:26:8C:3B:BC:4C:A3:E7:C8:0B:AA:4C:1F:D0:B8:BA:55:C7:01
         SHA256: C0:43:0D:AF:A7:FF:AB:B2:58:4B:66:9B:A7:D7:7F:3E:14:F5:B6:7E:DA:9C:DA:E2:CF:87:AF:B5:43:84:BC:97
         Signature algorithm name: SHA1withRSA
         Version: 1
*******************************************
>

5. Server-TrustStore ֻ����rootca��intermediate��֤��
keytool -importcert -alias rootca -file certs/rootca.cer -keypass changeit -keystore keystore/server-truststore.jks -storepass changeit -noprompt
keytool -importcert -alias intermediate -file certs/intermediate.cer -keypass changeit -keystore keystore/server-truststore.jks -storepass changeit -noprompt

keytool -list -keystore keystore/server-truststore.jks -storetype jks -v -storepass changeit

>
Keystore type: JKS
Keystore provider: SUN
Your keystore contains 2 entries
Alias name: rootca
Creation date: Feb 16, 2018
Entry type: trustedCertEntry
Owner: CN=*.brotherhui.com, OU=center, O=company, L=DL, ST=LiaoNing, C=CN
Issuer: CN=*.brotherhui.com, OU=center, O=company, L=DL, ST=LiaoNing, C=CN
Serial number: 89d1ae8240361070
Valid from: Fri Feb 16 13:21:51 CST 2018 until: Tue Jul 04 13:21:51 CST 2045
Certificate fingerprints:
         MD5:  D4:DD:BD:FB:B0:18:5C:5E:6B:53:19:E2:1B:0F:C3:F3
         SHA1: B3:8B:42:ED:FB:BA:B9:3E:E7:1A:88:75:55:88:90:D0:BF:E4:87:8A
         SHA256: 64:89:EB:87:51:E0:5F:8D:F8:51:93:F4:58:E5:EF:01:8A:1C:2F:44:6A:4B:65:4C:CD:EA:F8:CB:05:8E:6C:44
         Signature algorithm name: SHA1withRSA
         Version: 1
*******************************************
*******************************************
Alias name: intermediate
Creation date: Feb 16, 2018
Entry type: trustedCertEntry
Owner: CN=intermediate.brotherhui.com, OU=center, O=company, L=DL, ST=LiaoNing, C=CN
Issuer: CN=*.brotherhui.com, OU=center, O=company, L=DL, ST=LiaoNing, C=CN
Serial number: a13aee4a9cd9e4cf
Valid from: Fri Feb 16 13:22:25 CST 2018 until: Tue Jul 04 13:22:25 CST 2045
Certificate fingerprints:
         MD5:  AD:F6:0E:47:7B:41:93:3D:9D:89:CD:52:4B:15:B9:82
         SHA1: A2:99:A9:F3:99:41:AA:A8:6D:2E:E0:AC:91:31:E4:4A:FA:48:9C:3C
         SHA256: BF:26:7E:7D:DC:BE:C4:C9:F1:BB:7E:77:29:A1:CC:FA:61:B5:96:4B:55:EF:45:81:41:73:AB:78:21:F0:A3:BA
         Signature algorithm name: SHA1withRSA
         Version: 1
*******************************************
*******************************************
>

#### Conclusion

3 level 2ways ssl is working fine with this 

Type | Client | Server
Keystore | thirdlevel | rootca
truststore | rootca pubkey | rootca pubkey and intermediate pubkey 


Java certificates are stored in `%JAVA_HOME%\lib\security\cacerts`. Default password for it is `changeit`.

**Init keystore**
`"%JAVA_HOME%\bin\keytool.exe" -genkey -alias tomcat -keyalg RSA -keysize 2048 -validity 999 -keystore D:\keystore.jks`

**Add root cert**
`"%JAVA_HOME%\bin\keytool.exe" -import -alias root -keystore D:\truststore.jks -trustcacerts -file "D:\Root_CA.crt"`

**Add intermediate cert**
`"%JAVA_HOME%\bin\keytool.exe" -import -alias intermediate -keystore D:\truststore.jks -trustcacerts -file "D:\Issuing_CA.crt"`

**Generate client key**
`"%JAVA_HOME%\bin\keytool.exe" -import -alias tomcat -keystore D:\keystore.jks -file "D:\Issuing_CA.crt"`

**List all certs**
`"%JAVA_HOME%\bin\keytool.exe" -list -keystore "D:\keystore.jks" -storepass <password>`

**Export client key**
`"%JAVA_HOME%\bin\keytool.exe" -export -alias tomcat -keyalg RSA -keystore D:\keystore.jks -file D:\clientcert`

**Remove cert**
`"%JAVA_HOME%\bin\keytool.exe" -delete -alias tomcat -keystore D:\keystore.jks -storepass <password>`

## Add certificates to the Tomcat

**Create the key & cert for the Tomcat server**
`"%JAVA_HOME%\bin\keytool.exe" -genkey -v -alias tomcat -keyalg RSA -sigalg SHA256withRSA -validity 999 -keystore D:\tomcat\keystore.jks -storepass <password> -keypass <password> -dname "CN=localhost, OU=<OU>, O=<O>, L=SPb, ST=SPb, C=RU"`

**Create the key & cert for client**
`"%JAVA_HOME%\bin\keytool.exe" -genkey -v -alias tomcatKey -keyalg RSA -storetype PKCS12 -keystore D:\tomcat\tomcatKey.p12 -storepass <password> -keypass <password> -dname "CN=localhost, OU=<OU>, O=<O>, L=SPb, ST=SPb, C=RU"
`
**Add the certificate (containing the public key) to the Tomcat keystore**
`"%JAVA_HOME%\bin\keytool.exe" -export -alias tomcatKey -keystore D:\tomcat\tomcatKey.p12 -storetype PKCS12 -storepass <password> -rfc -file D:\tomcat\tomcatKey.cer`

`"%JAVA_HOME%\bin\keytool.exe" -import -v -file D:\tomcat\tomcatKey.cer -keystore D:\tomcat\keystore.jks -storepass <password>`

**Import the Certificate Authorities (CA) file to keystore**
`"%JAVA_HOME%\bin\keytool.exe" -importcert -file "D:\Root_CA.crt" -keystore D:\tomcat\keystore.jks -alias root

`"%JAVA_HOME%\bin\keytool.exe" -importcert -file "D:\Issuing_CA.crt" -keystore D:\tomcat\keystore.jks -alias serverCrt`

**Tomcat connections settings**
```xml
<Connector
	protocol="org.apache.coyote.http11.Http11NioProtocol"
	port="8443" maxThreads="200" scheme="https" secure="true" SSLEnabled="true"
	keystoreFile="/home/server-keystore.jks" keystorePass="changeit"
	truststoreFile="/home/server-truststore.jks" truststorePass="changeit"
	clientAuth="false" sslProtocol="TLS"/>
<Connector
	protocol="org.apache.coyote.http11.Http11NioProtocol"
	port="8143" maxThreads="200" scheme="https" secure="true" SSLEnabled="true"
	keystoreFile="/home/server-keystore.jks" keystorePass="changeit"
	truststoreFile="/home/server-truststore.jks" truststorePass="changeit"
	clientAuth="true" sslProtocol="TLS"
	SSLVerifyClient="require" SSLEngine="on"/>
```

## Import certificates to the truststore

`"%JAVA_HOME%\bin\keytool.exe" -importcert -file "D:\Root_CA.crt" -keystore D:\tomcat\x509\keystore.jks -alias dtRoot`

`"%JAVA_HOME%\bin\keytool.exe" -importcert -file "D:\Issuing_CA.crt" -keystore D:\tomcat\x509\keystore.jks -alias dtInter`

`"%JAVA_HOME%\bin\keytool.exe" -importcert -file "D:\Root_CA.crt" -keystore D:\tomcat\x509\truststore.jks -alias dtRoot`

`"%JAVA_HOME%\bin\keytool.exe" -importcert -file "D:\Issuing_CA.crt" -keystore D:\tomcat\x509\truststore.jks -alias dtInter`

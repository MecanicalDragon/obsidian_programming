**проблема**: идея не может сбилдить проект, и просто висит в Gradle Build
**решение**: проблема в том, что нужно добавить сертификат в jdk. Проверить это можно, выполнив билд из консоли и увидев ошибку: `sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target (PKIX path building failed)`

[solution](https://magicmonster.com/kb/prg/java/ssl/pkix_path_building_failed/)

также нужно добавить пароль от стора в *gradle.properties*:
`systemProp.javax.net.ssl.trustStorePassword = changeit`

1. Configure maven-publish plugin ([how?](https://www.jetbrains.com/help/space/publish-artifacts-from-a-gradle-project.html))
2. Configure signing plugin ([how?](https://www.jetbrains.com/help/space/publish-artifacts-to-maven-central.html))
3. You need an account [here](https://central.sonatype.org/publish/publish-guide/#introduction). If you don’t own a domain name, you should use GitHub repo.
4. You need to configure [GnuPG](https://www.jetbrains.com/help/space/publish-artifacts-to-maven-central.html). It is better to generate it in Ubuntu and copy it to windows. Anyway, there will be problems with key location and formats. Useful commands:
	1. `gpg --export-secret-keys -o ~/secring.kbx` – for secretKeyRingFile
	2. `gpg --list-secret-keys --keyid-format short` – to know the short key format (you need to substring `ssb` after `/` anyway).
	3. `gpg –keyserver <key-server> –send-keys <your-pub-key>` – to upload key in short format to sonatype server. Upload key to all denoted in nexus repo gpg servers.
5. You [must](https://central.sonatype.org/publish/requirements/) provide sources-jar, javadoc-jar, fully filled pom during the publishing process.
6. [Release!](https://central.sonatype.org/publish/release/)


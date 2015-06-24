# Undertow-swarm error replication example application based on leafy hellowalrd example

get all the gems in place

	gem install jar-dependencies --development
    bundle install

## starting the server

### with undertow-swarm (described error)

		rmvn wildfly-swarm:run


#### stack trace

		[INFO] --- wildfly-swarm-plugin:1.0.0.Alpha3:run (default-cli) @ webservice ---
		Downloading: https://repo.maven.apache.org/maven2/org/apache/maven/maven-plugin-api/3.2.5/maven-plugin-api-3.2.5.jar
		Downloading: https://repo.maven.apache.org/maven2/org/apache/maven/maven-model/3.2.5/maven-model-3.2.5.jar
		Downloading: https://repo.maven.apache.org/maven2/org/apache/maven/maven-artifact/3.2.5/maven-artifact-3.2.5.jar
		Downloading: https://repo.maven.apache.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.3.0.M1/org.eclipse.sisu.plexus-0.3.0.M1.jar
		Downloading: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-classworlds/2.5.1/plexus-classworlds-2.5.1.jar
		Downloaded: https://repo.maven.apache.org/maven2/org/apache/maven/maven-plugin-api/3.2.5/maven-plugin-api-3.2.5.jar (46 KB at 225.1 KB/sec)
		Downloaded: https://repo.maven.apache.org/maven2/org/codehaus/plexus/plexus-classworlds/2.5.1/plexus-classworlds-2.5.1.jar (49 KB at 243.1 KB/sec)
		Downloaded: https://repo.maven.apache.org/maven2/org/apache/maven/maven-artifact/3.2.5/maven-artifact-3.2.5.jar (54 KB at 252.6 KB/sec)
		Downloaded: https://repo.maven.apache.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.3.0.M1/org.eclipse.sisu.plexus-0.3.0.M1.jar (196 KB at 722.7 KB/sec)
		Downloaded: https://repo.maven.apache.org/maven2/org/apache/maven/maven-model/3.2.5/maven-model-3.2.5.jar (158 KB at 493.8 KB/sec)
		Exception in thread "main" org.jboss.modules.ModuleLoadError: Error loading module from modules/org/jboss/invocation/main/module.xml
			at org.jboss.modules.ModuleLoadException.toError(ModuleLoadException.java:74)
			at org.jboss.modules.Module.getPathsUnchecked(Module.java:1384)
			at org.jboss.modules.Module.loadModuleClass(Module.java:555)
			at org.jboss.modules.ModuleClassLoader.findClass(ModuleClassLoader.java:197)
			at org.jboss.modules.ConcurrentClassLoader.performLoadClassUnchecked(ConcurrentClassLoader.java:455)
			at org.jboss.modules.ConcurrentClassLoader.performLoadClassChecked(ConcurrentClassLoader.java:404)
			at org.jboss.modules.ConcurrentClassLoader.performLoadClass(ConcurrentClassLoader.java:385)
			at org.jboss.modules.ConcurrentClassLoader.loadClass(ConcurrentClassLoader.java:130)
			at org.wildfly.swarm.runtime.container.RuntimeServer.<init>(RuntimeServer.java:44)
			at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
			at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
			at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
			at java.lang.reflect.Constructor.newInstance(Constructor.java:408)
			at java.lang.Class.newInstance(Class.java:438)
			at org.wildfly.swarm.container.Container.createServer(Container.java:85)
			at org.wildfly.swarm.container.Container.<init>(Container.java:51)
			at org.wildfly.swarm.Swarm.main(Swarm.java:25)
		Caused by: org.jboss.modules.xml.XmlPullParserException: Failed to resolve artifact 'org.jboss.invocation:jboss-invocation:1.4.1.Final' (position: END_TAG seen ...rtifact name="org.jboss.invocation:jboss-invocation:1.4.1.Final"/>... @32:77)
			at org.jboss.modules.ModuleXmlParser.parseArtifact(ModuleXmlParser.java:756)
			at org.jboss.modules.ModuleXmlParser.parseResources(ModuleXmlParser.java:650)
			at org.jboss.modules.ModuleXmlParser.parseModuleContents(ModuleXmlParser.java:446)
			at org.jboss.modules.ModuleXmlParser.parseDocument(ModuleXmlParser.java:261)
			at org.jboss.modules.ModuleXmlParser.parseModuleXml(ModuleXmlParser.java:148)
			at org.jboss.modules.ModuleXmlParserBridge.parseModuleXml(ModuleXmlParserBridge.java:17)
			at org.wildfly.swarm.bootstrap.modules.BootstrapClasspathModuleFinder.findModule(BootstrapClasspathModuleFinder.java:35)
			at org.jboss.modules.ModuleLoader.findModule(ModuleLoader.java:452)
			at org.jboss.modules.ModuleLoader.loadModuleLocal(ModuleLoader.java:355)
			at org.jboss.modules.ModuleLoader.preloadModule(ModuleLoader.java:302)
			at org.jboss.modules.Module.addPaths(Module.java:1028)
			at org.jboss.modules.Module.link(Module.java:1398)
			at org.jboss.modules.Module.getPaths(Module.java:1359)
			at org.jboss.modules.Module.getPathsUnchecked(Module.java:1382)
			... 15 more

### with jetty

    rmvn jetty:run

the urls:

    [http://localhost:8080/app](http://localhost:8080/app)
    [http://localhost:8080/admin](http://localhost:8080/admin)

### with tomcat or wildfly

	rmvn tomcat:run
	rmvn wildfly:run

the urls:

    [http://localhost:8080/hellowarld/app](http://localhost:8080/hellowarld/app)
    [http://localhost:8080/hellowarld/admin](http://localhost:8080/hellowarld/admin)

## configurations

* Mavenfile
* WEB-INF/web.xml

## run some integration test

    rmvn verify

or

    rake

with jruby-9k due to some bundler bug you need to run (not needed with rvm)

    BUNDLE_DISABLE_SHARED_GEMS=true rake

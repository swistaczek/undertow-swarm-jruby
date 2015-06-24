#-*- mode: ruby -*-

project do
  artifact_id 'webservice'
  group_id    'com.example'
  version     '0.0.1'

  properties( 'project.build.sourceEncoding' => 'utf-8',
              'jruby.version'                => '1.7.20' )

  packaging :war

  dependencies do
    # Required ruby deps
    gem! 'bundler', '1.10.3'
    gem! 'jar-dependencies', '0.1.15'

    # Required java deps
    jar( 'org.jruby.rack:jruby-rack', '1.1.18',
         exclusions: [ 'org.jruby:jruby-complete' ] )
    jar( 'org.wildfly.swarm:wildfly-swarm-undertow', '1.0.0.Alpha3' )

    # Setup external dependencies managers
    gemfile
    jarfile if File.exists?('Jarfile.lock')
  end
end

pom 'org.jruby:jruby', '${jruby.version}'

resource do
  directory '${basedir}'
  includes [ 'app/**', 'config.ru' ]
end

resource do
  directory '${basedir}'
  includes [ 'config.ru' ]
  target_path 'WEB-INF'
end

build do
  final_name File.basename( File.expand_path( '.' ) )
  directory 'pkg'
end

plugin( :war, '2.2',
        webAppSourceDirectory: "${basedir}",
        webXml: 'WEB-INF/web.xml',
        webResources: [ { directory: '${basedir}',
                             targetPath: 'WEB-INF',
                             includes: [ 'config.ru' ] } ] )

# integration tests
###################

require 'open-uri'
results = []
execute 'download', phase: 'integration-test' do
  results << open( 'http://localhost:8080/hellowarld/app' ).string
  results << open( 'http://localhost:8080/hellowarld/admin' ).string
  results << open( 'http://localhost:8080/hellowarld/admin/ping' ).string
  results << open( 'http://localhost:8080/hellowarld/admin/health' ).string
  results << open( 'http://localhost:8080/hellowarld/admin/metrics' ).string
  results << open( 'http://localhost:8080/hellowarld/admin/threads' ).read
  results << open( 'http://localhost:8080/hellowarld/ping' ).string
  results << open( 'http://localhost:8080/hellowarld/health' ).string
  results << open( 'http://localhost:8080/hellowarld/metrics' ).string
  results << open( 'http://localhost:8080/hellowarld/threads' ).read
  results.each { |r| puts r[0..20] }
end

# verify the downloads
require 'json'
execute 'verify downloads', phase: :verify do
  sleep 0.5 #let jetty shut down in peace
  expected = 'christian'
  unless results[0].match( /#{expected}/ )
    raise "missed expected string in download: #{expected}"
  end
  expected = 'menu'
  unless results[1].match( /#{expected}/ )
    raise "missed expected string in download: #{expected}"
  end
  expected = 'pong'
  unless results[2].match( /#{expected}/ )
    raise "missed expected string in download: #{expected}"
  end
  json = JSON.parse( results[3] )
  unless json["app.health"]["healthy"]
    raise "healthy expected"
  end
  json = JSON.parse( results[4] )
  unless json["meters"]["webapp.responseCodes.ok"]["count"] == 1
    raise "one OK request expected"
  end
  unless results[5].length > 10000
    puts result[5]
    raise "expected thread dump to be big"
  end
  expected = 'pong'
  unless results[6].match( /#{expected}/ )
    raise "missed expected string in download: #{expected}"
  end
  json = JSON.parse( results[7] )
  unless json["app.health"]["healthy"]
    raise "healthy expected"
  end
  json = JSON.parse( results[8] )
  unless json["meters"]["webapp.responseCodes.ok"]["count"] == 1
    raise "one OK request expected"
  end
  unless json["meters"]["collected.responseCodes.2xx"]["count"] == 1
    raise "one 2xx request expected"
  end
  unless results[9].length > 10000
    puts result[9]
    raise "expected thread dump to be big"
  end
end

# jetty runner
##############

#plugin( 'org.mortbay.jetty:jetty-maven-plugin', '8.1.14.v20131031',
#        webAppSourceDirectory: "${basedir}" )
plugin( 'org.eclipse.jetty:jetty-maven-plugin', '9.3.0.M1',
        webAppSourceDirectory: "${basedir}",
        webApp: { contextPath: '/hellowarld' },
        stopPort: 9999,
        stopKey: 'foo' ) do
   execute_goal( 'start', id: 'start jetty', phase: 'pre-integration-test', daemon: true )
   execute_goal( 'stop', id: 'stop jetty', phase: 'post-integration-test' )
end


# tomcat runner
###############
plugin( 'org.codehaus.mojo:tomcat-maven-plugin', '1.1',
        warSourceDirectory: '${basedir}' )

# wildfly runner
################
plugin( 'org.wildfly.plugins:wildfly-maven-plugin:1.0.2.Final' )
plugin( 'org.wildfly.swarm:wildfly-swarm-plugin:1.0.0.Alpha3' )

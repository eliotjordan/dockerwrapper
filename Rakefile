require 'rake'
require 'ipaddress'
require 'httparty'

namespace :loris do
  desc "Build Loris docker"
  task :build do
    sh 'docker build -t "loris/loris" loris/.'
  end

  desc "Run Loris docker"
  task :run do
    sh <<-SHELL
      docker run -d \
        --name dockerwrapper \
        -v $(pwd)/loris/images:/usr/local/share/images \
        -p 5004:5004 \
        loris/loris
      docker exec -it \
        dockerwrapper \
        cp -r /opt/loris/tests/img/. /usr/local/share/images/
    SHELL
  end

  desc "Stop Loris docker"
  task :stop do
    sh 'docker rm -f dockerwrapper'
  end

  desc "Clean Loris docker"
  task :clean do
    # clean
  end

  desc "Test Loris endpoint"
  task :test do
    name = `(docker info | grep 'Name: ' | sed 's/Name: //')`
    if name['boot2docker']
      host = `boot2docker ip`
    else
      host = 'localhost'
    end
    response = HTTParty.get('http://' + host.strip + ':5004/01/02/0001.jp2/info.json')
    puts response.body
  end
end

require 'rake'
require 'httparty'
require 'ipaddress'

namespace :loris do
  desc "Build Loris docker"
  task :build do
    sh 'docker build -t "pulibrary/loris" loris/.'
  end

  desc "Run Loris docker"
  task :run do
    sh <<-SHELL
      docker run -d \
        --name dockerwrapper \
        -v $(pwd)/loris/images:/usr/local/share/images \
        -p 3000:3000 \
        pulibrary/loris
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
    host_url = IPAddress::IPv4::extract ENV['DOCKER_HOST']
    response = HTTParty.get('http://' + host_url.to_s + ":3000/01/02/0001.jp2/info.json")
    puts response.body
  end
end

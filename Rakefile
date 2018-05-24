begin
  require "bundler/gem_tasks"
rescue LoadError
  puts "bundler not installed, use 'gem install bundler' to install"
end

begin
  require "rspec/core/rake_task"
  RSpec::Core::RakeTask.new(:spec)
  task :default => :spec
rescue LoadError
  nil
end

namespace :docker do
  base_image = 'ciriethereum/base'

  desc 'pull base docker image'
  task :pull_base do
    system("docker pull #{base_image}:latest")
  end

  desc 'build base docker image, rerun this task after updated Gemfile or Dockerfile'
  task :build_base do
    system("docker build . -f docker/Base -t #{base_image}:latest")
  end

  desc 'open Ciri develop container shell'
  task :shell do
    container_name = 'ciri-develop'
    if system("docker inspect #{container_name} > /dev/null")
      system("docker start -i #{container_name}")
    else
      puts "start a new develop container: #{container_name}"
      system("docker run -v `pwd`:/app -it --name #{container_name} #{base_image}:latest bash")
    end
  end

  desc 'run tests in docker'
  task :test do
    system("docker run -v `pwd`:/app --rm #{base_image}:latest rake")
  end
end

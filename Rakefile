desc 'run test suite'
task :test do
  Dir.foreach("/var/lib/puppet/yaml/node/") { |node|
    next if node == '.' or node == '..'
    exit 1 unless system("ruby manitest.rb \
      --confdir ~/puppet \
      --node /var/lib/puppet/yaml/node/#{node}")
  }
end

namespace :hudson do
  jobdir = "/var/lib/hudson/jobs/Puppet"

  desc 'continuous integration task'
  task :ci => [:pull_nodes] do
    Dir.foreach("#{jobdir}/node") { |node|
      next if node == '.' or node == '..'
      exit 1 unless system("ruby manitest.rb \
        --confdir #{jobdir}/workspace \
        --node #{jobdir}/node/#{node}")
    }
  end

  desc 'pull current node specs'
  task :pull_nodes do
    exit 1 unless system("rsync -a puppet@puppet:/var/lib/puppet/yaml/node \
      #{jobdir}")
  end
end

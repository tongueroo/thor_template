# require "bundler/gem_tasks"
require "rspec/core/rake_task"

task :default => :spec

RSpec::Core::RakeTask.new

# DELETE AFTER USING
desc "Rename project"
task :rename do
  name = ENV['NAME'] || File.basename(Dir.pwd)
  camelize = lambda do |str|
    str.to_s.gsub(/\/(.?)/) { "::#{$1.upcase}" }.gsub(/(?:^|_)(.)/) { $1.upcase }
  end
  dir = Dir['**/thor_template*']
  begin
    from = dir.pop
    if from
      to = from.split('/')
      to[-1].gsub!('thor_template', name)
      FileUtils.mv(from, to.join('/'))
    end
  end while dir.length > 0
  Dir["**/*"].each do |path|
    if File.file?(path)
      `sed -i 's/thor_template/#{name}/g' #{path}`
      `sed -i 's/ThorTemplate/#{camelize.call(name)}/g' #{path}`
      no_space = File.read(path).gsub(/\s+\z/, '')
      File.open(path, 'w') { |f| f.write(no_space) }
    end
  end
end

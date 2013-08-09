require 'rdoc/task'

versions = %w(trunk ruby_2_0_0 ruby_1_9_3 ruby_1_8_7)
versions.each do |version|
  desc "Checks out the source for #{version}"
  task "source:#{version}" do
    sh "git clone --depth=1 --branch=#{version} git://github.com/ruby/ruby.git sources/#{version}" do |ok, res|
      next unless ok
    end
  end

  desc "Updates the source for #{version}"
  task "update:#{version}" => "source:#{version}" do
    sh "cd sources/#{version} && git pull origin #{version}"
  end

  namespace :rdoc do
    RDoc::Task.new version do |rdoc|
      rdoc.title = "Documentation for #{version.capitalize.gsub('_',' ')}"
      rdoc.main = "#{version}/README"
      rdoc.rdoc_dir = version
      rdoc.rdoc_files << "sources/#{version}"
      rdoc.rdoc_files << "sources/#{version}/README"
      rdoc.options << "-U"
      rdoc.options << "--root=sources/#{version}"
    end
  end
  task "rdoc:#{version}" => "update:#{version}"
end

desc "Checks out all sources for the following versions: #{versions.join(', ')}"
task "source:all" => versions.map { |v| "source:#{v.to_sym}" }

desc "Updates the sources for the following versions: #{versions.join(', ')}"
task "update:all" => %w[source:all] + versions.map { |v| "update:#{v.to_sym}" }

desc "Build RDoc HTML files for the following versions: #{versions.join(', ')}"
task "rdoc:all" => %w[update:all] + versions.map { |v| "rdoc:#{v.to_sym}" }

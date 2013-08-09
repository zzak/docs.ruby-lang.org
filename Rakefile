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
      root = "sources/#{version}"

      rdoc.title = "Documentation for #{version.capitalize.gsub('_',' ')}"
      rdoc.main = "README"
      rdoc.rdoc_dir = version
      rdoc.rdoc_files << root
      rdoc.rdoc_files << "#{root}/README"
      rdoc.options << "-U"
      rdoc.options << "--all"
      rdoc.options << "--root=#{root}"
      rdoc.options << "--page-dir=#{root}/doc"
      rdoc.options << "--encoding=UTF-8"
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

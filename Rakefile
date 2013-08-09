require 'rdoc/task'

versions = %w(trunk ruby_2_0_0 ruby_1_9_3 ruby_1_8_7)
versions.each do |version|
  source_dir = "sources/#{version}"

  directory source_dir do
    sh "git clone --depth=1 --branch=#{version} git://github.com/ruby/ruby.git #{source_dir}"
  end

  desc "Checks out source for #{version}"
  task "source:#{version}" => source_dir

  desc "Updates source for #{version}"
  task "update:#{version}" => source_dir do
    Dir.chdir source_dir do
      sh "git pull origin #{version}"
    end
  end

  namespace :rdoc do
    RDoc::Task.new version do |rdoc|
      rdoc.title = "Documentation for #{version.capitalize.gsub('_',' ')}"
      rdoc.main = "README"
      rdoc.rdoc_dir = version
      rdoc.rdoc_files << source_dir
      rdoc.options << "-U"
      rdoc.options << "--all"
      rdoc.options << "--root=#{source_dir}"
      rdoc.options << "--page-dir=#{source_dir}/doc"
      rdoc.options << "--encoding=UTF-8"
    end
  end
  task "rdoc:#{version}" => "update:#{version}"
end

desc "Checks out sources for all versions"
task "source" => versions.map { |v| "source:#{v}" }

desc "Updates sources for all versions"
task "update" => versions.map { |v| "update:#{v}" }

desc "Build RDoc HTML files for all versions"
task "rdoc" => versions.map { |v| "rdoc:#{v}" }


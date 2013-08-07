require 'rdoc/task'

versions = %w(trunk ruby_2_0_0 ruby_1_9_3 ruby_1_8_7)
versions.each do |version|
  namespace :source do
    desc "Checks out the source for #{version}"
    task version do
      sh "git clone --branch=#{version} git://github.com/ruby/ruby.git sources/#{version}" do |ok, res|
        next unless ok
      end
    end
  end

  namespace :update do
    desc "Updates the source for #{version}"
    task version do
      sh "cd sources/#{version} && git fetch origin && git rebase remotes/origin/#{version}"
    end
  end

  namespace :rdoc do
    RDoc::Task.new version do |rdoc|
      rdoc.title = "Documentation for #{version.capitalize.gsub('_',' ')}"
      rdoc.main = "#{version}/README"
      rdoc.rdoc_dir = version
      rdoc.rdoc_files << "sources/#{version}"
      rdoc.rdoc_files << "sources/#{version}/README"
      rdoc.options << "-U"
    end
  end
end

namespace :source do
  desc "Checks out all sources for the following versions: #{versions.join(', ')}"
  task :all => versions.map { |v| v.to_sym }
end

namespace :update do
  desc "Updates the sources for the following versions: #{versions.join(', ')}"
  task :all => versions.map { |v| v.to_sym }
end

namespace :rdoc do
  desc "Build RDoc HTML files for the following versions: #{versions.join(', ')}"
  task :all => versions.map { |v| v.to_sym }
end

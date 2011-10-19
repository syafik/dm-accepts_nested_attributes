source 'http://rubygems.org'

SOURCE         = ENV.fetch('SOURCE', :git).to_sym
REPO_POSTFIX   = SOURCE == :path ? ''                                : '.git'
DATAMAPPER     = SOURCE == :path ? Pathname(__FILE__).dirname.parent : 'http://github.com/datamapper'
DM_VERSION     = '~> 1.2.0'
DM_UVERSION    = '< 1.3'
DO_VERSION     = '~> 0.10.3'
DM_DO_ADAPTERS = %w[ sqlite postgres mysql oracle sqlserver ]

gem 'dm-core', DM_VERSION, DM_UVERSION

group :development do

  gem 'dm-validations',  DM_VERSION, DM_UVERSION
  gem 'dm-constraints',  DM_VERSION, DM_UVERSION

  gem 'rake',      '~> 0.8.7'
  gem 'rspec',     '~> 1.3'
  gem 'yard',      '~> 0.5'
  gem 'jeweler'

end

group :quality do
  gem 'yardstick', '~> 0.1', :platforms => :mri_18
end

group :datamapper do

  adapters = ENV['ADAPTER'] || ENV['ADAPTERS']
  adapters = adapters.to_s.tr(',', ' ').split.uniq - %w[ in_memory ]

  if (do_adapters = DM_DO_ADAPTERS & adapters).any?
    do_options = {}
    do_options[:git] = "#{DATAMAPPER}/do#{REPO_POSTFIX}" if ENV['DO_GIT'] == 'true'

    gem 'data_objects', DO_VERSION, do_options.dup

    do_adapters.each do |adapter|
      adapter = 'sqlite3' if adapter == 'sqlite'
      gem "do_#{adapter}", DO_VERSION, do_options.dup
    end

    gem 'dm-do-adapter', DM_VERSION, DM_UVERSION
  end

  adapters.each do |adapter|
    gem "dm-#{adapter}-adapter", DM_VERSION, DM_UVERSION
  end

  plugins = ENV['PLUGINS'] || ENV['PLUGIN']
  plugins = plugins.to_s.tr(',', ' ').split.push('dm-migrations').uniq

  plugins.each do |plugin|
    gem plugin, DM_VERSION, DM_UVERSION
  end

end

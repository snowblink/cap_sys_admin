# Usage:
#  cap update_servers -Sservers=qa # if you are using cap 2.0.0

role :dev, *%w(
user@dev_machine1
user@dev_machine2
user@dev_machine3
)

role :qa, *%w(
user@qa_machine1
user@qa_machine2
user@qa_machine3
)

role :live, *%w(
user@live_machine1
user@live_machine2
user@live_machine3
)

abort "Needs capistrano 2." unless respond_to?(:namespace)

set :user, 'user' unless exists?(:user)
set :servers, 'dev_machine1' unless exists?(:servers)

apply_to = servers.split(/,/).map{|s| s.to_sym}

def confirm(to_do)
  puts "Please confirm you wish to #{to_do}: "
  servers.split(/,/).each do |s|
    puts s
    roles[s.to_sym].each {|h| puts "  #{h.host}"}
  end
  print "(y/n) "
  if $stdin.gets.chomp =~ /^[Yy](es)?$/
    yield
  else
    puts "Not proceeding with #{to_do}"
  end
end

desc "Update servers"
task :update_servers, :roles => apply_to do
  confirm('update') do
    sudo "apt-get update"
    sudo "apt-get -y upgrade"
  end
end

desc "Dist upgrade servers"
task :dist_upgrade, :roles => apply_to do
  confirm('dist upgrade') do
    sudo "apt-get update"
    sudo "apt-get -y dist-upgrade"
  end
end

desc "Restart servers"
task :restart, :roles => apply_to do
  confirm('REBOOT') do
    sudo "reboot"
  end
end

desc "Info"
task :info, :roles => apply_to do
  run "uname -a"
  run "lsb_release -a"
  run "uptime"
  run "lspci -vv"
end

desc "Displays current date time"
task :date_time, :roles => apply_to do
  run "date"
end

desc "Uname"
task :uname, :roles => apply_to do
  run "uname -a"
end

desc "Halt servers"
task :halt, :roles => apply_to do
  confirm('HALT') do 
    sudo "halt"
  end
end

desc "Uptime"
task :uptime, :roles => apply_to do
  run "uptime"
end

desc "Update Gems"
task :update_gems, :roles => apply_to do
  confirm('UPDATE GEMS') do
    sudo "gem update"
  end
end
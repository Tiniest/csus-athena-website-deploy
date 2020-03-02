desc "Deploy to Athena"
task :deploy do
  puts "Deploying to #{ENV['ATHENA_USER']}@athena.ecs.csus.edu/html"
  status = system("rsync -avze 'ssh' --delete src/ #{ENV['ATHENA_USER']}@athena.ecs.csus.edu:/gaia/class/student/#{ENV['ATHENA_USER']}/html/")
  if status
    puts "Deployed Successfully"
  else
    puts "Deploy Failed"
    puts status
  end
end

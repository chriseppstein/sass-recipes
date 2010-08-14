task :default do
  sh "compass compile"
  FileList.new('**/*.haml').each do |file|
    puts "Compiling #{file} to html"
    sh "haml #{file} #{file.sub(/haml/,'html')}"
  end
end
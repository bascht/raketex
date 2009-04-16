PROJECT_NAME = "thesis"
namespace :dir do
  namespace :pyg do
    desc "Pygmentizes all files in the directory to latex"
    task :latex do
      Dir.glob('**/*.{rb,xml,yml,php,html,java}').each do |filename|
        puts "pygmentizing #{File.basename(filename)}"
        `pygmentize -O'style=emacs,linenos=1,encoding=utf-8' -f latex #{filename} > #{filename + '.tex'}`
      end
    end
  end
  namespace :dot do
    desc "Render all images"
    task :png do
      Dir.glob('**/*.{dot,circo,dia}').each do |filename|
        directory = File.dirname(filename) + '/'
        extension = File.extname(filename)
        basename  = File.basename(filename, extension)
        puts "Rendering #{filename}..."
        `dot #{filename} -Tpdf -o #{directory + basename}.pdf` if File.extname(filename) == '.dot'
        `circo #{filename} -Tpdf -o #{directory + basename}.pdf` if File.extname(filename) == '.circo'
        `dia --filter=eps -e #{filename}.eps #{filename}` if File.extname(filename) == '.dia'
     end
    end
  end
end
namespace :latex do
  task :all => [:index, :bib, :compile]
  desc "Counts words of main document"
  task :count do
    puts "#{`detex #{PROJECT_NAME} | wc -w`.strip} words in thesis"
    if (file = ENV["file"])
      puts "#{`detex #{file} | wc -w`.strip} words in #{file}"
    end
  end
  desc "Generates the Index"
  task :index do
    puts "Generating Index for #{PROJECT_NAME}."
    `makeindex -s #{PROJECT_NAME}.ist -t #{PROJECT_NAME}.alg -o #{PROJECT_NAME}.acr #{PROJECT_NAME}.acn`
    `makeindex -s #{PROJECT_NAME}.ist -t #{PROJECT_NAME}.glg -o #{PROJECT_NAME}.gls #{PROJECT_NAME}.glo`
  end
  desc "Generating BibTeX"
  task :bib do
    puts "Generating BibTeX for #{PROJECT_NAME}."
    `bibtex #{PROJECT_NAME}`
  end
  desc "Compile LaTeX"
  task :compile do
    puts "Compiling #{PROJECT_NAME}."
    `pdflatex #{PROJECT_NAME}`
  end
  desc "Generate PDF File"
  task :pdf do
    puts "Generating PDF for #{PROJECT_NAME}."
    `pdflatex #{PROJECT_NAME}.dvi`
  end
  desc "Count PDF Pages"
  task :pages do
    puts "Pages for Project #{PROJECT_NAME}:"
    puts `pdfinfo #{PROJECT_NAME}.pdf|grep Pages`
  end

end

desc "Grep out the TODO's"
task :todo do
  puts "\n** Whats left to do for #{PROJECT_NAME} **\n"
  puts `grep -n %TODO *.tex */*.tex`
end

desc "All (dot, tex, pdf)"
task :default => ["dir:pyg:latex", "dir:dot:png", "latex:all", "todo"]

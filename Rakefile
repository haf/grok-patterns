require 'bundler/setup'
require 'grok-pure'

def run patterns, input, sample
  grok = Grok.new
  patterns.each do |p|
    grok.add_patterns_from_file p
  end
  grok.compile input

  match = grok.match sample
  if match
    $stdout.puts "."
  else
    $stderr.puts "Err(input): #{input}"
    $stderr.puts "Err(sample): #{sample}"
  end
end

task :specs do
  patterns = Dir.glob 'patterns/*'
  subjects = Dir.glob('groks/*').reject { |f| f.end_with? '-sample' }
  subjects.each do |subject|
    grok_filter = File.open(subject, 'rb') do |io|
      io.read
    end
    grok_filter_input = File.open("#{subject}-sample", 'rb') do |io|
      io.read
    end

    $stdout.puts "running test on grok filter: #{subject}"
    run patterns, grok_filter, grok_filter_input
  end
end
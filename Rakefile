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

desc 'for each file in "groks/" test with {filename}-sample'
task :specs do
  patterns = Dir.glob 'patterns/*'
  subjects = Dir.glob('groks/*').reject { |f| f.end_with? '-sample' }
  subjects.each do |subject|
    grok_filter = File.open(subject, 'rb') do |io|
      io.read
    end
    Dir.glob "#{subject}-sample*" do |sample_file|
      grok_filter_input = File.open(sample_file, 'r') do |io|
        io.read
      end

      $stdout.puts "running test on grok filter: #{subject} with sample #{sample_file}"
      run patterns, grok_filter, grok_filter_input
    end
  end
end

def template_grok pattern, fields
%{
  grok {
    patterns_dir => "/etc/logstash/patterns"
    match => [
      "message", "#{pattern.gsub /"/, '\"'}"
    ]
    add_field    => #{fields.inspect}
    add_tag      => ["grokked"]
  }
}
end

desc 'create logstash.conf from the groks/-files, cp into vagrant-elk-box'
task :prepare do
  subjects =
    Dir.glob('groks/*').
    reject { |f| !!(f =~ /-sample.*/) }. # proof ruby isomorphic to perl
    map { |file| [file, File.open(file, 'rb') { |io| io.read } ]  }

  filters =
    subjects.
      map { |file, data| template_grok data, ["grok_type", File.basename(file)]}.
      join("\n")

  File.open('vagrant-elk-box/confs/logstash/logstash.conf', 'w+') do |io|
    io.write %{
input { stdin {} }
filter {
  #{filters}
  if "grokked" in [tags] {
    mutate {
      remove_tag   => ["_grokparsefailure"]
    }
  }
  if [audit_execve_rest] {
    kv {
      prefix       => "audit_execve_"
      source       => "audit_execve_rest"
      trim         => '"'
      remove_field => "audit_execve_rest"
    }
  }
}
output { stdout { codec => json_lines } }
}
  end
end
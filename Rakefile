FLAC_DIR = 'flac'.freeze
MP3_DIR = 'mp3'.freeze
XLD_CMD = 'xld'.freeze

flac_files = FileList["#{FLAC_DIR}/**/*.flac"]
mp3_files = flac_files.pathmap("#{MP3_DIR}/%-2d/%n.mp3")
artists = FileList["#{FLAC_DIR}/*"]

task :default do
  p flac_files.size
  p mp3_files[0]
  p artists

  print escape('hoge"hage"hige')
end


def escape(str)
  str.gsub(/"/, '\"')
end




desc 'Convert All Files'
task 'convert' => mp3_files

artists.each do |a|
  name = a.pathmap('%n').unicode_normalize
  mp3_files_by_artist = mp3_files.select { |f| f =~ %r{^#{MP3_DIR}/#{name}} }

  desc "Convert Folder: #{name}"
  task "conv_#{name}" => mp3_files_by_artist
  task "check_#{name}" do
    p mp3_files_by_artist
  end
end

rule %r{^#{MP3_DIR}/.+\.mp3} => "%{^#{MP3_DIR},#{FLAC_DIR}}X.flac" do |t|
  output_path = t.name.pathmap('%d')
  mkdir_p output_path
  sh "#{XLD_CMD} -f mp3 -o \"#{output_path}\" \"#{escape(t.source)}\""
end

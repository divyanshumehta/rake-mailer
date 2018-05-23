require 'mail'
require 'csv'

task :default => :send_mail
task :send_mail => :create_custom_messages do

  user = YAML.load_file('secret_stuff.yml')

  options = {
    :address              => "smtp.gmail.com",
    :port                 => 587,
    :domain               => 'smtp.gmail.com',
    :user_name            => user['email'],
    :password             => user['password'],
    :authentication       => 'plain',
    :enable_starttls_auto => true  }


  Mail.defaults do
    delivery_method :smtp, options
  end

  # Reading all users
  csv_text = File.read('db.csv')
  csv = CSV.parse(csv_text, :headers => true)
  csv.each_with_index do |row,index|
    dest = row.to_hash
    mail = Mail.new do
      from    user['email']
      to      dest['email']
      subject 'e-certificate - Workshop on FOSS NIT Durgapur'
      body    File.read('messages/'+dest['name']+index.to_s+'.txt')
      add_file 'certi_generate/certipdfs/'+dest['name']+index.to_s+'.pdf'
    end
    # puts mail
    if mail.deliver!
      puts dest['name']+" MAILED "
    else
      puts "Invalid email"
    end
  end

end

task :create_custom_messages do
  csv_text = File.read('db.csv')
  data = CSV.parse(csv_text, :headers => true)
  data.each_with_index do |row,index|
    dest = row.to_hash
    message = File.read('message.txt')
    new_contents = message.gsub(/NAME/, dest['name'])
    File.open('messages/'+dest['name']+index.to_s+'.txt', "w") {|file| file.puts new_contents }
  end
end

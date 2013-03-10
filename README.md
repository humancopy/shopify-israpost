israpost
========

Israel Post shipping rates class and API. Made with Ruby, YAML and Sinatra (no DB).

How to use API
--------------

```ruby
require "net/http"
require "uri"
require 'json'

# I think I'll call my next son "JSON" (and my girl Ruby).

address = "http://israpost.herokuapp.com/get-rate"
params = {:country=>"japan", :weight=>350}
uri = URI.parse(address)
uri.query = URI.encode_www_form(params)
response = Net::HTTP.get_response(uri)

if response.code.to_i==200
  my_hash = JSON.load(response.body)
  puts my_hash.collect {|k,v| "#{k} => #{v}\n" }
  # This is the kind of result you get:
  # airmail_group => 3
  # ems_group => 5
  # name_calculated => JAPAN
  # israel_post_name => יפן 
  # air_parcel_group => 3
  # appear_in_shipping_list => 1
  # country_code => JP
  # common_name => 
  # official_name_english => JAPAN
  # airmail => 14.9
  # air_parcel => 89.5
  # ems => 91.0
  # cost => 14.9
  # weight => 350
  # country => JAPAN
  # delivery_method => airmail
  # parcel => false
else
  puts "Error, code #{response.code}."
  puts response.body
end
```

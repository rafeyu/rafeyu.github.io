---
layout: post
title: "Ruby Strange Problem I Found"
modified: 2015-05-26 15:19:23 +0700
tags: [ruby,problem]
category: ruby
image:
  feature: 
  credit: 
  creditlink: 
comments: true
share: true
---

I use Ruby 2.0.0 and I found "strange" error when I didn't use "https" on my http request.

This was my first code, working fine but didn't cool:

    uri = URI(my_url)
    http = Net::HTTP.new(uri.host, uri.port)
    http.use_ssl = true
    request = Net::HTTP::Post.new(uri.request_uri, headers)
    request.set_form_data(form_post) unless form_post.nil?
    response = http.request(request)

My second code, seems cool, but I tried to not `use_ssl`:

    uri = URI(my_url)
    request = Net::HTTP::Post.new(uri.request_uri, headers)
    request.set_form_data(form_post) unless form_post.nil?
    res = Net::HTTP.start(uri.hostname, uri.port) do |http|
        http.request(request)
    end

Taraa, the problem came

    /usr/lib64/ruby/2.0.0/net/protocol.rb:153:in `read_nonblock': end of file reached (EOFError)
    	from /usr/lib64/ruby/2.0.0/net/protocol.rb:153:in `rbuf_fill'
    	from /usr/lib64/ruby/2.0.0/net/protocol.rb:134:in `readuntil'
    	from /usr/lib64/ruby/2.0.0/net/protocol.rb:144:in `readline'
    	from /usr/lib64/ruby/2.0.0/net/http/response.rb:39:in `read_status_line'
    	from /usr/lib64/ruby/2.0.0/net/http/response.rb:28:in `read_new'
    	from /usr/lib64/ruby/2.0.0/net/http.rb:1412:in `block in transport_request'
    	from /usr/lib64/ruby/2.0.0/net/http.rb:1409:in `catch'
    	from /usr/lib64/ruby/2.0.0/net/http.rb:1409:in `transport_request'
    	from /usr/lib64/ruby/2.0.0/net/http.rb:1382:in `request'
    	from ./urimg:45:in `block in my_function'
      from /usr/lib64/ruby/2.0.0/net/http.rb:852:in `start'
    	from /usr/lib64/ruby/2.0.0/net/http.rb:582:in `start'
    	from ./urimg:43:in `my_function'
    	from ./urimg:93:in `<main>

Guess what?? Just add an `use_ssl`, the problem gone!

    res = Net::HTTP.start(uri.hostname, uri.port, :use_ssl => uri.scheme == 'https') do |http|

That's why I called it "strange", because the problem should not look like that.

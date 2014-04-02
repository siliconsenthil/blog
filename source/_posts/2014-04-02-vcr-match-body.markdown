---
layout: post
title: "VCR - Match body"
date: 2014-04-02 18:50
comments: true
categories: [rails, rspec, vcr]
keywords: rails,rspec,vcr
description: "How to write a custom matcher for VCR to match http post body"
---

We had to have make external API calls and we used [vcr](https://github.com/vcr/vcr) to record and replay in our integration tests. All was going on well.

Despite this coverage, we saw our code breaking in production due to small chages in request xml body. We wanted our integration spec to cover these too. By default, vcr [matches](https://www.relishapp.com/vcr/vcr/v/2-0-0-beta1/docs/request-matching) request method and url. We wanted it to match the post body too. Simple match by body does not solve as it does plain string match. It would give [false negatives](http://stackoverflow.com/a/17838775/227705).

So, we decided to match our xml request body. This is what we did.

<!-- more -->

``` ruby Gemfile
gem 'equivalent-xml'
```

``` ruby spec/spec_helper.rb
VCR.configure do |c|
  #some other configs
  c.default_cassette_options = {record: :once, match_requests_on:  [:method, :uri, :xml_post_body]}
  c.register_request_matcher :xml_post_body do |request_1, request_2|
    XmlPostBodyMatcher.match?(request_1, request_2)
  end
end
```

``` ruby spec/support/xml_post_body_matcher.rb
class XmlPostBodyMatcher
  def self.match?(request_1, request_2)
    return true if request_1.method == :get
    return true if !xml_body?(request_1)
    EquivalentXml.equivalent?(Nokogiri::XML(request_1.body), Nokogiri::XML(request_2.body))
  end
end
```

We added a new matcher called <code>XmlPostBodyMatcher</code> and it uses <code>equivalent-xml</code> gem to match if the post body is an xml.

This has helped us to have strict-yet-sensible  integration spec and increased our confidence on the test suite.

Hope this is useful for you too.
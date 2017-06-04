[ ![Codeship Status for BookerSoftwareInc/frederick_api_gem](https://app.codeship.com/projects/43a5ea40-2b13-0135-7e95-4afd89638027/status?branch=master)](https://app.codeship.com/projects/224007)

```text
 _____             _           _      _         _    ____ ___
|  ___| __ ___  __| | ___ _ __(_) ___| | __    / \  |  _ \_ _|___
| |_ | '__/ _ \/ _` |/ _ \ '__| |/ __| |/ /   / _ \ | |_) | |/ __|
|  _|| | |  __/ (_| |  __/ |  | | (__|   <   / ___ \|  __/| |\__ \
|_|  |_|  \___|\__,_|\___|_|  |_|\___|_|\_\ /_/   \_\_|  |___|___/
```


This gem provides a client for Frederick's V2 APIs.

Note: Our V2 APIs have not yet been released for use by customers or partners. See
[Frederick Developers](https://developers.hirefrederick.com) for supported APIs and documentation.

## Installation

Put this in your Gemfile:

```ruby
gem 'frederick_api'
```

You're now ready to go with Frederick's v2 API!

### Configuring FrederickAPI

You can use `FrederickAPI.configure` or environment variables
to configure the Frederick API client.

```ruby
# config/initializers/frederick_api.rb
...
FrederickAPI.configure do |c|
  c.base_url = 'https://api.hirefrederick.com'
  c.api_key = '1234-5678-1234-5678-1234-5678'
end
...
```

Environment variables can also be used:
  * `FREDERICK_API_BASE_URL`: Same as `base_url` above
  * `FREDERICK_API_KEY`: Same as `api_key` above
  
Environments:
  * For testing (default), use `FREDERICK_API_BASE_URL = https://api.staging.hirefrederick.com`
  * For production, use `FREDERICK_API_BASE_URL = https://api.hirefrederick.com`
  
NOTE: You must specify the production base URL of `https://api.hirefrederick.com` in order to use this gem with
Frederick's production API.

## Usage

Frederick V2 Resources correspond to ([JSON API](http://jsonapi.org/) compatible) APIs and use the
[json_api_client](https://github.com/chingor13/json_api_client) gem under the hood, so provide access
to standard "ActiveRecord-like" functionaliy such as `.create`, `.find`, `.where`, `.order`, `.includes` to create, find,
filter, sort, and include relationships.

### Access Tokens

An access token is required to access resources on behalf of a use. Use `Resource.with_access_token { ... }` to make
requests with an access token.


```ruby
# Fetch a location
id = '6fdf0530-3e4e-46f1-9d11-5f90c48a50dc'
location = FrederickAPI::V2::Location.find(id)
=> #<FrederickAPI::V2::Location:0x007fd2f29a7618>

location.name
=> 'Bizzy Biz'

# Update a location
location.name = 'Biz Bizziest'
location.save
=> true

# To instantiate a resource for update without fetching it first, set an id
location = FrederickAPI::V2::Location.new(id: id)
location.update_attributes(phone_number: '(555) 555-5555')
=> true
```
